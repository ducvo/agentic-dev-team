---
name: create-agent-skill
description: >
  Create new agent skills that conform to the agentskills.io open standard
  (SKILL.md format). Use this skill when the user wants to create, scaffold,
  or author a new agent skill, write a SKILL.md file, package instructions
  for other agents, or build reusable agent capabilities — even if they
  don't explicitly say "agent skill" or "SKILL.md."
---

# Create Agent Skill

Create agent skills conforming to the [agentskills.io](https://agentskills.io) open standard.

## When to use this skill

- Create a new agent skill from scratch
- Convert existing knowledge, workflows, or runbooks into a skill
- Scaffold the directory structure for a skill
- Write or improve a SKILL.md file

## Workflow

### Step 1: Understand what the skill should do

Before writing anything, clarify with the user:
1. **What task does the skill address?** Get a concrete description of the workflow or capability.
2. **What does the agent lack?** Focus on knowledge the agent wouldn't have without the skill — project conventions, domain procedures, non-obvious edge cases, specific tools/APIs.
3. **What source material exists?** Ask for runbooks, docs, API specs, code examples, or past transcripts that capture the expertise.

Do not generate a skill from general knowledge alone. Ground it in real expertise or source material.

### Step 2: Scaffold the directory

```
skill-name/
├── SKILL.md          # Required: metadata + instructions
├── scripts/          # Optional: executable code
├── references/       # Optional: detailed documentation
└── assets/           # Optional: templates, resources
```

The directory name must match the `name` field in the frontmatter.

### Step 3: Write the SKILL.md

Start from the template at [assets/SKILL-TEMPLATE.md](assets/SKILL-TEMPLATE.md).

**Frontmatter (required):**

```yaml
---
name: skill-name
description: >
  What this skill does and when to use it. Max 1024 chars.
  Use imperative phrasing: "Use this skill when..."
---
```

**Name rules:** lowercase letters, numbers, hyphens only. 1-64 chars. No leading/trailing/consecutive hyphens. Must match parent directory name.

**Description rules:** max 1024 chars. Use imperative phrasing ("Use this skill when..."). Focus on user intent, not implementation. List specific trigger contexts including cases where the user doesn't name the domain directly.

See [references/SPECIFICATION.md](references/SPECIFICATION.md) for the complete field reference including optional fields (`license`, `compatibility`, `metadata`, `allowed-tools`).

### Step 4: Write effective instructions

The body content after frontmatter contains the skill instructions in Markdown. Follow these patterns — see [references/BEST-PRACTICES.md](references/BEST-PRACTICES.md) for detailed guidance.

- **Procedures over declarations.** Teach *how to approach* a class of problems, not what to produce for one instance.
- **Defaults over menus.** Pick one default approach; mention alternatives briefly.
- **Gotchas.** List concrete corrections to mistakes the agent will make without being told.
- **Checklists** for multi-step workflows so the agent tracks progress.
- **Validation loops.** Do work → validate → fix → repeat until passing.
- **Templates** for any output needing a specific format.
- **Cut what the agent already knows.** Don't explain what a PDF is or how HTTP works.

### Step 5: Structure for progressive disclosure

Skills manage context in three tiers:

1. **Metadata** (~100 tokens): `name` + `description` load at startup for all skills
2. **Instructions** (<5000 tokens): full SKILL.md body loads on activation
3. **Resources** (as needed): files in `scripts/`, `references/`, `assets/` load on demand

Keep SKILL.md under 500 lines. Move detailed reference material to separate files with specific load conditions:

```markdown
Read [references/api-errors.md](references/api-errors.md) if the API returns a non-200 status code.
```

Avoid generic "see references/ for details."

### Step 6: Validate

- [ ] `name` field is valid (lowercase, hyphens, 1-64 chars, matches directory)
- [ ] `description` is under 1024 characters
- [ ] SKILL.md body is under 500 lines / 5000 tokens
- [ ] All referenced files exist with relative paths from skill root
- [ ] File references are one level deep (no nested chains)
- [ ] Run `skills-ref validate ./skill-name` if available
- [ ] Test with a realistic prompt and review the agent's execution trace

## Gotchas

- The `name` must exactly match the parent directory name. `name: my-skill` requires directory `my-skill/`.
- Descriptions grow during iteration — check length stays under 1024 chars.
- Focused skills (2-3 modules) outperform comprehensive ones. Don't cover every edge case.
- Skills generated from general knowledge without real source material tend to be vague ("handle errors appropriately"). Always ground in specifics.
- The entire SKILL.md body loads into context on activation. Every token competes for attention.
- File references should be one level deep from SKILL.md. No deeply nested chains.
