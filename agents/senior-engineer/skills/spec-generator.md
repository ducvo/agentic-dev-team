---
name: spec-generator
description: >
  Convert a ticket, user story, or verbal description into a structured agent
  spec. Use this skill when preparing a task for an AI coding agent, writing
  a prompt for Cursor/Claude Code/Kiro, or turning a Jira ticket into
  something an agent can execute reliably. Also use when a previous agent run
  produced wrong output and the spec needs to be fixed.
---

# Spec Generator

Convert intent into a structured spec that produces reliable agent output.

## Workflow

### Step 1: Extract the essentials
From the input (ticket, description, verbal), identify:
- What should exist after this is done that doesn't exist now?
- What files are likely involved?
- What patterns already exist in the codebase to follow?
- What must NOT be changed?

### Step 2: Estimate blast radius
Count likely files affected:
- **Small** (1–3 files): safe for parallel agents
- **Medium** (4–15 files): single agent with check-ins
- **Large** (15+): human-first, agent assist only

### Step 3: Write the spec

```markdown
## Task: [Short title]

### Goal
[One sentence: what should exist after this completes]

### Scope
**In scope:**
- [file or module 1]
- [file or module 2]

**Out of scope (do NOT touch):**
- [explicitly list anything that might seem related but isn't]

### Context
- Follow the pattern in: [existing file that shows the right approach]
- Relevant docs: [link or filename]

### Requirements
1. [Specific, testable requirement]
2. [Another requirement]

### Acceptance criteria
| Scenario | Input | Expected result |
|----------|-------|----------------|
| Happy path | [example] | [expected] |
| Edge case | [example] | [expected] |
| Error case | [example] | [expected] |

### Stop conditions
Stop and ask if:
- [ ] The task requires touching files outside the defined scope
- [ ] A required dependency doesn't exist
- [ ] Two reasonable interpretations of a requirement would produce different code
- [ ] Any test is failing after 2 fix attempts
```

## Gotchas
- The out-of-scope list is as important as the goal — agents add features that seem logical
- "Follow the pattern in X" is more effective than describing the pattern — agents read code better than prose
- Acceptance criteria must be concrete input/output pairs — "handle errors gracefully" is not a criterion
- If a previous agent run went wrong, the fix is almost always in the spec, not the prompt
- Don't include implementation hints — let the agent choose the approach within the constraints
