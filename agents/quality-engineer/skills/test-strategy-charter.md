---
name: test-strategy-charter
description: >
  Create a testing strategy for a new feature or AI-powered component. Use
  this skill when a new feature lands in sprint, when deciding what to
  automate vs. evaluate vs. test manually vs. monitor, when defining quality
  thresholds for an AI feature, or when the team asks "how should we test this?"
---

# Test Strategy Charter

Define what to test, how to test it, and what "good enough" looks like — before implementation starts.

## Step 1: Classify the feature

**Deterministic or non-deterministic?**
- Deterministic (same input → same output): standard assertion-based testing
- Non-deterministic (AI/LLM output): eval frameworks with thresholds

**Risk zone:**
- Green (UI, config, docs): static analysis + AI-supplemental tests
- Yellow (data transforms, APIs, state): test-first + adversarial review + integration tests
- Red (auth, payments, security, data at scale): all six layers + production monitoring

## Step 2: Define the testing approach

```markdown
## Test Strategy: [Feature name]

**Risk zone:** Green | Yellow | Red
**Deterministic:** Yes | No

### Automate
- Unit tests for: [specific functions]
- Integration tests for: [specific integrations]

### Evaluate (AI features only)
- Quality dimensions: [relevance / faithfulness / coherence / safety]
- Eval framework: [PromptFoo / DeepEval / Ragas / LangSmith]
- Pass thresholds: [e.g., relevance > 0.85, safety = 1.0]

### Test manually
- Exploratory scenarios: [list]
- Persona journeys: [list]
- Accessibility: [if applicable]

### Monitor in production
- Metrics: [error rate, latency, output quality sample]
- Alerts: [thresholds that trigger investigation]

### Acceptable variance
[For non-deterministic features: what range of outputs is acceptable?]
```

## Step 3: Write test expectations first

Before any implementation:
- Write test descriptions in plain language
- Write assertion stubs with expected values
- List edge cases the AI coder is likely to miss

Do NOT look at any implementation before writing these.

## Gotchas
- Define "acceptable variance" before the feature ships — after shipping, any variance becomes the baseline
- Eval thresholds require sign-off from product + legal before finalizing
- For AI features, the test strategy must cover what happens when the model is updated
- Manual testing cannot keep pace with agentic PR velocity — automate or evaluate everything possible
