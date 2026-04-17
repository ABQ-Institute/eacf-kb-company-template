# 5. Agents

One file per agent. The audit trail for AI deployment in this organisation.

## Adding an agent

Copy `_template/README.md` to `1-agent-name.md`. Fill in the configuration, skills, and constraints. Open a PR — company-layer owner approves.

## What each agent file contains

- KB_LOCATION and platform
- Skills loaded and their purpose
- What the agent can read and write
- Where it operates (Slack, terminal, IDE)
- Any constraints beyond the defaults

## Why this matters

Every agent with access to this KB should be documented here. This tells humans what's running, what it can do, and who deployed it.
