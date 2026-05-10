---
name: regression-test-generator
description: >
  Generate regression tests after a bug fix or production incident to prevent
  recurrence. Use this skill after a defect is fixed, after a production
  incident is resolved, when closing a bug report, or when a QA review finds
  a gap in test coverage that allowed a defect to reach production.
---

# Regression Test Generator

Turn a fixed bug into a test that prevents it from coming back.

## Why regression tests matter for AI-generated code
AI agents will regenerate similar code in future sessions. Without a regression test, the same defect reappears — often in a slightly different form that passes existing tests. Regression tests are the memory that prevents agents from repeating mistakes.

## Workflow

### Step 1: Understand the defect
From the bug report, extract:
- The exact input that triggered the bug
- The incorrect output that was produced
- The correct output the spec requires
- The root cause (spec gap / context gap / logic error / missing boundary)

### Step 2: Write the regression test

The test must:
- Use the **exact input** from the bug report (not a similar input)
- Assert the **correct output** from the spec (not the buggy output)
- Have a name that describes the bug, not the feature: `should_not_return_null_when_list_is_empty` not `test_list_function`
- Include a comment linking to the bug report or incident

```typescript
// Regression: Bug #NNN — [short description]
// Previously returned [wrong output] when [condition]
it('should [correct behaviour] when [condition that triggered bug]', () => {
  // Arrange — exact input from bug report
  const input = [exact value];

  // Act
  const result = functionUnderTest(input);

  // Assert — correct output per spec AC #N
  expect(result).toEqual([correct expected value]);
});
```

### Step 3: Verify the test fails on the unfixed code
Before merging the fix, confirm the regression test fails on the pre-fix code. A regression test that passes on buggy code is worthless.

### Step 4: Add to the permanent test suite
- Place in the same test file as the function under test
- Do not create a separate "regression" test file — regression tests are just tests
- If the bug was in an AI-generated feature, add the scenario to the eval suite as an adversarial case

### Step 5: Update context files if root cause was a spec or context gap
- If the bug was caused by a **spec gap**: update the spec's acceptance criteria to include this case
- If the bug was caused by a **context gap**: update AGENTS.md with the gotcha so future agent sessions don't repeat it

## Output format

```markdown
## Regression Test: [Bug title]

**Bug reference:** #NNN
**Root cause:** Spec gap | Context gap | Logic error | Missing boundary
**Test location:** [file path]

### Test case
[Code block with the regression test]

### Verification
- [ ] Test fails on pre-fix code
- [ ] Test passes on fixed code
- [ ] Added to permanent test suite

### Follow-up
- [ ] Spec updated (if spec gap)
- [ ] AGENTS.md updated (if context gap)
- [ ] Eval suite updated (if AI feature)
```

## Gotchas
- A regression test that passes on buggy code provides false confidence — always verify it fails first
- Name the test after the bug, not the feature — future engineers need to understand why this test exists
- If the same bug recurs in a different form, the regression test was too narrow — broaden the input range
- For AI features, add the bug's input to the eval suite as an adversarial case — not just the unit test suite
