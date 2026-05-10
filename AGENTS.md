# AGENTS.md — agentic-dev-team

## Purpose
This repo is a knowledge base and configuration library for operating a software delivery team that uses agentic AI development tools. It contains research reports, agent steering files, and reusable skills — not application code.

## Repo structure
```
agents/<role>/AGENT.md          # Steering file for that role's AI assistant
agents/<role>/skills/<skill>.md # On-demand skill procedures for that role
skills/<skill>/SKILL.md         # Reusable skills installable into any AI tool
team-structure.md               # Research: how traditional roles evolve with AI tools
multi-repo-documentation.md     # Research: documenting multi-repo systems for agents
```

## File conventions
- `AGENT.md` — steering file format: facts and standing rules only, no procedures, under 200 lines
- `skills/*.md` — skill format: SKILL.md frontmatter + step-by-step procedures, under 500 lines
- Research reports — plain markdown, cited sources, evidence graded as Strong/Moderate/Weak/Emerging
- All markdown files use `##` for top-level sections (the `#` title is the only H1)

## Commit rules
- Commit in logical chunks — one concern per commit (never mix agent additions, research docs, and skill updates)
- Conventional commits: `type(scope): short description`
  - `feat(agents):` — new or updated agent configuration
  - `feat(skills):` — new or updated skill
  - `docs:` — research reports, README, documentation
  - `fix(agents):` — correction to an existing agent
- Stage specific files, never `git add .`
- Commit message body: explain what changed and why when non-obvious

## Adding a new agent
1. Create `agents/<role>/AGENT.md` — purpose, role context, universal rules, commit rules (if applicable)
2. Create `agents/<role>/skills/<skill>.md` for each on-demand procedure
3. Update `README.md` agents table
4. Commit AGENT.md and skills/ separately: `feat(agents): add <role> agent` then skills in one commit

## Adding a new skill (to skills/)
1. Create `skills/<skill-name>/SKILL.md` with valid frontmatter (name, description)
2. Add supporting files to `references/` or `assets/` as needed
3. Update `README.md` skills table
4. Commit: `feat(skills): add <skill-name> skill`

## Adding a research document
1. Name the file descriptively in kebab-case: `<topic>.md`
2. Include: research date, source count, evidence grading
3. Commit: `docs: add <topic> research report`

## DO NOT
- Put procedures in AGENT.md files (procedures belong in skills)
- Put standing rules in skill files (rules belong in AGENT.md)
- Add application code, build scripts, or package files — this is a docs/config repo
- Modify skills/ files copied from ~/.kiro/skills/ without also updating the source

## Key references
- README.md — full usage guide and agent/skill index
- multi-repo-documentation.md — how to document systems for agents
- team-structure.md — how traditional roles evolve with agentic AI tools
