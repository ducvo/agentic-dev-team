---
name: pr-review
description: >
  Review a pull request from an AI coding agent for implementation quality,
  architectural consistency, and spec adherence. Use this skill when reviewing
  an agent-generated PR before merge, when checking whether an agent stayed
  within scope, or when a PR looks locally correct but might introduce global
  inconsistencies. Complements the Architect's ai-code-review (risk scoring)
  with implementation-level quality checks.
---

# PR Review

Review agent-generated PRs for implementation quality — the things that look fine locally but break the codebase globally.

## Before starting
- Read the spec the agent was given (link should be in the PR description)
- Read the acceptance criteria — these are the ground truth for correctness
- Open a fresh context session — don't review in the same session that generated the code

## Review checklist

### Spec adherence
- [ ] PR description references the spec or ticket
- [ ] Every acceptance criterion is addressed — check each one explicitly
- [ ] Nothing was built that's in the "Not Included" scope boundary
- [ ] Stop conditions were respected (no scope creep into adjacent files)

### Architectural consistency
- [ ] Does this follow the existing patterns in the codebase? (check 2–3 similar features)
- [ ] Does it duplicate logic that already exists in a utility or shared module?
- [ ] Does it respect layer boundaries (no business logic in controllers, no DB calls from handlers)?
- [ ] Does it conflict with any accepted ADR?
- [ ] Are new abstractions justified, or could existing ones be reused?

### Implementation quality
- [ ] Are error paths handled explicitly (not silently swallowed)?
- [ ] Are boundary conditions handled (empty lists, null, zero, max values)?
- [ ] Is the code readable without needing to understand the agent's reasoning?
- [ ] Are there any magic numbers or hardcoded values that should be constants or config?

### Test coverage
- [ ] Were tests written before or after the implementation? (check commit order)
- [ ] Do tests assert expected behaviour or just verify what the code does? (tautological test check)
- [ ] Are error paths and boundary conditions tested — not just the happy path?
- [ ] Would these tests catch a regression if the implementation changed?

### Security (flag for Architect review if any apply)
- [ ] New dependency added? (verify it exists in registry, check for CVEs)
- [ ] User input used without validation?
- [ ] Sensitive data logged or exposed in error messages?
- [ ] Auth/permission checks present on new endpoints?

## Output format

```
## PR Review: [PR title]

**Spec:** [link]
**Verdict:** Approve | Request changes | Escalate to Architect

### Spec adherence
- AC1: ✅ / ❌ [notes]
- AC2: ✅ / ❌ [notes]

### Architectural issues
[List, or "None found"]

### Implementation issues
[List, or "None found"]

### Test quality
[Assessment]

### Security flags
[List for Architect, or "None"]

### Required changes before merge
- [ ] [Specific change]
```

## Gotchas
- AI PRs look locally clean — good formatting, consistent naming — but hide global inconsistencies; always check architectural fit
- A PR that passes CI is not a PR that's ready to merge — CI doesn't check spec adherence or architectural consistency
- If the agent wrote both the code and the tests in the same session, treat test coverage as unverified until adversarial review is done
- Scope creep is the most common agent PR failure — check the "Not Included" list explicitly
