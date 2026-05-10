---
name: tech-debt-triage
description: >
  Identify and prioritize technical debt introduced by AI-generated code.
  Use this skill when auditing a codebase for pattern drift, duplicate
  implementations, stale documentation, naming inconsistencies, or rising
  complexity from multiple agent sessions. Also use for weekly codebase
  health checks or before a major refactor sprint.
---

# Tech Debt Triage

Identify and prioritize AI-generated technical debt before it compounds.

## Why this is different from normal tech debt
AI-assisted codebases accumulate debt in specific patterns:
- **Duplicate implementations**: agents reinvent helpers that already exist
- **Pattern fragmentation**: each agent session applies slightly different conventions
- **Stale context files**: AGENTS.md / CLAUDE.md diverge from actual codebase state
- **Dead code**: no author ownership means no one removes it
- **Tautological tests**: coverage metrics look healthy but tests don't catch bugs

## Audit workflow

### Step 1: Scan for duplication
Look for:
- Multiple implementations of the same utility (date formatting, error handling, API clients)
- Inconsistent naming for the same concept across modules
- Parallel abstractions that do the same thing differently

### Step 2: Check pattern consistency
- Pick 3 representative features and compare their implementation patterns
- Flag where agents applied different conventions for the same problem type
- Check layer boundaries — business logic leaking into wrong layers

### Step 3: Audit context files
- Read AGENTS.md / CLAUDE.md against the actual codebase
- Flag rules that are no longer accurate
- Flag patterns described that no longer exist (or exist differently)
- Flag missing patterns that agents are clearly using

### Step 4: Check test quality signals
- Coverage % vs. actual defect catch rate (if production data available)
- Ratio of assertion-heavy tests to assertion-light tests
- Presence of tests that only assert the happy path

### Step 5: Prioritize

| Category | Priority | Effort | Action |
|----------|----------|--------|--------|
| Duplicate critical utilities | High | Low | Consolidate, update imports |
| Stale context files | High | Low | Update AGENTS.md |
| Pattern fragmentation in active modules | Medium | Medium | Refactor + add linter rule |
| Dead code | Low | Low | Delete |
| Tautological tests | Medium | High | Rewrite with human expectations |

## Output format

```
## Tech Debt Triage Report

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
- Don't run a big cleanup agent without first auditing what it will touch — large blast radius cleanup jobs are high risk
- Stale context files are the highest-leverage fix: updating them prevents future debt from accumulating
- Rising static analysis warning counts are a leading indicator — check the trend, not just the current count
- Teams that catch drift at month 3 avoid the month-12 crisis where refactoring becomes prohibitively expensive
