---
name: agent-identity-governance
description: >
  Design and audit permission models for AI agents. Use this skill when
  setting up credentials for a new AI agent, auditing existing agent service
  accounts, designing least-privilege access for an agent integration, or
  reviewing agent permissions after a security incident. Also use when asked
  "what permissions does this agent need?"
---

# Agent Identity Governance

Design least-privilege permission models for AI agents — traditional IAM was built for humans.

## Why agents are different
- Operate 24/7 with no natural session boundaries
- Can interact across multiple systems in a single job
- Accumulate permissions over time without natural review triggers
- A compromised agent credential is a persistent, automated attacker

## Permission design workflow

### Step 1: Define the agent's action inventory
List every action the agent needs:
- Read: which repos, files, databases, APIs?
- Write: which repos, files, databases, APIs?
- Execute: which commands, scripts, pipelines?
- Create: PRs, issues, deployments?
- Delete: anything? (flag for extra scrutiny)

### Step 2: Apply least-privilege
For each action:
- Is this required for the agent's defined job? If no → remove
- Can this be read-only instead of read-write? If yes → use read-only
- Can this be scoped to a specific repo/path instead of broad access? If yes → scope it
- Does this need to be available 24/7 or only during pipeline runs? If pipeline-only → use short-lived credentials

### Step 3: Service account spec

```markdown
## Agent Service Account: [agent name]

**Human owner:** [name — accountable for this agent's actions]
**Purpose:** [one sentence]
**Credential type:** Short-lived token | Long-lived key | OAuth app

### Permissions
| Resource | Action | Scope | Justification |
|---|---|---|---|
| [repo] | read | [specific path] | [why needed] |
| [API] | [method] | [specific endpoint] | [why needed] |

### Explicitly denied
- Delete operations on production data
- Direct push to main/master
- [Other prohibited actions]

### Credential rotation
- Cadence: [e.g., 90 days]
- Owner: [name]

### Quarterly audit checklist
- [ ] Named human owner still active?
- [ ] Permissions still minimum required?
- [ ] Any delete permissions? (justify or remove)
- [ ] Credentials rotated on schedule?
- [ ] Audit log coverage confirmed?
```

## Red flags requiring immediate review
- Agent has write access to production databases
- Agent can push directly to main/master
- Org-wide repo access instead of specific repo access
- Credentials not rotated in > 90 days
- No human owner assigned
- Agent can self-approve PRs or merge to protected branches

## Gotchas
- "The agent needs access to everything" is a design smell — break the job into smaller scoped jobs
- Short-lived credentials (OIDC tokens, temporary STS) are always preferable to long-lived keys
- Audit logs are not optional — without them you cannot investigate what an agent did during an incident
- Agents that can self-approve PRs are a governance failure, not a feature
