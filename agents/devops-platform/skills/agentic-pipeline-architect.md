---
name: agentic-pipeline-architect
description: >
  Design CI/CD pipelines for AI agent workloads with built-in verification
  layers. Use this skill when setting up a pipeline for an AI coding agent,
  when adding verification gates to an existing pipeline, when a pipeline is
  shipping agent-generated code without adequate checks, or when asked "how
  should we gate AI-generated PRs?"
---

# Agentic Pipeline Architect

Design pipelines that correctly govern agent-generated code — human-code pipeline patterns are insufficient.

## Why agent pipelines are different
Traditional CI asks: "Did it pass?" Agent pipelines must ask:
- Does the code match the spec?
- Did the agent also write the tests? (tautological test risk)
- Does this dependency actually exist in any registry?
- Is the cost within budget?

## Verification layers

Apply layers proportional to risk zone:

| Layer | Green | Yellow | Red |
|---|---|---|---|
| Lint + type check | ✓ | ✓ | ✓ |
| Unit tests | ✓ | ✓ | ✓ |
| Spec adherence check | | ✓ | ✓ |
| Dependency provenance | | ✓ | ✓ |
| Adversarial test review | | ✓ | ✓ |
| Integration tests | | ✓ | ✓ |
| Security scan (Semgrep/Snyk) | | ✓ | ✓ |
| Cost budget gate | ✓ | ✓ | ✓ |
| Human review required | | Medium risk | Always |
| 2-person review + threat model | | | Always |

## Pipeline template

```yaml
name: Agent PR Pipeline

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  # Layer 1: Fast gates (always)
  lint-and-type-check:
    runs-on: ubuntu-latest
    steps:
      - run: npm run lint && npm run typecheck

  unit-tests:
    runs-on: ubuntu-latest
    steps:
      - run: npm test -- --coverage
      - name: Fail if coverage drops
        run: npx coverage-check --threshold 80

  cost-gate:
    steps:
      - name: Check token usage vs budget
        run: scripts/check-token-budget.sh

  # Layer 2: Agent-specific gates
  dependency-provenance:
    steps:
      - name: Verify all new deps exist in registry
        run: scripts/verify-deps.sh
      - name: Check for known CVEs
        run: npx audit-ci --moderate

  security-scan:
    steps:
      - run: npx semgrep --config=auto src/

  # Layer 3: Spec adherence (Yellow/Red only)
  spec-adherence:
    if: contains(github.event.pull_request.labels.*.name, 'ai-generated')
    steps:
      - name: Check PR description has spec reference
        run: scripts/check-spec-link.sh
      - name: Run acceptance criteria tests
        run: npm run test:acceptance

  # Layer 4: Human review routing
  review-routing:
    steps:
      - name: Calculate risk score and assign reviewers
        run: scripts/route-review.sh
        # Outputs: risk-level, required-reviewers
```

## Rollback strategy for model deployments
Unlike code, model behavior can shift without a code change:
- Tag every deployment with model version + prompt hash
- Run eval suite on canary before full rollout
- Keep previous model version hot for 24h after deployment
- Define behavioral diff threshold that triggers automatic rollback

## Gotchas
- Agents can generate tests designed to pass regardless of correctness — the test suite is not a reliable gate without human review of test quality
- Hallucinated dependency names are a real supply chain risk — always verify new packages exist in the registry
- Cost gates must run early (fail fast) not at the end of the pipeline
- "Did CI pass?" is not sufficient for agent-generated code — spec adherence is a separate check
