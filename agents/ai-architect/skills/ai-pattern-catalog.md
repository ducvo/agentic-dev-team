---
name: ai-pattern-catalog
description: >
  Reference catalog of AI-specific design patterns for systems that integrate
  LLMs. Use this skill when designing or reviewing an AI-integrated system,
  choosing between architectural approaches for LLM features, deciding how to
  handle hallucinations or unsafe outputs, optimizing LLM cost and latency,
  or explaining AI system architecture to stakeholders. Also use when asked
  "what pattern should we use for X?" in an AI system context.
---

# AI Pattern Catalog

Reference for AI-specific design patterns. These extend (not replace) classical patterns — GoF and SOLID still apply; these address problems the GoF never encountered.

## Prompting & Context Patterns

### RAG (Retrieval-Augmented Generation)
**Problem:** Model knowledge is stale, general, or lacks proprietary data.
**Solution:** At query time, retrieve relevant documents from an external store and inject them into the prompt. The model reasons over retrieved content rather than relying on training data alone.
**When to use:** Domain-specific Q&A, documentation assistants, any feature requiring up-to-date or proprietary knowledge.
**Trade-off:** Adds retrieval latency and a vector store dependency. Retrieval quality directly determines output quality.

### Few-Shot Prompting
**Problem:** Model output format, tone, or domain adaptation is inconsistent.
**Solution:** Provide 2–5 input/output examples in the prompt before the actual input. The model generalises from examples without retraining.
**When to use:** Enforcing output format, adapting to domain-specific terminology, reducing inconsistency in structured outputs.
**Trade-off:** Examples consume context tokens. More examples = better consistency but higher cost.

### Chain-of-Thought (CoT)
**Problem:** Model jumps to incorrect answers on complex reasoning tasks.
**Solution:** Instruct the model to "think step by step" before producing the final answer. Intermediate reasoning steps reduce premature pattern-matching.
**When to use:** Multi-step reasoning, mathematical problems, complex decision trees, any task where the model frequently produces plausible-but-wrong answers.
**Trade-off:** Increases output length and latency. Modern frontier models do this implicitly; explicit CoT is most valuable for smaller models.

### Role Prompting
**Problem:** Model response style, scope, or assumptions don't match the use case.
**Solution:** Assign a persona in the system prompt ("You are a senior security engineer reviewing code for vulnerabilities"). Constrains tone, content boundaries, and assumed expertise.
**When to use:** Specialised assistants, constraining response scope, enforcing professional tone.
**Trade-off:** Persona can be overridden by sufficiently strong user prompts — combine with guardrails for safety-critical applications.

---

## Responsible AI Patterns

### Output Guardrails
**Problem:** Model produces harmful, biased, or policy-violating content despite good prompts.
**Solution:** Post-generation layer that checks output against rules before delivery. Can block, modify, or flag. Implemented as classifiers, rule engines, or a second LLM.
**When to use:** Any user-facing AI feature, especially in regulated domains (legal, medical, financial).
**Trade-off:** Adds latency. Overly aggressive guardrails degrade usefulness. Tune thresholds carefully.

### LLM-as-Judge (Model Critic)
**Problem:** Automated testing can't catch hallucinations, bias, or quality regressions in LLM output.
**Solution:** A second model (same or different) reviews the primary model's output against defined criteria. Used in CI/CD pipelines to catch regressions when prompts, models, or configs change.
**When to use:** Automated QA for AI features, A/B testing prompt changes, evaluating model upgrades.
**Trade-off:** Doubles inference cost for evaluated calls. Use offline/async for CI, not in the hot path.
**Reference:** GitHub Copilot uses a second LLM to evaluate its primary model.

---

## AI-Ops Patterns

### Prompt-Model-Config Versioning
**Problem:** Uncontrolled prompt changes, model swaps, or config tweaks silently degrade output quality.
**Solution:** Treat each (prompt, model, config) combination as a versioned release. Tag it, run it against a golden dataset of test queries, gate deployment on quality metrics. Revert if metrics degrade.
**When to use:** Any production AI feature. Mandatory before model upgrades or prompt refactors.
**Trade-off:** Requires maintaining a golden dataset and evaluation pipeline. Investment pays off at the first silent regression it catches.

