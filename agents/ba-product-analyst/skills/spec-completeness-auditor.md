---
name: spec-completeness-auditor
description: >
  Review a draft spec for gaps, ambiguities, and anti-patterns before it goes
  to an AI agent. Use this skill when reviewing a spec before agent execution,
  when acceptance criteria seem vague, or when a previous agent run failed and
  the spec needs to be audited.
---

# Spec Completeness Auditor

Catch spec defects before the agent runs — they're the most expensive defects to fix.

## Audit checklist

### Pre-flight: triage
- [ ] A triage decision record exists for this requirement
- [ ] Decision is "Proceed as-is" or "Alter" (not pending or escalated)
- [ ] Risk/compliance flags from triage are reflected in the spec (governance section present if flagged)

### Structure
- [ ] Objective states what will exist after completion (not what will be done)
- [ ] "Not Included" section exists with at least 2 out-of-scope items
- [ ] Business rules are specific and testable
- [ ] Input/output contract is defined

### Acceptance criteria
- [ ] Each criterion is binary pass/fail
- [ ] Each criterion has a concrete input and expected output
- [ ] Happy path covered
- [ ] At least 2 edge cases covered
- [ ] At least 1 error case covered
- [ ] No vague quality attributes ("fast", "robust", "handle errors gracefully")

### Boundaries
- [ ] "Always" tier defined
- [ ] "Ask First" tier defined (most commonly omitted)
- [ ] "Never" tier defined

### Anti-patterns to flag and fix
- **Implementation hints**: spec tells HOW to implement → remove
- **Pseudo-code in spec**: agent should choose the approach → remove
- **Restated codebase content**: point to existing file instead
- **Vague quality attributes**: convert to measurable criteria or remove

## Output format

```
## Spec Audit: [Spec name]

### Structure: Pass | Needs work
[Issues found]

### Acceptance criteria: Pass | Needs work
[Specific criteria that are vague, missing, or not binary]

### Boundaries: Pass | Needs work
[Missing tiers]

### Anti-patterns found
- [Anti-pattern]: [location] → [suggested fix]

### Gaps requiring BA decision
- [Question that must be answered before this spec is agent-ready]

### Verdict
Ready | Not ready — [what must be fixed]
```

## Gotchas
- The "Ask First" tier is the most commonly omitted and most costly when missing
- Acceptance criteria referencing implementation details are fragile — test behavior, not implementation
- If you can't write a concrete input/output example for a criterion, the requirement isn't understood yet
