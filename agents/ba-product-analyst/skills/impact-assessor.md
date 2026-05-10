---
name: impact-assessor
description: >
  Assess the downstream impact of a requirement change. Use this skill when
  a stakeholder requests a change mid-sprint, when a requirement is modified
  after a spec has been written, when evaluating whether a change is in or
  out of scope, or when communicating the cost of a change to stakeholders.
---

# Impact Assessor

Identify what a requirement change breaks before agreeing to it.

## Workflow

### Step 1: Characterize the change
- What is being added, removed, or modified?
- Is this a scope expansion, clarification, or correction?
- When was the original requirement agreed?

### Step 2: Identify affected artifacts

| Artifact type | Check |
|---|---|
| Specs | Which specs reference this requirement? |
| Acceptance criteria | Which ACs are invalidated or need updating? |
| Agent jobs | Which jobs have already run or are in progress? |
| Code | Which modules implement this requirement? |
| Tests | Which tests verify this requirement? |
| Downstream systems | Which integrations depend on this behavior? |
| Compliance | Does this change affect audit requirements? |

### Step 3: Classify the impact
- **Clarification** (no behavior change): update spec, no rework
- **Scope addition** (new behavior): new spec section, new agent job, new tests
- **Scope change** (existing behavior modified): update spec, re-run affected jobs, update tests
- **Breaking change** (downstream systems affected): requires stakeholder sign-off

### Step 4: Output

```markdown
## Impact Assessment: [Change description]

**Change type:** Clarification | Addition | Modification | Breaking

### Affected artifacts
- Specs: [list]
- Acceptance criteria: [list]
- Agent jobs to re-run: [list]
- Tests to update: [list]
- Downstream impacts: [list]

### Effort estimate
- Spec updates: [hours]
- Agent re-runs: [count × time]
- Test updates: [hours]
- Total: [estimate]

### Recommendation
Proceed | Defer | Descope | Needs stakeholder decision

### Required sign-offs
- [ ] [Stakeholder] — [what they need to approve]
```

## Gotchas
- "Small" requirement changes often have large downstream impacts — always check all artifact types
- Changes to acceptance criteria invalidate agent jobs that have already run — those jobs need re-running
- Scope additions mid-sprint are the most common cause of missed deadlines — quantify the cost before agreeing
- Breaking changes to downstream systems require Tech Lead sign-off, not just BA