### Metrics-Driven AI-Ops
**Problem:** Traditional uptime/error-rate monitoring misses AI-specific failures (hallucination rate, output quality drift).
**Solution:** Track AI-specific metrics: latency, token usage, user acceptance rate, hallucination rate (via LLM-judge), cost per call. Set alerts on quality metrics, not just availability.
**When to use:** All production AI systems.
**Key metrics:** Latency p95, tokens/request, acceptance rate, hallucination rate, cost/call, daily active users per feature.

---

## Optimization Patterns

### Intelligent Model Routing
**Problem:** Sending all requests to the most capable (expensive) model wastes cost on simple tasks.
**Solution:** A lightweight routing layer classifies request complexity and routes to the appropriate model tier.

| Tier | Use for | Examples |
|---|---|---|
| Cheap/fast | Simple, deterministic tasks | Linting, formatting, doc generation, classification |
| Balanced | Standard tasks | Feature implementation, test generation, code review |
| Expensive | Complex reasoning | Architecture decisions, security review, novel problems |

**Trade-off:** Routing logic adds complexity. Misrouting a complex task to a cheap model produces bad output.

### Prompt Prefix Caching
**Problem:** Repeated calls with the same system prompt or context waste tokens and add latency.
**Solution:** Cache the expensive prefix (system instructions, few-shot examples, large context) so subsequent calls only process the new user input.
**When to use:** Any pipeline where multiple calls share a common prefix (most agentic pipelines).
**Impact:** Up to 85% latency reduction on large prompts (AWS Bedrock). Also reduces cost proportionally.

### Semantic Caching
**Problem:** Users ask semantically identical questions with different wording, generating redundant LLM calls.
**Solution:** Cache responses by semantic similarity (vector distance), not exact string match. Return cached response if a new query is sufficiently similar to a cached one.
**When to use:** High-volume Q&A systems, documentation assistants, customer support bots.
**Trade-off:** Cache invalidation is harder than exact-match caching. Stale cached responses can mislead users.

---

## Agentic Patterns

### Spec-Driven Development
**Problem:** Vibe coding (conversational AI iteration) loses context, produces scope creep, and creates unmaintainable code.
**Solution:** Before any agent execution: write requirements, create a technical design, break into discrete tasks. Each stage produces a persistent artifact. The spec is the source of truth — code is a disposable intermediate.
**When to use:** Any feature beyond a prototype. Mandatory for production code.
**Evidence:** AWS/Kiro cut a two-week feature to two days using this approach (BuiltIn, Feb 2026).

### Verification Loop
**Problem:** Agents accumulate unverified changes, making failures hard to isolate.
**Solution:** Build → verify (compile, test, lint) → fix → repeat at every step. Never proceed to the next step with a failing verification. Escalate if the same failure persists after 2 attempts.
**When to use:** All agentic coding workflows.

### Human-in-the-Loop Escalation
**Problem:** Agents proceed past ambiguity or scope boundaries, producing wrong or dangerous output.
**Solution:** Define explicit escalation triggers before starting. Agent stops and asks when: spec is ambiguous, task requires out-of-scope files, confidence is below threshold, or a security-sensitive change is required.
**When to use:** All agentic workflows. Escalation triggers belong in the spec, not discovered mid-execution.

## Gotchas

- RAG quality is bounded by retrieval quality — a bad retriever produces confidently wrong answers
- LLM-as-judge is not a substitute for human review on high-risk changes — it catches regressions, not novel failures
- Prompt versioning without a golden dataset is just version control — the dataset is what makes it useful
- Model routing misclassification is silent — monitor output quality per tier, not just routing decisions
- Spec-driven development requires upfront investment; the payoff is at maintenance time, not delivery time
