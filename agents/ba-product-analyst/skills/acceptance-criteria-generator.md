---
name: acceptance-criteria-generator
description: >
  Generate acceptance criteria for a requirement or business rule. Use this
  skill when writing acceptance criteria for a spec, when existing criteria
  are vague or incomplete, when converting business rules into testable
  conditions, or when an agent run failed because the criteria weren't
  specific enough. Also use when asked "how do we know this is done?"
---

# Acceptance Criteria Generator

Write acceptance criteria that are concrete enough for an AI agent to verify and a QA engineer to test.

## What makes a good acceptance criterion
- **Binary**: pass or fail — no partial credit, no spectrum
- **Concrete**: specific input and expected output, not a description of behaviour
- **Business-facing**: tests what the system should do, not how it does it
- **Independent**: each criterion can be tested in isolation

## Workflow

### Step 1: Extract the business rules
From the spec or requirement, list every distinct business rule:
- What must always be true?
- What must never happen?
- What changes based on input conditions?
- What are the boundary values?

### Step 2: Generate criteria by category

**Happy path** — the normal, expected flow:
```
Given [normal input]
When [action]
Then [expected result]
```

**Boundary conditions** — values at the edges of valid ranges:
```
Given [minimum valid value] → [expected result]
Given [maximum valid value] → [expected result]
Given [value just outside range] → [expected error]
```

**Error cases** — invalid input, missing data, system failures:
```
Given [invalid input] → [expected error message / behaviour]
Given [missing required field] → [expected validation error]
Given [downstream service unavailable] → [expected fallback]
```

**Edge cases** — unusual but valid scenarios a domain expert would know:
```
Given [unusual but valid input] → [expected result]
```

### Step 3: Format as a table

```markdown
### Acceptance criteria

| # | Scenario | Input | Expected output |
|---|----------|-------|----------------|
| 1 | Happy path | [concrete example] | [concrete expected] |
| 2 | Boundary — minimum | [min value] | [expected] |
| 3 | Boundary — maximum | [max value] | [expected] |
| 4 | Invalid input | [invalid example] | [error message] |
| 5 | Missing required field | [example] | [validation error] |
| 6 | [Domain edge case] | [example] | [expected] |
```

### Step 4: Validate each criterion
For each criterion, ask:
- [ ] Can I write a test for this without looking at the implementation?
- [ ] Would a wrong implementation fail this criterion?
- [ ] Is the expected output specific enough that two people would agree on pass/fail?

If any answer is no, rewrite the criterion.

## Common anti-patterns to avoid

| Anti-pattern | Example | Fix |
|---|---|---|
| Vague quality attribute | "Response should be fast" | "Response time < 200ms for inputs under 1MB" |
| Implementation detail | "The function should call validateEmail()" | "Invalid email format returns 400 with message 'Invalid email'" |
| Compound criterion | "User can log in and see their dashboard" | Split into two separate criteria |
| Untestable assertion | "The system handles errors gracefully" | "When DB is unavailable, returns 503 with retry-after header" |

## Gotchas
- If you can't write a concrete input/output example, the requirement isn't understood yet — go back to the stakeholder
- AI agents use acceptance criteria as their verification target — vague criteria produce vague verification
- Error cases are the most commonly omitted category and the most likely source of production incidents
- Boundary conditions are where AI-generated code fails most often — always include them
