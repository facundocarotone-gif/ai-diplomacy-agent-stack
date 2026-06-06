# Security Model

This document describes security controls for a single-operator agentic stack that ingests untrusted content (web pages, documents, messages), maintains a private knowledge base, and can assist with code and deployments.

It is written to be **auditable**: each control names the threat it addresses and the intended enforcement mechanism, not just a principle.

The governing rule is simple and load-bearing:

> Agents augment research and operations. The human is the decision layer.
> Irreversible or externally visible actions require explicit human approval.

This file pairs with [`GOVERNANCE.md`](GOVERNANCE.md), [`AGENTS.md`](AGENTS.md), [`STACK.md`](STACK.md), [`AUTOMATIONS.md`](AUTOMATIONS.md), and [`ARCHITECTURE.md`](ARCHITECTURE.md).

---

## Threat model

What this design defends against, in rough order of likelihood:

1. **Prompt injection via ingested content.** A benign-looking input — a web page, document, message, file or folder name, transcript, or tool output — contains text that tries to redirect an agent into leaking data or taking an action. This is treated as the **primary** threat.
2. **Credential exposure.** Keys leaking into logs, the repository, or agent context.
3. **Over-broad tool access.** An agent able to do more than its mandate requires.
4. **Accidental disclosure.** Sensitive content reaching the public repository.
5. **Unreviewed irreversible actions.** Publishing, sending to third parties, deploying to production, deleting, spending, or changing permissions without a human in the loop.

---

## 1. Instruction / data boundary

This is the core anti-prompt-injection control. It draws a hard line between **instructions** and **data**:

- **Trusted instructions come from the operator** through designated control channels.
- **External content is data, never authority** — web content, documents, message bodies, file names, folder names, transcripts, and tool outputs are treated as untrusted even when they are useful.

Enforcement:

- **No execution of in-content instructions.** If ingested content says “ignore previous instructions,” “send this,” “run this,” or similar, the agent does not comply. It treats the text as part of the source being analyzed and continues with the original task.
- **Identity by source, not by claim.** The privilege of a request is determined by verified origin and channel, not by text that asserts authority (“as the administrator…”).
- **Provenance and delimitation.** Untrusted text should retain source/provenance metadata and be clearly delimited from operator instructions before entering model context.
- **Capability backstop.** Even if injected text influences a model response, tool restrictions and approval gates are intended to prevent injection alone from producing an irreversible effect.

**Honest limitation:** prompt-injection mitigation is not solved. This design does not claim to detect every malicious string. It assumes some injection attempts will get through and relies on **least privilege plus human approval** as the backstop rather than perfect detection.

---

## 2. Tool access and least privilege

- Tool access should be **explicit and deny-by-default**: each agent receives only the capabilities needed for its mandate.
- Monitoring and research roles should be **network-read-only** unless a specific write path is justified.
- Write access should be scoped to the relevant repository, memory section, or project surface.
- High-impact capabilities — publishing, third-party messaging, production deploys, deletion, permission changes, and financial actions — should not be available as autonomous actions. They route through the approval gate in §5.

The per-function posture is summarized in [`STACK.md`](STACK.md#agent-cabinet-by-function) and the automation constraints are described in [`AUTOMATIONS.md`](AUTOMATIONS.md).

---

## 3. Read-only by default

- The default posture for agents and automations is **read-only**.
- Write and execute capabilities are opt-in, scoped by task and resource.
- QA / security / ops work starts with **read-only diagnostics** and proposes changes before applying them.
- Automations may monitor, summarize, check, and draft; they may not publish, send to third parties, deploy to production, delete, spend, or change permissions unattended.

---

## 4. Secrets management

- **Secrets do not belong in the repository.** Environment files, tokens, cookies, private keys, credentials, and service-role keys are excluded from public documentation and should be blocked by `.gitignore` where relevant.
- Credentials are stored outside the public project tree and exposed only through scoped runtime configuration, environment management, or tool wrappers. Where possible, tools should use local proxies or wrappers rather than exposing raw keys directly to agent context.
- Sensitive local paths such as `.ssh`, `.aws`, private key files, credential stores, and raw vault contents should not be mounted into public-facing or repo-scoped workspaces.
- Logs and memory are treated as sensitive by default. They should be scrubbed before publication and never copied wholesale into the public repository.

---

## 5. Human approval gates

The following actions require explicit operator approval before execution:

- publishing or posting public content;
- sending messages to third parties or public/external audiences;
- deploying to production;
- deleting data;
- changing access controls or sharing permissions;
- financial actions;
- creating or modifying standing automations that can affect external systems.

Clarification: replies to the operator inside the approved control channel and internal agent coordination are within normal operation. The approval gate applies to third-party, public, production, financial, destructive, or permission-changing actions.

How the gate works:

- **Approval is per action**, not a blanket grant.
- The agent presents a human-readable summary of what will happen before execution.
- Automations may stage actions — draft, prepare, queue, or recommend — but may not execute gated actions unattended.

---

## 6. Isolation and boundaries

- Agents operate within separate session/workspace boundaries where supported, reducing accidental context bleed between workstreams.
- Per-workstream memory is scoped: agents write only to their relevant memory or wiki area; shared knowledge is read-mostly.
- Inter-agent messages and delegated tasks should be validated by source/session, not by self-asserted identity in the message text.
- Workspace names and paths should be validated to avoid path traversal or accidental access outside the intended project boundary.

---

## 7. Public repository checklist

Run before pushing updates to a public repository. Where feasible, check git history as well as the current working tree.

- [ ] No credentials, tokens, cookies, API keys, auth headers, private keys, or `.env` files.
- [ ] No hostnames, IP addresses, account identifiers, analytics IDs, chat IDs, or internal routing identifiers.
- [ ] No private filesystem paths. Illustrative paths are acceptable only when clearly marked as non-real.
- [ ] No real system prompts containing sensitive operational detail; use placeholders or sanitized templates.
- [ ] No personal data, own or third-party: names, contacts, schedules, private messages, health, finance, or family context.
- [ ] No raw private vault, logs, transcripts, or memory files; only public-safe samples.
- [ ] Examples are anonymized and clearly marked as illustrative.
- [ ] Secret scanning has been run where feasible, including history for high-risk releases.
- [ ] All links point only to already-public resources.
- [ ] Claims about the system are factual, bounded, and do not imply official institutional use or autonomous decision-making.

---

## Scope note

This is a personal, experimental system, not an official or institutional one. Anyone adapting it should map these controls onto their own organization’s security, approval, legal, and records-management requirements before reuse.
