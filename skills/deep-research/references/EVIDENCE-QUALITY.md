# Evidence Quality Framework

Load this reference when grading evidence for research findings.

## Evidence Hierarchy

Stronger evidence types should be preferred over weaker ones. When sources conflict, higher-tier evidence generally wins.

### Tier 1: Primary Empirical Evidence
- Systematic reviews and meta-analyses
- Randomized controlled trials / controlled experiments
- Large-scale observational studies with proper methodology
- Official statistics from authoritative bodies (census, WHO, NIST)

### Tier 2: Primary Source Documents
- Official documentation, specifications, standards (RFCs, W3C specs)
- Peer-reviewed research papers (single studies)
- Government and regulatory filings, court records
- Raw datasets with documented methodology
- First-party announcements (company blog posts about their own product)

### Tier 3: Expert Analysis
- Analysis by recognized domain experts with stated methodology
- Technical books by established authorities
- Conference proceedings and workshop papers
- Investigative journalism with named sources and evidence

### Tier 4: Secondary Reporting
- News articles reporting on primary sources
- Well-sourced blog posts by practitioners
- Industry reports from reputable firms (Gartner, McKinsey — note potential bias)
- Stack Overflow answers (check votes, dates, accepted status)

### Tier 5: Weak Evidence
- Unsourced blog posts, opinion pieces, editorials
- Forum posts, social media, comments
- AI-generated summaries (may hallucinate)
- Content farms, SEO-optimized listicles
- Undated or anonymous sources

## Grading Claims

For each major claim in your research, assign a grade:

| Grade | Criteria | Action |
|-------|----------|--------|
| **Strong** | 3+ independent Tier 1-2 sources agree; no conflicts | State with high confidence |
| **Moderate** | 2 independent sources (Tier 1-3) or 1 Tier 1 source | State with moderate confidence |
| **Weak** | Single Tier 2-3 source, or multiple Tier 4-5 sources | State with low confidence; flag as needing verification |
| **Contested** | Tier 1-3 sources actively disagree | Report all positions with citations; analyze why they differ |
| **Unknown** | No Tier 1-4 evidence found | State explicitly that evidence is insufficient |

## Bias Detection Checklist

Apply to every key source:

- [ ] **Funding**: Who funded this work? Any financial conflicts of interest?
- [ ] **Advocacy**: Is the source advocating for a position or reporting findings?
- [ ] **Selection**: Does the source cherry-pick data or present a balanced view?
- [ ] **Recency**: Is this outdated? Has consensus shifted since publication?
- [ ] **Independence**: Does this source cite independent evidence, or just repeat others?
- [ ] **Methodology**: For empirical claims — is the methodology described and sound?
- [ ] **Retraction**: Has this been corrected, retracted, or superseded?

## Common Evidence Pitfalls

**Citation laundering**: Claim A cites Blog B, which cites News Article C, which cites the original Tweet D. The claim appears well-sourced but rests on a single weak origin. Always trace claims back to their primary source.

**Survivorship bias**: "All successful companies do X" ignores failed companies that also did X. Look for base rates and counterexamples.

**Correlation vs causation**: "Countries that eat more chocolate win more Nobel prizes" is a correlation, not a causal finding. Flag when sources conflate the two.

**Appeal to authority**: An expert's opinion outside their domain of expertise is Tier 4-5, not Tier 3. A Nobel physicist's views on economics carry no special weight.

**Outdated consensus**: Scientific and technical consensus shifts. A 2015 best practice may be a 2026 anti-pattern. Always check for more recent evidence.
