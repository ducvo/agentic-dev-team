---
name: create-agent
description: >
  Design and configure a new AI agent — including its steering file, skills,
  and overall configuration. Use this skill when the user wants to create an
  agent, set up an AI assistant for a specific purpose, define what an agent
  should do, decide what goes in a steering file vs a skill, or figure out
  whether to build a generic or project-specific skill. Also use when the user
  asks how to configure, structure, or scope an agent's instructions.
---

# Create Agent

Design and configure a new AI agent: its steering file, skills, and the
balance between them.

## When to use this skill

- Creating a new agent from scratch
- Deciding what goes in a steering file vs a skill
- Deciding whether a skill should be generic or project-specific
- Structuring an agent's configuration for a new project or team

## Workflow

### Step 1: Clarify the agent's purpose

Before writing anything, establish:

1. **What does this agent do?** One sentence: "This agent helps [who] do [what]."
2. **What context does it always need?** (project conventions, tooling, constraints)
3. **What tasks does it perform?** List the distinct workflows it will execute.
4. **Who uses it?** Just you, your project team, or anyone?

The answers drive every configuration decision that follows.

### Step 2: Decide what goes in the steering file vs skills

Apply the rules in [references/STEERING-VS-SKILLS.md](references/STEERING-VS-SKILLS.md).

**Quick rule:**
- Steering file = standing facts and universal rules (always needed, short)
- Skills = step-by-step procedures and task-specific playbooks (on-demand)

**Token budget:**
- Steering file: keep under 200 lines / ~4,000 tokens
- Each skill body: keep under 500 lines / ~5,000 tokens

If a section of the steering file reads like a numbered procedure, extract it to a skill.

### Step 3: Decide generic vs project-specific for each skill

Apply the criteria in [references/GENERIC-VS-SPECIFIC.md](references/GENERIC-VS-SPECIFIC.md).

**Quick test:** "Would this skill work unchanged in a different repo?"
- Yes → generic skill, install at `~/.kiro/skills/`
- No → project-specific skill, install at `.kiro/skills/` in the repo

### Step 4: Write the steering file

Structure:

```markdown
# [Agent Name]

## Purpose
One sentence: what this agent does and for whom.

## Project context
- Build command: `npm run build`
- Test command: `npm test`
- Key conventions: [e.g., "use conventional commits", "snake_case for DB columns"]
- Non-obvious constraints: [things the agent can't infer from the codebase]

## Universal rules
- [Rule that applies to every task, every session]
- [Another standing rule]
```

Keep it to facts and rules only. No procedures. No step-by-step instructions.

### Step 5: Write each skill

Use the `create-agent-skill` skill to author each SKILL.md.

For each skill:
- [ ] Name matches directory (lowercase, hyphens, 1-64 chars)
- [ ] Description triggers correctly — test with realistic prompts
- [ ] Body is procedures, not declarations
- [ ] Gotchas section captures non-obvious corrections
- [ ] Referenced files exist at the paths stated
- [ ] Body under 500 lines

### Step 6: Validate the full configuration

- [ ] Steering file under 200 lines
- [ ] No procedures in the steering file (extract to skills)
- [ ] Each skill has a clear, distinct trigger condition
- [ ] No overlap between skills (each skill owns one workflow)
- [ ] Generic skills installed at user scope; project skills at project scope
- [ ] Test each skill with a realistic prompt

## Gotchas

- **Don't put procedures in the steering file.** The most common mistake. If it has numbered steps, it belongs in a skill.
- **Don't make the steering file a knowledge dump.** Only include what the agent cannot infer from the codebase itself. Redundant context wastes tokens and degrades performance.
- **Generic skills retrieved from a large pool barely outperform no skills** (arxiv, 2026). A well-scoped project-specific skill consistently outperforms a generic one. When in doubt, scope narrower.
- **The description field is the single most important factor** for whether a skill triggers correctly. Spend time on it. Test it.
- **Skills with executable scripts are 2× more likely to contain vulnerabilities** than instruction-only skills. Review any `scripts/` directory carefully.
- **Skill names must exactly match their directory name.** `name: my-skill` requires directory `my-skill/`.
