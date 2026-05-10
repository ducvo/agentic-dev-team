# Search Strategies

Load this reference for advanced query techniques and source discovery patterns.

## Query Formulation

### Vary query phrasing
For each sub-question, try 3-5 variations:
- Technical terminology: `"retrieval augmented generation" architecture`
- Plain language: `how do AI agents search the web and use results`
- Question form: `what is the best approach for agent-driven research`
- Specific + domain: `"deep research" agent SKILL.md 2026`
- Negation/contrast: `limitations of single-step retrieval vs iterative`

### Target primary sources
- `"[topic] official documentation"`
- `"[topic] specification" OR "[topic] RFC"`
- `"[topic] peer reviewed" OR "[topic] study" OR "[topic] paper"`
- `site:arxiv.org [topic]` or `site:github.com [topic]`
- `"[topic]" filetype:pdf` for papers and reports

### Temporal targeting
- Add year: `[topic] 2026` or `[topic] latest`
- Use date ranges when available in search tools
- For rapidly evolving topics, prioritize last 6-12 months
- For established topics, foundational papers may be more valuable than recent commentary

## Source Discovery Patterns

### Follow the citation chain
1. Find a good source on the topic
2. Check what it cites (backward references) — these are often the primary sources
3. Check who cites it (forward references via Google Scholar "Cited by") — these are newer work building on it
4. Repeat for the most relevant citations

### Identify authoritative domains
- `.gov`, `.edu`, `.ac.uk` — tend toward accuracy, may be slow to update
- Official project sites (e.g., `docs.python.org`, `react.dev`) — authoritative for that project
- Standards bodies (W3C, IETF, ISO, NIST) — authoritative for standards
- Preprint servers (arxiv.org) — cutting edge but not peer-reviewed

### Cross-reference strategy
For each major claim, find sources that:
- Are independent (don't cite each other)
- Come from different types (e.g., a paper + official docs + practitioner blog)
- Were published at different times (consistency over time = stronger evidence)

## Research Scenarios

### Rapidly evolving topics (AI, crypto, regulations)
- Prioritize sources from last 6 months
- Check official changelogs and release notes
- Note when information might already be outdated
- Search for "[topic] breaking changes" or "[topic] deprecated"

### Controversial or contested topics
- Search explicitly for opposing viewpoints
- Look for systematic reviews that survey the debate
- Identify the strongest arguments on each side
- Note which sources might have advocacy positions

### Technical implementation questions
- Official documentation first
- GitHub issues and discussions for real-world edge cases
- Stack Overflow (check date, votes, accepted status)
- Verify against actual behavior when possible

### Comparative research ("X vs Y")
- Use same evaluation criteria for all options
- Search for head-to-head comparisons by neutral parties
- Check for vendor bias in comparison articles
- Note what each option is optimized for — "better" depends on context
