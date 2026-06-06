# Operational Stack

This document describes how the stack is organized in practice: hosting, agent roles, memory, automations, model routing, and deployment surface.

It is the operational companion to [`ARCHITECTURE.md`](ARCHITECTURE.md), [`AGENTS.md`](AGENTS.md), [`AUTOMATIONS.md`](AUTOMATIONS.md), and [`SECURITY.md`](SECURITY.md).

It is **sanitized for public release**. The following are intentionally omitted: hosting provider, credentials and tokens, exact prompts, exact schedules, account and analytics IDs, private filesystem paths, and private vault contents. Where a real value would be sensitive, this document uses a representative placeholder and says so.

---

## Operating model

A single human operator works through a **coordinator** agent that routes requests to a small cabinet of **specialized agents**. State lives in a plain-text, Obsidian-compatible **memory vault**. A scheduler runs recurring monitoring and maintenance jobs. Public-interest prototypes are maintained through GitHub and deployed to a managed hosting surface.

The human is the decision layer. Agents monitor, retrieve, analyze, draft, and prepare work; they do not publish, send to third parties, deploy to production, delete, spend, or change permissions without explicit approval. See [`SECURITY.md`](SECURITY.md).

The system is organized in five layers:

```txt
sources / inbox
(web, documents, notes, messages)
        ↓
coordinator
(routing, context recovery, approvals)
        ↓
specialized agents
(research, governance, engineering, QA, positioning)
        ↓
memory vault
(raw logs → distilled wiki)
        ↓
GitHub / deployment surface
(with human approval for production or external effects)
```

---

## Hosting layer

- A **self-managed Linux environment** runs the orchestrator and agent sessions. The provider and host details are intentionally unspecified.
- Agents operate within separate session/workspace boundaries where supported, reducing accidental context bleed between workstreams.
- Sensitive local context is kept private and excluded from the public repository.
- Public-safe excerpts can be copied into reviewer-facing documentation, but the private vault itself is not published.

---

## Agent cabinet by function

The cabinet uses **role separation**. Each function has a mandate, default posture, scoped tools, and a model tier matched to the work. Roles are defined narratively in [`AGENTS.md`](AGENTS.md); this is the operational view.

| Function | Default posture | Tool scope | Model tier |
|---|---|---|---|
| **Coordinator** | Orchestrate; no external side effects | Routing, memory retrieval, run-log updates, approval requests | A for routing; escalates to B/C |
| **Governance & diplomacy** | Read-mostly | Web read, memory read, scoped wiki updates | B; C for sensitive analysis |
| **Research radar** | Read-only by default | Web/paper read, scoped wiki updates | A–B |
| **Engineering** | Write-scoped | Repo read/edit, tests, deployment preparation; production deploy requires approval | B; C for hard debugging |
| **QA / security / ops** | Read-only diagnostics first | Health checks, log review, config inspection, issue reports | A–B |
| **Positioning / opportunities** | Draft-only | Memory read, public-safe drafting; no third-party send | B |

High-impact actions are not intended to be autonomous. Publishing, third-party messaging, production deploys, deletions, permission changes, financial actions, and standing automation changes route through approval gates.

---

## Model tiers

Model calls can be routed through direct providers or aggregation gateways depending on task, cost, reliability, and availability. Tiers are assigned by task, not permanently by agent:

- **Tier A — triage / low-cost.** Routing, classification, deduplication, low-stakes summarization, and automation pre-checks.
- **Tier B — workhorse.** Drafting, structured analysis, code edits, and most day-to-day agent work.
- **Tier C — frontier / sparing use.** Complex reasoning and final review of policy-sensitive or externally bound text.

Cost discipline is part of the operating model: where possible, recurring jobs use cheap pre-checks — “did anything material change?” — before waking a stronger model.

---

## Memory / second brain

The knowledge layer is **portable plain-text Markdown**, Obsidian-compatible, so it remains human-readable and survives changes in tooling. It follows an LLM-wiki pattern in three stages:

1. **Raw capture** — activity, clips, and notes land in append-only logs or workstream notes.
2. **Distillation** — agents compile durable knowledge into wiki pages organized as projects, concepts, entities, and timelines.
3. **Retrieval** — agents read context back from the same files, so what a human reviews and what an agent retrieves are aligned.

Two scopes:

- **Shared / global** — common knowledge, read-mostly; only designated workflows should write.
- **Per-workstream / local** — scoped context owned by a specific agent, project, or workflow.

The wiki is reviewed and corrected by the human. The real vault is private; the public [`second-brain/`](second-brain/) sample is a small, public-safe illustration.

---

## Automation layer

A scheduler runs cron, interval, and one-off jobs such as:

- morning governance and research briefings;
- monitoring of UN / AI-governance processes and model releases;
- project and deployment health checks;
- knowledge-maintenance passes that turn recent raw captures into wiki pages.

Two rules govern automations:

- **Pre-check before spend where possible.** Cheap checks gate higher-cost model calls when a task can first ask whether anything changed.
- **No irreversible authority.** Automations may monitor, summarize, check, and draft. They may stage actions, but they do not execute publishing, third-party sends, production deploys, deletions, permission changes, or financial actions unattended.

See [`AUTOMATIONS.md`](AUTOMATIONS.md) and [`SECURITY.md`](SECURITY.md).

---

## Deployment surface: GitHub + managed hosting

Source lives in **GitHub**; prototypes are deployed to a managed hosting provider. The engineering role can inspect repositories and prepare changes. Production deploys and irreversible operations require human approval.

Public artifacts maintained or supported by workflows like the ones described here:

| Project | What it is | Link |
|---|---|---|
| **AILEI** | AI Language Evaluation Index — language equity across models | <https://ailei-one.vercel.app/> |
| **Quorum** | UN Security Council simulation / diplomatic reasoning environment | <https://quorum-sc.vercel.app/> |
| **Antarctica Embassy** | Experimental autonomous-embassy diplomatic interface | <https://antarctica-embassy.vercel.app/> |

These links are not meant to imply that the public repository contains their implementation code. They show the kind of deployed artifacts the operating model supports.

---

## Operational layout (sanitized)

A representative layout of the running system. Names are illustrative; real private paths, provider details, and contents are not shown. Secrets and allowlists live outside public project roots and are not committed.

```txt
stack/
├── orchestrator/              # coordinator: routing, context, run loop
│   ├── router.*               # request → agent routing
│   ├── scheduler.*            # cron / interval / one-off jobs
│   └── run-log/               # append-only activity log (gitignored)
├── agents/                    # separate workspace/session boundary per function
│   ├── governance/            # read-mostly; policy & UN-process monitoring
│   ├── research-radar/        # read-only by default; model / paper tracking
│   ├── engineering/           # write-scoped; production deploy gated by approval
│   ├── qa-sec-ops/            # read-only diagnostics first
│   └── positioning/           # drafting; no third-party send
├── memory/                    # Obsidian-compatible Markdown vault
│   ├── raw/                   # append-only captures
│   ├── wiki/                  # distilled: projects / concepts / entities / timelines
│   └── .index/                # local retrieval index (gitignored)
├── automations/               # job definitions + pre-check scripts where useful
└── deploy/                    # deployment manifests (no secrets)

# stored outside public project roots, never committed:
secrets/                       # env files and tokens, managed outside the repo
allowlists/                    # per-agent tool / path boundaries where supported
```

---

## What this document does not contain

By design: hosting provider; API keys, tokens, cookies, or credentials; exact system prompts; exact schedules; analytics, account, or chat IDs; private filesystem paths; and private vault contents. The security rationale for these omissions is in [`SECURITY.md`](SECURITY.md).
