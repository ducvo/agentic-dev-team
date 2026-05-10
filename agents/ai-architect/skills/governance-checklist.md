---
name: governance-checklist
description: >
  Run a governance health check on an agent platform or codebase. Use this
  skill when onboarding a new agent, auditing existing agent governance,
  preparing for a security review, checking compliance readiness, or when
  asked "are our agents properly governed?"
---

# Governance Checklist

Audit agent governance health — gaps here turn manageable mistakes into production crises.

## Checklist

### Agent inventory
- [ ] Every deployed agent has a named human owner
- [ ] Every agent has a defined scope (what it can do, what it cannot)
- [ ] Every agent has documented escalation triggers
- [ ] Retired agents have been decommissioned (credentials revoked, pipelines disabled)

### Access controls
- [ ] All agent service accounts follow least-privilege
- [ ] No agent can push directly to main/master
- [ ] No agent can self-approve its own PRs
- [ ] No agent has delete access to production data without explicit justification
- [ ] Agent credentials are rotated on schedule (≤ 90 days)
- [ ] Short-lived credentials used where possible (OIDC, temporary STS)

### Audit trails
- [ ] All agent actions are logged with: agent ID, action, timestamp, human owner
- [ ] Logs are retained for the required compliance period
- [ ] Logs are queryable (can answer "what did agent X do between time A and B?")
- [ ] Cost attribution tags on all LLM API calls (team, repo, pipeline, step)

### Quality gates
- [ ] Every agent PR goes through the defined review tier (auto / 1-human / 2-person)
- [ ] Risk scoring is applied to every agent PR
- [ ] Dependency provenance is verified for all agent-added packages
- [ ] Security scanning runs on all agent-generated code

### Cost controls
- [ ] Hard token budget per pipeline job
- [ ] Retry budget with circuit breaker
- [ ] Cost alerts configured with named owner
- [ ] Monthly cost review scheduled

### Incident readiness
- [ ] Runbooks exist for: cost spike, credential leak, infinite loop, hallucinated dependency
- [ ] Kill-switch procedure documented and tested
- [ ] On-call rotation covers agent incidents

## Output format

```
## Governance Health Check

**Date:** YYYY-MM-DD
**Scope:** [agents / repos reviewed]

### Gaps found
| Category | Gap | Risk | Recommended fix |
|---|---|---|---|

### Critical gaps (fix immediately)
- [Gap]: [risk] — [fix]

### Medium gaps (fix this sprint)
- [Gap]: [risk] — [fix]

### Compliant areas
- [Area]: [evidence]
```

## Gotchas
- "We have CI" is not governance — governance requires audit trails, named owners, and escalation paths
- The most dangerous gap is usually the one that hasn't caused an incident yet
- Governance retrofitted after an incident costs 10× more than governance built in from the start
- Every agent needs a retirement plan — ungoverned zombie agents are a persistent security risk
