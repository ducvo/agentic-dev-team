---
name: eval-suite-builder
description: >
  Build an evaluation suite for an AI-powered feature. Use this skill when
  setting up quality measurement for a non-deterministic AI feature, when
  traditional assertion-based tests are insufficient, when defining pass/fail
  thresholds for LLM output, or when the team asks "how do we know if the AI
  output is good enough?"
---

# Eval Suite Builder

Build repeatable quality measurement for non-deterministic AI features.

## Why evals, not tests
Traditional tests assert exact outputs. AI features produce variable outputs. Evals measure quality dimensions against thresholds — "is this output relevant enough?" not "is this output exactly X?"

## Step 1: Define quality dimensions

| Dimension | Definition | Typical threshold |
|---|---|---|
| Relevance | Does the output address the input? | > 0.85 |
| Faithfulness | Is the output grounded in provided context? | > 0.90 |
| Coherence | Is the output logically consistent? | > 0.90 |
| Safety | Does the output avoid harmful content? | = 1.0 |
| Accuracy | Is factual content correct? | > 0.95 |
| Completeness | Does the output cover all required elements? | > 0.80 |

## Step 2: Build the test input set

For each quality dimension, create inputs covering:
- **Normal cases**: representative real-world inputs
- **Edge cases**: boundary inputs, unusual but valid
- **Adversarial cases**: inputs designed to expose failures (prompt injection, off-topic)
- **Regression cases**: inputs from past production failures

Minimum: 20 inputs per dimension.

## Step 3: Get threshold sign-off

Before wiring thresholds into CI, get explicit sign-off from:
- **Product** — do these thresholds reflect acceptable user experience?
- **Legal / Compliance** — for safety, accuracy, or regulated content dimensions, are these thresholds sufficient?

Document the sign-off:
```markdown
## Threshold sign-off: [Feature name]
| Dimension | Threshold | Approved by | Date |
|---|---|---|---|
| Safety | = 1.0 | [name] | [date] |
| Accuracy | > 0.95 | [name] | [date] |
```

Do not merge eval gates into CI until this sign-off is recorded.

## Step 4: LLM-as-judge prompt template

```
You are evaluating an AI system's output for [quality dimension].

Input: {input}
Output: {output}

Score from 0.0 to 1.0:
- 1.0: [definition of perfect score]
- 0.5: [definition of acceptable score]
- 0.0: [definition of failing score]

Return only JSON: {"score": 0.0-1.0, "reason": "one sentence"}
```

## Step 5: Wire into CI

```yaml
eval-gate:
  trigger: PR modifies src/ai/** or prompts/**
  run: npx promptfoo eval --config evals/config.yaml
  fail-if: any dimension below threshold
  report: post results as PR comment
```

## Output format

```markdown
## Eval Suite: [Feature name]

### Quality dimensions
| Dimension | Threshold | Rationale |

### Test inputs
[20+ inputs per dimension: normal / edge / adversarial / regression]

### Scoring rubrics
[LLM-as-judge prompts per dimension]

### Baseline
[Run before any changes to establish baseline scores]
```

## Gotchas
- Use a different model than the one being evaluated for LLM-as-judge (avoid shared biases)
- Thresholds set too high cause constant false failures; too low give false confidence — calibrate against human ratings
- Eval suites must be updated when the model is updated — behavioral drift is a deployment event
- Never use the same session to generate test inputs and evaluate outputs
