---
name: context-reader
description: >
  Read and understand the codebase context before implementing a task. Use
  this skill at the start of any task to find existing patterns to follow,
  identify relevant files, understand conventions, and avoid reinventing
  what already exists. Also use when unsure which pattern to follow or when
  the codebase is unfamiliar.
---

# Context Reader

Orient to the codebase before implementing — find the patterns to follow, not the patterns to invent.

## Why this matters
Agents that skip context reading reinvent utilities that already exist, apply the wrong naming convention, violate layer boundaries, and introduce inconsistent patterns. Reading context first takes 2 minutes and prevents hours of rework.

## Step 1: Read the project context files
In order:
1. `AGENTS.md` or `CLAUDE.md` at the repo root — conventions, patterns, DO NOT list
2. `docs/architecture.md` — layer structure, module boundaries
3. Any ADRs referenced in AGENTS.md relevant to this task

Extract:
- Naming conventions that apply to this task
- Layer rules (what can call what)
- Patterns explicitly listed as "follow this" or "do not do this"
- Shared utilities relevant to this task

## Step 2: Find the reference implementation
For the type of thing you're building, find an existing example in the codebase:
- Building an API endpoint? Find an existing endpoint in the same service
- Building a data transform? Find an existing transform of the same type
- Writing a test? Find an existing test for a similar function

Read it carefully:
- What naming pattern does it use?
- How does it handle errors?
- What does it import from where?
- How is it tested?

Note the reference file path — you'll use it in the spec: "follow the pattern in [file]"

## Step 3: Check for existing utilities
Before implementing any helper logic, search for existing utilities:
- Date/time formatting
- Error handling and custom error classes
- HTTP client / API call wrappers
- Validation helpers
- Logging patterns

If it exists, use it. Do not reimplement.

## Step 4: Summarise what you found

```markdown
## Context Summary: [Task title]

**Conventions that apply:**
- [Naming convention]
- [Layer rule]

**Reference implementation to follow:**
- [file path]: [what it shows]

**Existing utilities to use (do not reimplement):**
- [utility]: [file path]

**Patterns to avoid (from DO NOT list):**
- [anti-pattern]
```

Use this summary to inform the implementation and include the reference file in the spec context section.

## Gotchas
- If AGENTS.md says "follow the pattern in X" — read X before writing anything
- If you can't find a reference implementation, ask before inventing a new pattern
- Shared utilities are the most commonly reinvented thing — always check before implementing date formatting, error handling, or HTTP calls
- Layer violations are the most common architectural mistake — check what can call what before writing any cross-layer code
