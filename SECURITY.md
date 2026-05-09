# Security and Privacy

This repository is intentionally sanitized. It describes patterns, not private infrastructure.

## Do not publish

- API keys;
- tokens;
- credentials;
- private prompts containing sensitive context;
- chat IDs or account IDs;
- production config files;
- logs with personal or confidential data;
- raw diplomatic material unless already public and cleared;
- exact operational schedules where unnecessary.

## Recommended controls

- Prefer read-only monitoring by default.
- Scope tool permissions by agent role.
- Keep secrets outside prompts and repos.
- Require approval for public/external/irreversible actions.
- Use audit logs for important actions.
- Separate public examples from private production configuration.
- Review generated wiki pages before publication.

## Threat model

Main risks:

- accidental disclosure of private context;
- over-permissioned tools;
- automation acting without human review;
- stale memory producing wrong conclusions;
- confusing drafts with official positions;
- prompt injection from external content.

## Public repo principle

If a detail would help an attacker, identify a private person, expose an account, or reveal operational access, it does not belong in the public version.
