---
name: context-file-curator
description: >
  Update and maintain AGENTS.md, CLAUDE.md, or equivalent context files after
  agent sessions. Use this skill after completing a significant agent task,
  at the end of a sprint, when a new pattern is established, when a gotcha is
  discovered, or when an agent produced wrong output due to missing context.
  Also use to audit whether context files are stale.
---

# Context File Curator

Keep context files current so agents don't repeat mistakes or reinvent patterns.

## Why this matters
Agents have no persistent memory across sessions. Context files are the only mechanism for accumulating institutional knowledge. Stale context files are the #1 cause of pattern drift and repeated hallucinations.

## After each significant agent session, capture

### New patterns established
```markdown
## Patterns
- [Pattern name]: [one-line description] — see [file] for reference implementation
```

### Gotchas discovered
```markdown
## Gotchas
- [What the agent got wrong]: [correct approach]
- [Non-obvious constraint]: [explanation]
```

### Architectural decisions made
```markdown
## Decisions
- [Decision]: [rationale] — see ADR-NNN for full context
```

### Anti-patterns to avoid
```markdown
## Do NOT
- [Anti-pattern]: [why it's wrong] [what to do instead]
```

## Staleness audit workflow

Read the current context file and check each rule against the codebase:

1. **Still accurate?** Does the codebase actually follow this rule?
2. **Still relevant?** Is the pattern/tool/convention still in use?
3. **Missing?** What patterns are agents clearly using that aren't documented?
4. **Contradictory?** Do any rules conflict with each other or with ADRs?

Flag each issue:
- `[STALE]` — rule no longer reflects reality
- `[MISSING]` — pattern in use but not documented
- `[CONFLICT]` — contradicts another rule or ADR

## Context file structure

```markdown
# [Project] Agent Context

## Purpose
[One sentence: what this project does]

## Architecture
- [Key architectural fact agents need]
- [Service boundaries]
- [Data flow summary]

## Conventions
- [Naming convention]
- [File structure rule]
- [Pattern to follow]

## Patterns
- [Pattern]: see [file]

## Gotchas
- [Non-obvious fact that causes mistakes]

## Do NOT
- [Anti-pattern to avoid]

## Decisions
- [Decision]: [rationale] — ADR-NNN
```

## Gotchas
- Keep the file under 200 lines — extract procedures to skills
- Don't restate what's obvious from the codebase — only include what agents can't infer
- "See [file] for reference implementation" is more reliable than describing the pattern in prose
- Update after every session that establishes a new pattern — don't batch updates
