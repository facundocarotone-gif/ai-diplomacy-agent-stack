# Agent Roles

This stack uses role separation. Each agent has a mandate, a context boundary, and a different default posture toward tools and action.

## Coordinator

**Mandate:** chief-of-staff layer.

Responsibilities:

- route requests;
- coordinate specialist agents;
- keep track of open loops;
- maintain operating context;
- summarize results for the human operator;
- ensure approvals before external or irreversible actions.

## AI Governance & Diplomacy Agent

**Mandate:** multilateral AI governance, UN processes, policy analysis, and diplomatic context.

Responsibilities:

- monitor AI governance developments;
- track UN processes and institutional timelines;
- support diplomatic analysis;
- connect technical AI developments to governance implications;
- maintain context for projects such as AILEI and Antarctica Embassy.

## AI Research Radar Agent

**Mandate:** AI technical developments and research monitoring.

Responsibilities:

- track model releases and capability shifts;
- summarize relevant papers, talks, and technical debates;
- maintain educational explainers;
- identify developments relevant to governance and public-interest technology.

## Engineering Agent

**Mandate:** implementation, debugging, deployment, and technical maintenance.

Responsibilities:

- build and maintain prototypes;
- inspect codebases;
- run tests and deployments with approval where required;
- maintain engineering documentation;
- support projects such as UNscribe, Quorum, AILEI, and Antarctica Embassy.

## QA / Security / Operations Agent

**Mandate:** reliability, safety, and operational health.

Responsibilities:

- monitor system health;
- check scheduled tasks;
- review security posture;
- identify broken automations;
- verify delivery and deployment status;
- prefer read-only diagnostics before fixes.

## Positioning / Opportunities Agent

**Mandate:** narrative, audiences, market/problem framing, and public-interest opportunities.

Responsibilities:

- help explain projects clearly;
- identify target users and use cases;
- refine public narratives;
- scan for relevant opportunities;
- support launch/readme/presentation framing.

## Public documentation rule

This repository describes roles and workflows, not private identities, credentials, chat IDs, personal data, or production secrets.
