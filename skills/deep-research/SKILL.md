---
name: deep-research
description: >
  Conduct rigorous, citation-backed research on any topic using iterative
  search, evidence grading, and structured synthesis. Use this skill when
  asked to research, investigate, deep-dive, fact-check, compare options,
  review literature, or answer questions requiring comprehensive, verified
  information from multiple sources — even if the user doesn't say "research"
  explicitly. Prioritizes evidence quality and calibrated confidence over
  speed or volume.
---

# Deep Research

Iterative, evidence-graded research methodology. The goal is not just to find information but to make you *more calibrated* about what is known, what is uncertain, and what is unknown.

## Core Principles

1. **Iterate, don't march** — Search and reasoning interleave in a loop, not a linear pipeline
2. **Grade evidence, not just sources** — A claim's strength depends on the evidence behind it, not just who said it
3. **Cite everything** — No unsourced assertions; every claim gets a numbered reference
4. **Quantify uncertainty** — "I don't know" and "sources conflict" are valid findings
5. **Triangulate** — Verify claims across independent sources; watch for circular citation

## Workflow

### 1. Scope

Before searching, define:
- **Core question(s)** — What specifically needs answering?
- **Depth level** — Quick check, standard review, or exhaustive deep-dive?
- **Recency needs** — Is timeliness critical? (e.g., API versions, regulations, current events)
- **Success criteria** — What would constitute a sufficient answer?

If scope is ambiguous, ask the user to clarify before proceeding.

**Adaptive depth:** Not every question needs exhaustive research. Calibrate effort:
- Quick check: 2-3 sources, single search round
- Standard review: 5-10 sources, 2-3 search rounds
- Deep-dive: 10+ sources, iterate until coverage is sufficient

### 2. Search → Evaluate → Deepen (iterate)

This is a loop, not a sequence. Repeat until coverage is sufficient.

**Search:**
- Use 3-5 query variations per sub-question (technical terms + plain language)
- Search for primary sources first: official docs, papers, specs, data
- Follow citation chains — what does this source cite? who cites it?
- Use multiple search tools when available (web search, docs, codebase, APIs)

**Evaluate coverage:**
After each search round, ask:
- Which sub-questions are well-covered (2+ independent sources)?
- Which have only a single source or weak evidence?
- What gaps remain? What follow-up questions emerged?
- Are there contradictions that need resolution?

**Deepen:**
- For gaps: formulate new queries targeting the specific missing information
- For contradictions: search for sources that explain the disagreement
- For weak evidence: look for primary sources behind secondary claims
- Stop when: additional searches yield diminishing returns, or all sub-questions have adequate coverage

### 3. Grade Evidence

For each major claim, assess using the hierarchy in [references/EVIDENCE-QUALITY.md](references/EVIDENCE-QUALITY.md):

**Quick grading:**
- **Strong**: 3+ independent authoritative sources agree, primary evidence available
- **Moderate**: 2 independent sources or 1 highly authoritative primary source
- **Weak**: Single source, secondary only, or sources with potential bias
- **Contested**: Sources actively disagree; report all positions
- **Unknown**: Insufficient evidence found; state this explicitly

**Bias checks** (apply to every key source):
- Who funded or published this? Any conflicts of interest?
- Is this source advocating for a position or reporting findings?
- Are opposing viewpoints acknowledged?
- Is the claim being repeated from a single origin (citation laundering)?

### 4. Synthesize

Structure output using the template at [assets/REPORT-TEMPLATE.md](assets/REPORT-TEMPLATE.md).

Key synthesis rules:
- Lead with findings, not process
- State confidence level for each major finding
- Report conflicts with citations for each position and analysis of why they differ
- Separate facts from interpretation
- List gaps and limitations honestly
- End with numbered citations including URL and access date

### 5. Cite

Every factual claim must have a numbered inline citation:

```
The rate limit is 60 requests/minute [1], with higher limits for enterprise [2].

[1] "Rate Limits" — OpenAI API Docs, https://..., accessed 2026-04-03
[2] "Enterprise FAQ" — OpenAI, https://..., accessed 2026-04-03
```

Citation must include: source name, URL (if web), access date, publication date (if available).

## Gotchas

- Wikipedia is a starting point, not an endpoint — follow its citations to primary sources
- High Google ranking ≠ high evidence quality; SEO-optimized content farms rank well
- "A study found..." is meaningless without which study, by whom, published where, sample size
- Multiple sources repeating the same claim may trace back to a single origin — check for circular citation
- Absence of evidence is not evidence of absence — state when you couldn't find information, don't skip the topic
- Older sources aren't automatically wrong, but check if consensus has shifted
- Industry-funded research isn't automatically biased, but conflicts of interest should be noted

## Search Strategy Reference

For advanced query techniques and source discovery patterns, see [references/SEARCH-STRATEGIES.md](references/SEARCH-STRATEGIES.md).
