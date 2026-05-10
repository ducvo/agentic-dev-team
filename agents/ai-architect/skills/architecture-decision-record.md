---
name: architecture-decision-record
description: >
  Draft, review, or update an Architecture Decision Record (ADR). Use this
  skill when the user needs to document an architecture decision, evaluate
  technology options, capture tradeoffs, record a design choice, or detect
  whether an existing ADR is stale relative to current code. Also use when
  the user asks "what should we use for X" or "how do we decide between Y and Z."
---

# Architecture Decision Record

Draft, review, and maintain ADRs that agents and engineers can rely on.

## Workflow

### Step 1: Gather context
Before drafting, collect:
- The problem being solved (one sentence)
- Constraints (performance, cost, compliance, team skill, existing stack)
- Options already under consideration
- Any existing ADRs this decision relates to or supersedes

### Step 2: Draft the ADR

```markdown
# ADR-[NNN]: [Short title — decision made, not problem]

**Date:** YYYY-MM-DD
**Status:** Proposed | Accepted | Deprecated | Superseded by ADR-NNN
**Deciders:** [names or roles]

## Context
[What situation forces this decision? What constraints apply?
Keep to facts — no opinions here.]

## Options considered

### Option A: [Name]
- Pros: ...
- Cons: ...

### Option B: [Name]
- Pros: ...
- Cons: ...

## Decision
[What was decided and why. One clear statement.]

## Consequences
- Positive: ...
- Negative / tradeoffs accepted: ...
- Risks: ...

## Compliance with existing ADRs
[Does this conflict with or supersede any existing ADR? List them.]
```

### Step 3: Staleness check
When reviewing an existing ADR, check:
- Does the codebase still reflect this decision?
- Has the technology landscape changed (new versions, deprecations)?
- Have constraints changed (scale, compliance, team)?
- If stale: update status to Deprecated and draft a superseding ADR

## Gotchas
- Title the ADR with the decision made, not the problem: "Use PostgreSQL for primary storage" not "Database selection"
- Status "Accepted" means agents must follow it — keep accepted ADRs current or mark them deprecated
- Don't document implementation details in ADRs — those belong in code comments or READMEs
- One decision per ADR — compound decisions are hard to supersede cleanly
- Agents replicate existing patterns including bad ones — stale ADRs that contradict current code cause drift
