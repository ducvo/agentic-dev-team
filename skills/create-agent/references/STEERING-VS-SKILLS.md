# Steering Files vs Skills: Balance Rules

When to put agent configuration in a steering file vs a skill.

---

## The Core Distinction

| | Steering File | Skill |
|---|---|---|
| **Loading** | Every session, every turn | Only when invoked |
| **Token cost** | Constant baseline | Near-zero until triggered |
| **Content type** | Facts and standing rules | Step-by-step procedures |
| **Scope** | Cross-task, always relevant | Task-specific, sometimes relevant |
| **Examples** | Build commands, naming conventions, universal guardrails | Deploy playbook, PR review checklist, migration procedure |

The architectural principle: **progressive disclosure**. Only load context when it's needed.

---

## What Goes in the Steering File

Include only what the agent needs in **every session**:

- **Project facts the agent can't infer from code:** build command, test command, deploy target, non-standard tooling
- **Universal guardrails:** "never push directly to main," "always use conventional commits," "never delete production data"
- **Coding conventions:** naming standards, file structure rules, language/framework preferences
- **Non-obvious constraints:** custom flags, unconventional patterns, known gotchas about the codebase

**Format:** Short declarative statements. No numbered steps. No "if X then Y" procedures.

---

## What Goes in a Skill

Move content to a skill when it:

- Has numbered steps or a sequential procedure
- Only applies to one type of task (deploy, review, migrate, etc.)
- Is large enough that loading it every session would waste tokens
- Is a checklist, playbook, or runbook

**Signal to extract:** Any section of your steering file that reads like a procedure belongs in a skill.

---

## Token Budget

| Layer | Content | Target |
|---|---|---|
| Steering file | Always-on facts and rules | < 200 lines / ~4,000 tokens |
| Skill body | On-demand procedures | < 500 lines / ~5,000 tokens |
| Skill reference files | Deep detail loaded on demand | ~700 tokens/file |

**Empirical data:**
- A 1,200-line steering file consumed 42,000 tokens/conversation. Modularising to skills cut that **83%**.
- A React project reduced startup tokens from 7,121 to 2,213 (**69% reduction**) by moving task-specific content to skills.
- ETH Zurich study: detailed config files reduced agent success rates ~3% while increasing inference costs **20–159%**. Only include what the agent cannot infer from the codebase itself.
- Anthropic's recommendation: keep each steering file under 200 lines.

---

## Decision Test

Ask these questions about each piece of content:

1. **Does the agent need this in every session?**
   - Yes → steering file
   - No → skill

2. **Is it a fact/rule or a procedure?**
   - Fact/rule → steering file
   - Procedure → skill

3. **Does it have numbered steps?**
   - Yes → skill

4. **Would loading this every session waste tokens?**
   - Yes → skill

---

## Common Mistakes

**Putting procedures in the steering file:**
```markdown
# Bad — this is a procedure, not a rule
## Deployment
1. Run `npm run build`
2. Run `npm test`
3. Push to staging branch
4. Wait for CI to pass
5. Merge to main
```
→ Extract to a `deploy` skill.

**Making the steering file a knowledge dump:**
```markdown
# Bad — agent can infer this from the codebase
The project uses React with TypeScript. Components are in src/components/.
Pages are in src/pages/. The API is in src/api/.
```
→ Remove it. The agent can read the file tree.

**Correct steering file content:**
```markdown
# Good — facts the agent cannot infer
- Build: `npm run build:prod` (not `npm run build` — that's dev only)
- Never commit directly to main or develop branches
- All DB column names use snake_case; TypeScript interfaces use camelCase
```
