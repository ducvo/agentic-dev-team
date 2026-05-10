---
name: bug-report
description: >
  Write a structured bug report for a defect found in AI-generated or
  human-written code. Use this skill when a test fails, when a production
  incident is investigated, when an adversarial review finds a defect, or
  when an agent needs enough context to reproduce and fix a bug correctly.
  Also use when a vague bug report needs to be improved before assignment.
---

# Bug Report

Write bug reports that give an agent or engineer everything needed to reproduce, understand, and fix the defect without back-and-forth.

## Why structure matters for AI agents
Vague bug reports produce vague fixes. An agent given "the login doesn't work" will guess at the cause and likely fix the wrong thing. A structured report with exact reproduction steps, expected vs. actual behaviour, and the relevant spec AC produces a targeted, correct fix.

## Bug report template

```markdown
## Bug: [Short title — what is wrong, not what should happen]

**Severity:** Critical | High | Medium | Low
**Risk zone:** Red | Yellow | Green
**Code origin:** AI-generated | Human-written | Unknown
**Spec reference:** [Link to spec or AC number]

### Description
[One paragraph: what is broken and why it matters]

### Steps to reproduce
1. [Exact step]
2. [Exact step]
3. [Exact step]

### Expected behaviour
[What the spec / acceptance criteria says should happen]
AC reference: [AC # from spec]

### Actual behaviour
[What actually happens — include error messages, stack traces, or output verbatim]

### Environment
- Branch / commit: [hash]
- Input data: [exact input that triggers the bug]
- Dependencies: [relevant versions if applicable]

### Root cause hypothesis
[If known: what in the code is causing this — be specific about file and line if possible]
[If unknown: "Unknown — requires investigation"]

### Fix constraints
- Must not change: [files or behaviour that must remain unchanged]
- Must satisfy: [AC # that this fix must pass]
- Related tests to update: [list or "none"]
```

## Severity guide

| Severity | Condition |
|---|---|
| Critical | Data loss, security vulnerability, system unavailable, incorrect financial calculation |
| High | Core feature broken, no workaround, affects most users |
| Medium | Feature partially broken, workaround exists |
| Low | Minor UI issue, edge case, cosmetic |

## For AI-generated code bugs — additional fields

```markdown
### AI generation context
- Agent / tool that generated this code: [name]
- Spec used: [link]
- Likely cause category: Spec gap | Context gap | Model limitation | Tautological test missed it
```

This helps the team identify whether the fix is in the code, the spec, or the context file.

## Gotchas
- "Expected: it should work" is not an expected behaviour — quote the spec or AC
- Stack traces belong in the report verbatim — don't paraphrase them
- If the bug was caught by a tautological test that passed, note this — the test needs to be fixed too
- Severity must reflect business impact, not technical complexity
