# Automation Layer

Scheduled agent runs are used for recurring monitoring and maintenance. They are useful, but constrained.

## Principle

> Automations can watch, summarize, check, draft, and maintain context. They should not decide, publish, delete, spend, or represent.

## What automations are allowed to do

- monitor public sources;
- produce briefings;
- check project or system health;
- update non-sensitive internal documentation;
- draft recommendations;
- flag anomalies;
- maintain a second brain.

## What automations should not do without human approval

- publish public messages;
- send official communications;
- change production infrastructure;
- delete data;
- spend money;
- expose private information;
- make policy or diplomatic decisions.

## Representative automation categories

### Briefings

Recurring briefings summarize developments in AI governance, technical AI research, and relevant institutional processes.

### Project monitoring

Project checks can monitor deployments, pipelines, data refreshes, and QA status for tools such as UNscribe, AILEI, Quorum, and Antarctica Embassy.

### System health

Operational checks monitor agent jobs, delivery status, gateway health, and security posture.

### Knowledge maintenance

A scheduled wiki compiler can review raw memory logs and convert them into durable Markdown pages inside the second brain.

## Why not publish exact cron jobs?

Exact schedules, IDs, paths, account names, and production configuration are operational details. They change often and can expose unnecessary security information.

This public repo documents the pattern, not the private infrastructure.

## Public-safe scheduled run template

See [`templates/cron-template.md`](templates/cron-template.md).
