---
name: incident-postmortem
description: >
  Write a structured post-mortem after a production incident. Use this skill
  when an agent-caused or agent-related incident has been resolved, when a
  governance gap led to a production failure, when a security vulnerability
  was found in AI-generated code, or when the team needs to capture root
  cause and remediation to prevent recurrence.
---

# Incident Post-Mortem

Capture what happened, why, and what changes prevent recurrence — with specific attention to agent governance gaps.

## Blameless principle
Post-mortems identify system and process failures, not individual failures. The goal is to improve the system, not assign blame.

## Post-mortem template

```markdown
# Incident Post-Mortem: [Short title]

**Date:** YYYY-MM-DD
**Severity:** Critical | High | Medium
**Duration:** [detection time] → [resolution time] = [total duration]
**Author:** [name]
**Reviewers:** [names]

## Summary
[2–3 sentences: what happened, what was the impact, how it was resolved]

## Timeline
| Time | Event |
|------|-------|
| HH:MM | [What happened] |
| HH:MM | [Detection / alert fired] |
| HH:MM | [Investigation started] |
| HH:MM | [Root cause identified] |
| HH:MM | [Mitigation applied] |
| HH:MM | [Incident resolved] |

## Root cause
[One clear statement of the root cause — not a list of contributing factors]

**Root cause category:**
- [ ] Agent governance gap (missing scope, permissions, escalation trigger)
- [ ] Spec defect (vague AC, missing scope boundary, missing constraint)
- [ ] Context gap (agent lacked knowledge of existing pattern or constraint)
- [ ] AI code defect (silent failure, security vulnerability, logic error)
- [ ] Pipeline gap (insufficient verification before merge/deploy)
- [ ] Human error
- [ ] External dependency failure

## Contributing factors
- [Factor 1]
- [Factor 2]

## Impact
- Users affected: [count or description]
- Data affected: [yes/no — describe if yes]
- Revenue impact: [if applicable]
- SLA breach: [yes/no]

## Agent governance assessment
*(Complete this section for any incident involving AI-generated code or agent actions)*

- Was there a named human owner for the agent job? [Yes / No]
- Was the agent job spec complete (objective, ACs, scope, stop conditions)? [Yes / No / Partial]
- Did the agent respect its scope boundary? [Yes / No]
- Was the PR reviewed at the correct risk tier? [Yes / No / Not reviewed]
- Was the governance gap previously identified in a governance checklist? [Yes / No / Unknown]

## Remediation

### Immediate (already done)
- [Action taken during incident]

### Short-term (this sprint)
- [ ] [Specific action]: [owner]

### Long-term (this quarter)
- [ ] [Structural change]: [owner]

### Governance updates required
- [ ] Update AGENTS.md: [what to add]
- [ ] Update spec template: [what to add]
- [ ] Update governance checklist: [what to add]
- [ ] Create/update ADR: [decision to formalise]
- [ ] Update pipeline verification: [what gate to add]
```

## After the post-mortem

1. Share with the full team — incidents are learning opportunities for everyone
2. Add the root cause to the relevant AGENTS.md as a gotcha
3. If a spec defect caused it, update the spec template or `spec-completeness-auditor` checklist
4. If a governance gap caused it, run the `governance-checklist` across all agents
5. Create regression tests via QA's `regression-test-generator`

## Gotchas
- "The agent did something unexpected" is not a root cause — find the governance or spec gap that allowed it
- Incidents caused by AI-generated code are almost always traceable to a spec gap, context gap, or missing review gate
- A post-mortem with no governance updates is a post-mortem that will be repeated
- Complete within 48 hours of resolution — details fade and the team moves on
