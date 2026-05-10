---
name: agent-cost-investigator
description: >
  Diagnose LLM cost spikes and generate optimization recommendations. Use
  this skill when investigating unexpected LLM spend, when a pipeline's token
  usage is higher than expected, when a cost alert fires, or when preparing
  a monthly cost review. Also use when asked "why is our AI bill so high?"
---

# Agent Cost Investigator

Diagnose LLM cost spikes and optimize spend without blanket bans that kill legitimate workloads.

## Why this is hard
CI/CD pipelines remove the human pacing mechanism. An agent in a ReAct loop grows context O(n²) per step. A single misconfigured pipeline can generate millions of tokens overnight. Without per-step attribution, the only tool is blanket bans.

## Investigation workflow

### Step 1: Attribute the spend
Break down cost by:
- Team / repo
- Pipeline / workflow name
- Agent step (which step is expensive?)
- Model tier (are expensive models used for cheap tasks?)
- Time pattern (spike vs. sustained growth vs. gradual drift)

If attribution data isn't available — that's the first fix. Add metadata tags to all LLM calls.

### Step 2: Identify the pattern

| Pattern | Likely cause | Fix |
|---|---|---|
| Single step dominates | Context too large | Compress context, cheaper model, add caching |
| Cost grows with PR count | No caching on repeated prompts | Semantic cache or prompt prefix cache |
| Overnight spike | Infinite retry loop | Add retry budget + circuit breaker |
| Gradual drift upward | Context window growing | Add context pruning, reset sessions |
| One repo dominates | No per-repo budget | Add per-repo hard cap |

### Step 3: Apply optimizations

**Model routing** (highest impact):
- Tier 1 (cheap): linting, formatting, doc generation
- Tier 2 (balanced): feature implementation, test generation, code review
- Tier 3 (expensive): complex architecture, security review, novel problems

**Caching** (second highest impact):
- Semantic cache: deduplicate similar prompts (saves 30–60% on repeated workflows)
- Prompt prefix cache: share common context across calls in same pipeline

**Budget controls**:
- Hard token limit per pipeline job (fail fast, don't let loops run overnight)
- Retry budget: max 3 retries with exponential backoff
- Circuit breaker: stop pipeline if cost exceeds threshold

## Output format

```
## Cost Investigation Report

**Period:** [date range]
**Total spend:** [amount] vs. baseline: [+/- %]

### Top cost drivers
1. [Repo/pipeline/step]: [cost] ([% of total]) — [root cause]

### Root cause
[Specific finding: which step, why it's expensive, what changed]

### Optimization recommendations
| Action | Estimated saving | Effort | Risk |
|---|---|---|---|

### Immediate actions (this week)
- [Action]

### Structural fixes (this sprint)
- [Action]
```

## Gotchas
- Without per-step attribution, you're guessing — add metadata tagging before investigating
- The most expensive step is rarely the most obvious one — always break down by step
- Hard budget caps must be set above normal operating range or they'll block legitimate workloads
- A cost spike that resolves itself is usually a loop that hit a retry limit — find and fix the root cause
