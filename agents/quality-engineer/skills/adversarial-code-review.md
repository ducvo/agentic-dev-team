---
name: adversarial-code-review
description: >
  Review AI-generated code for silent failures, missing edge cases, security
  vulnerabilities, and tautological tests. Use this skill when reviewing a PR
  produced by an AI agent, when validating AI output against business intent,
  or when a previous AI-generated feature caused a production incident.
---

# Adversarial Code Review

Find what AI-generated code gets wrong that normal review misses.

## The adversarial mindset
Assume at least one subtle logic error exists. AI code looks locally clean but hides silent failures — wrong result, no error, no stack trace. Run this review in a fresh context session (not the one that generated the code).

## Review checklist

### Silent failure patterns
- [ ] Does every code path return a meaningful result or throw a meaningful error?
- [ ] Are there conditions where the function returns null/undefined/empty silently?
- [ ] Does the code handle unexpected data from external calls?
- [ ] Are there off-by-one errors in loops, pagination, or date ranges?

### Boundary conditions
- [ ] What happens with empty input? Null? Zero? Negative numbers?
- [ ] What happens at the maximum allowed value?
- [ ] What happens with Unicode, special characters, or very long strings?
- [ ] What happens when a list has 0 items? 1 item? Maximum items?

### Security
- [ ] Is user input validated before use?
- [ ] Are there SQL/command injection vectors?
- [ ] Is sensitive data logged or in error messages?
- [ ] Are auth/permission checks present and correct?
- [ ] Are new dependencies real packages (not hallucinated names)?

### Test quality
- [ ] Do tests assert expected behavior or just verify what the implementation does?
- [ ] Are boundary conditions tested (not just happy path)?
- [ ] Are error paths tested?
- [ ] Would these tests catch a subtle logic error?

### Spec and acceptance criteria adherence
- [ ] Obtain the spec's acceptance criteria before starting this review
- [ ] For each AC: does the code produce the expected output for the given input?
- [ ] Are there ACs the code silently ignores or partially implements?
- [ ] Does the code implement anything NOT in the spec (scope creep)?

## Output format

```
## Adversarial Review: [function/PR name]

### Silent failure risks
- [Specific scenario where wrong result is returned silently]

### Missing boundary conditions
- [Input that would cause unexpected behavior]

### Security findings
- [Specific vulnerability or risk]

### Test quality issues
- [Tautological test or missing assertion]

### Spec / AC adherence gaps
- [AC that is not satisfied or only partially implemented]

### Verdict: Pass | Needs changes | Block
[Specific changes required]
```

## Gotchas
- 60% of AI code faults compile, pass tests, and look correct in review — silent failures are the norm
- Tautological tests give false confidence — 90% coverage can mean almost nothing is meaningfully tested
- Run in a fresh context — don't use the same session that generated the code (shared blind spots)
- The most dangerous AI code is the code that looks most correct
