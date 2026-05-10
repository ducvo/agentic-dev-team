---
name: ai-code-review
description: >
  Review AI-generated code for architectural fit, security vulnerabilities,
  and spec adherence. Use this skill when reviewing a PR produced by an AI
  agent, assessing risk level of an agent-generated change, checking whether
  AI output matches its spec, or deciding whether a PR needs 1-human or
  2-person review. Not for general code review of human-written code.
---

# AI Code Review

Architectural-level review of agent-generated PRs — the things volume-pressured humans miss.

## Review checklist

### 1. Spec adherence (check first)
- [ ] Does the PR implement what the spec's Objective says — no more, no less?
- [ ] Are all acceptance criteria satisfied? (test each one)
- [ ] Is the "Not Included" scope boundary respected?
- [ ] Does the PR description explain what was changed and why?

### 2. Architectural consistency
- [ ] Does this follow existing patterns in the codebase, or introduce a new abstraction?
- [ ] Does it duplicate logic that already exists elsewhere? (agents commonly reinvent helpers)
- [ ] Does it respect layer boundaries (e.g., no business logic in controllers)?
- [ ] Does it conflict with any accepted ADR?

### 3. Security (OWASP LLM Top 10 focus)
- [ ] No hardcoded secrets, API keys, or credentials
- [ ] Input validation present on all external inputs
- [ ] Auth/permission checks not bypassed or weakened
- [ ] No SQL/command injection vectors
- [ ] No sensitive data logged or exposed in error messages
- [ ] New dependencies: do they exist in the registry? Are they maintained? Any known CVEs?

### 4. Test quality
- [ ] Are tests human-authored expectations or AI-generated tautologies?
- [ ] Do tests verify behavior (what it should do) or implementation (what it does)?
- [ ] Are boundary conditions and error paths covered?
- [ ] Would these tests catch a silent logic error?

### 5. Risk scoring

| Factor | Low | Medium | High |
|--------|-----|--------|------|
| Files changed | <4 | 4–15 | 15+ |
| Touches auth/payments/security | No | Adjacent | Direct |
| New external dependency | No | Existing vendor | New vendor |
| Cross-service impact | No | Read-only | Write/delete |
| Test coverage of changes | >80% | 50–80% | <50% |

**Review tier:**
- All Low → auto-merge (if CI passes)
- Any Medium → 1 human reviewer
- Any High → 2-person review + threat model + **trigger QA `adversarial-code-review`**

For High-risk PRs, explicitly request adversarial review from QA before merge. Do not substitute a second human reviewer for adversarial review — they serve different purposes.

## Output format

```
## AI Code Review: [PR title]

**Risk score:** Low | Medium | High
**Recommended review tier:** Auto-merge | 1-human | 2-person + threat model

### Spec adherence
[Pass/fail per acceptance criterion]

### Architectural issues
[List any, or "None found"]

### Security findings
[List any, or "None found"]

### Test quality
[Assessment]

### Recommendation
[Approve / Request changes / Escalate]
[Specific changes required if any]
```

## Gotchas
- AI PRs look locally clean (good formatting, consistent naming) but hide global inconsistencies — always check architectural fit, not just local correctness
- 59% of developers report shipping AI code they don't fully understand — the review is the last line of defense
- Tautological tests (AI writes code then writes tests for that code) give false confidence — check what the tests actually assert
- A new dependency added by an agent may not exist in any registry (hallucinated package names are a real supply chain risk)
