---
name: spec-drafter
description: >
  Convert raw stakeholder input into a structured agent-ready spec. Use this
  skill when turning a meeting transcript, stakeholder notes, Jira ticket, or
  one-sentence request into a spec that an AI agent can execute reliably.
  Also use when a previous agent run produced wrong output and the spec needs
  to be rebuilt from the original business intent.
---

# Spec Drafter

Convert raw business intent into a machine-executable spec.

## Why spec quality matters
A consultant writing bad code by hand produces one broken artifact per day. An agent produces a dozen before lunch. The blast radius of a vague spec compounds at machine speed.

## Workflow

### Step 1: Extract the essentials
From the transcript/ticket/notes, identify:
- What business problem is being solved?
- What does "done" look like in concrete terms?
- What must NOT be included (scope boundary)?
- What edge cases would a domain expert know about?

### Step 2: Classify spec depth
- **Simple** (config change, small form): Objective + Acceptance Criteria + Boundaries
- **Standard** (new feature, integration): Full 7-section spec
- **Complex** (compliance, multi-system): Full spec + schema contracts + governance

### Step 3: Write the spec

```markdown
## Spec: [Feature name]

### Objective
[One sentence: what will exist after this is built that doesn't exist now]

### Not included
- [Out of scope item — list obvious adjacent features that are excluded]

### Business rules
- [Rule 1: specific and testable]
- [Edge case handling]

### Input / output contract
Input: [schema or concrete example]
Output: [schema or concrete example]

### Acceptance criteria
| Scenario | Input | Expected output |
|----------|-------|----------------|
| Happy path | [example] | [expected] |
| Edge case | [example] | [expected] |
| Error case | [example] | [expected] |

### Boundaries
**Always** (agent can do without asking): [action]
**Ask first** (must confirm before proceeding): [action]
**Never** (hard prohibition): [action]

### Test plan
- [How to verify this works]
```

### Step 4: Flag gaps
- [ ] Acceptance criteria are concrete input/output pairs (not vague quality attributes)
- [ ] "Not Included" covers obvious adjacent features
- [ ] "Ask First" tier is defined
- [ ] Error cases are specified

## Gotchas
- "Handle errors gracefully" is not an acceptance criterion — specify what error handling looks like
- Don't include implementation hints — specify what, not how
- Vague quality attributes ("fast", "robust") must be converted to measurable criteria or removed
- If the stakeholder can't answer "what does done look like?", the spec isn't ready
