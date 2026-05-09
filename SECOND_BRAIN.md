# Second Brain / Obsidian Knowledge Graph

The stack uses an Obsidian-compatible Markdown vault as the human-readable knowledge layer.

## Why Obsidian

Obsidian is useful because it keeps knowledge in plain Markdown while adding:

- backlinks;
- graph view;
- local-first ownership;
- easy search;
- durable project notes;
- human-readable structure.

This makes it a good bridge between human thinking and agent memory.

## LLM Wiki Pattern

The vault follows an LLM Wiki Pattern:

1. Raw activity is captured in daily logs, notes, transcripts, and project updates.
2. Agents periodically review raw material.
3. Durable knowledge is distilled into project, concept, and entity pages.
4. Pages are connected with wikilinks.
5. Humans review and edit the resulting graph.
6. Agents can later consult the vault as structured long-term context.

## Suggested vault structure

```txt
second-brain/
├── README.md
├── proyectos/
│   ├── UNscribe.md
│   ├── AILEI.md
│   ├── Quorum.md
│   └── Antarctica-Embassy.md
├── conceptos/
│   ├── LLM-Wiki-Pattern.md
│   ├── Agentic-Diplomacy.md
│   └── AI-Governance.md
├── entidades/
│   ├── Coordinator-Agent.md
│   ├── Governance-Agent.md
│   ├── Research-Agent.md
│   ├── Engineering-Agent.md
│   └── Ops-Agent.md
├── inbox/
└── attachments/
```

## What belongs in the vault

- project state;
- durable concepts;
- people and institutions where relevant;
- decisions and rationales;
- research summaries;
- workflows;
- open loops;
- lessons learned.

## What does not belong in a public vault

- credentials;
- chat IDs;
- private personal details;
- sensitive logs;
- confidential diplomatic material;
- raw transcripts unless explicitly public;
- production configuration.

## Human-agent division of labor

Agents can help keep the vault updated. Humans decide what is important, correct, shareable, and strategically relevant.
