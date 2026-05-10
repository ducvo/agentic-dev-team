---
name: codebase-health-audit
description: >
  Audit a codebase for AI-generated technical debt: duplicate implementations,
  pattern drift, naming inconsistencies, dead code, and stale context files.
  Use this skill for weekly codebase health checks, before a major refactor,
  when code complexity is rising, or when multiple agent sessions have run
  across the same codebase.
---

# Codebase Health Audit

Catch AI-generated technical debt before it compounds into a month-12 crisis.

## AI-specific debt patterns to look for

- **Duplicate implementations**: agents reinvent helpers that already exist elsewhere
- **Pattern fragmentation**: each agent session applies slightly different conventions for the same problem
- **Stale context files**: AGENTS.md / CLAUDE.md diverge from actual codebase state
- **Dead code**: no author ownership means no one removes it
- **Tautological tests**: coverage looks healthy but tests don't catch bugs

## Audit checklist

### Duplication scan
- [ ] Multiple implementations of the same utility (date formatting, error handling, API clients)?
- [ ] Inconsistent naming for the same concept across modules?
- [ ] Parallel abstractions that do the same thing differently?

### Pattern consistency
- [ ] Pick 3 representative features — do they follow the same implementation pattern?
- [ ] Are layer boundaries respected (no business logic in controllers, etc.)?
- [ ] Are naming conventions consistent across modules added in different sessions?

### Context file accuracy
- [ ] Does AGENTS.md / CLAUDE.md reflect the actual codebase?
- [ ] Are there rules that are no longer accurate?
- [ ] Are there patterns agents are clearly using that aren't documented?

### Test quality signals
- [ ] Is coverage % correlated with actual defect catch rate?
- [ ] Are there tests that only assert the happy path?
- [ ] Are there tests that would pass even if the implementation were wrong?

### Static analysis trend
- [ ] Is the warning count rising sprint over sprint?
- [ ] Are new warning categories appearing (new anti-patterns being introduced)?

## Output format

```
## Codebase Health Audit

**Date:** YYYY-MM-DD
**Scope:** [files/modules reviewed]

### Critical (fix this sprint)
- [Issue]: [location] — [recommended action]

### Medium (schedule next sprint)
- [Issue]: [location] — [recommended action]

### Low (backlog)
- [Issue]: [location] — [recommended action]

### Context file updates needed
- [Stale rule or missing pattern]

### Suggested agent cleanup jobs
- [Job spec stub for each automatable fix]
```

## Gotchas
- Don't run a large cleanup agent without first scoping what it will touch — large blast radius cleanup is high risk
- Stale context files are the highest-leverage fix: updating them prevents future debt from accumulating
- Rising static analysis warning counts are a leading indicator — check the trend, not just the current count
- Teams that catch drift at month 3 avoid the month-12 crisis where refactoring becomes prohibitively expensive
