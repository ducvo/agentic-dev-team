---
name: pr-writer
description: >
  Write a structured pull request description for a completed agent task.
  Use this skill after completing a coding task to document what was changed,
  why, which acceptance criteria were satisfied, and what the reviewer should
  focus on. Always use before opening a PR.
---

# PR Writer

Write PR descriptions that give human reviewers the context they need to review efficiently.

## Before writing the PR description

### Step 1: Verify commits are clean
- [ ] Each commit covers one concern (feature / fix / refactor — not mixed)
- [ ] Commit messages follow conventional commits format
- [ ] No unrelated files were staged
- [ ] No secrets, credentials, or PII in any commit

If commits are messy, clean them up before opening the PR.

### Step 2: Self-review against the spec
Go through each acceptance criterion and verify:
- Does the code produce the expected output for the given input?
- Are error cases handled as specified?
- Is anything in the "Not Included" section accidentally implemented?

If any AC fails, fix it before opening the PR. Do not open a PR with a known failing AC.

### Step 3: Write the PR description

```markdown
## What
[One sentence: what this PR does]

## Why
[Which spec objective this satisfies — link to spec or ticket]

## Changes
- [File or module]: [what changed and why]
- [File or module]: [what changed and why]

## Acceptance criteria
| Criterion | Status | Notes |
|---|---|---|
| [AC from spec] | ✅ PASS / ❌ FAIL | [how verified] |
| [AC from spec] | ✅ PASS / ❌ FAIL | [how verified] |

## Scope
**Changed:** [list of files modified]
**Explicitly NOT changed:** [files in scope that were intentionally left unchanged, and why]

## Testing
- [ ] Unit tests: [pass/fail, coverage %]
- [ ] Integration tests: [pass/fail or N/A]
- [ ] Manual verification: [what was checked]

## Reviewer notes
[Anything the human reviewer should pay special attention to]
[Flag any areas of uncertainty or where a second opinion is valuable]

## Out of scope (not implemented)
[List anything from the spec's "Not Included" section that was confirmed not implemented]
```

## Rules
- Every AC from the spec must appear in the table with a status
- If any AC is FAIL, do not open the PR — fix it first or escalate
- The "Explicitly NOT changed" section prevents reviewers from wondering if something was missed
- Reviewer notes must be honest — flag uncertainty, don't hide it

## Gotchas
- A PR with no spec reference is a red flag to reviewers — always link the spec or ticket
- "All tests pass" is not sufficient — list which tests and what they cover
- If you wrote the tests yourself, flag this in reviewer notes (tautological test risk)
- Never mark an AC as PASS if you're uncertain — mark it as "Needs verification" instead
