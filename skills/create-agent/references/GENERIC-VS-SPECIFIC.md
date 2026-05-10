# Generic vs Project-Specific Skills: Decision Criteria

When to build a reusable generic skill vs a project-specific one.

---

## The Core Tradeoff

| | Generic Skill | Project-Specific Skill |
|---|---|---|
| **Portability** | Works across any project | Tied to one codebase |
| **Effectiveness** | Lower — may not match task well | Higher — tailored to exact context |
| **Authoring cost** | Once, amortised across many uses | Per-project |
| **Trigger reliability** | Harder — needs broad description | Easier — narrow, precise description |
| **Risk of misleading agent** | Higher if poorly matched | Lower — purpose-built |
| **Install location** | `~/.kiro/skills/` (user scope) | `.kiro/skills/` in repo (project scope) |

**Key empirical finding (arxiv, UCSB + MIT, 2026, n=34k skills):**

| Condition | Agent pass rate |
|---|---|
| Task-specific curated skills | 55.4% |
| Generic retrieved skills | 38.4% |
| No skills | 35.4% |

Generic skills retrieved from a large pool barely outperform no skills. Weaker models dropped *below* their no-skill baseline when given irrelevant generic skills — noise actively misled them.

---

## Decision Criteria

### Make it generic when:

- The workflow is **domain-agnostic**: "write a conventional commit," "do a security review," "write a Playwright test"
- It will be used **across multiple projects** or by multiple team members
- The procedure **doesn't reference project-specific paths, APIs, or conventions**
- You want to **share it** via a skills registry or Git submodule
- Examples: `deep-research`, `code-review`, `pr-review`, `create-agent-skill`, `write-adr`

### Make it project-specific when:

- The workflow **references project-specific paths, env vars, or APIs**: "deploy to our Vercel project," "run our migration playbook"
- It **embeds project conventions** that differ from standard practice: custom token mappings, non-standard naming, bespoke tooling
- The procedure **only makes sense in this codebase**
- Examples: `deploy-to-staging` (with your env vars), `db-migration` (with your schema validation), `tailwind-migration` (with your token mappings)

---

## The Portability Test

Ask: **"Would this skill work unchanged in a different repo?"**

- **Yes** → generic, install at `~/.kiro/skills/`
- **No** → project-specific, install at `.kiro/skills/` in the repo

If the skill references any of these, it's project-specific:
- Specific file paths (`src/api/`, `scripts/migrate.py`)
- Environment variables (`VERCEL_PROJECT_ID`, `DATABASE_URL`)
- Project-specific commands (`npm run build:prod` vs `npm run build`)
- Internal API endpoints or service names
- Custom naming conventions unique to this project

---

## Scope Levels

| Scope | Install location | Who benefits |
|---|---|---|
| Personal | `~/.kiro/skills/` | You, across all your projects |
| Project | `.kiro/skills/` in repo | Everyone on this project |
| Team/Org | Shared Git repo, submoduled in | Everyone in the org |

---

## The Refinement Bridge

The arxiv study found that **query-specific refinement** — adapting a generic skill to the specific task at hand — recovers most of the performance gap (38.4% → 48.2% for Claude).

Practical implication: maintain a library of generic skills, but when a generic skill consistently underperforms for a specific project, **fork it and add project-specific context** rather than maintaining two separate full copies. Keep the generic version as the base.

---

## Decision Flowchart

```
Does the skill reference project-specific paths, APIs, or conventions?
├── Yes → Project-specific skill (.kiro/skills/ in repo)
└── No → Will it be used across multiple projects?
         ├── Yes → Generic skill (~/.kiro/skills/)
         └── No → Still generic, but consider whether it's worth authoring
                  (project-specific skills are lower effort to write)
```

---

## Common Mistakes

**Making a generic skill too broad:**
A skill named `coding` that covers "write code, review code, test code, deploy code" will never trigger reliably and will mislead the agent when it does. Scope to one workflow.

**Making a project-specific skill when generic would do:**
If you're writing `write-conventional-commit` with your project's specific prefixes hardcoded, consider whether a generic version with the standard convention would work just as well. Hardcoding reduces portability for minimal gain.

**Forgetting the description is the trigger:**
Generic skills need descriptions broad enough to match diverse phrasings but specific enough not to over-trigger. Project-specific skills can use narrow, precise descriptions. Both need explicit "Do NOT use when" clauses for near-miss cases.
