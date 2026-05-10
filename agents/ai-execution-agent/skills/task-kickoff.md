---
name: task-kickoff
description: >
  Read and confirm understanding of a spec before writing any code. Use this
  skill at the start of every agent task to parse the spec, identify risks,
  confirm scope, and surface ambiguities before they become failed runs. Also
  use when a spec seems unclear or when the task feels larger than expected.
---

# Task Kickoff

Parse the spec and confirm understanding before writing a single line of code. Catching ambiguities here costs minutes; catching them after a failed run costs hours.

## Kickoff checklist

### 1. Parse the spec
Read the entire spec and extract:
- [ ] **Objective** — one sentence: what should exist after this task completes?
- [ ] **Scope** — which files are in scope? Which are explicitly out of scope?
- [ ] **Acceptance criteria** — list every AC; these define done
- [ ] **Stop conditions** — what triggers escalation?
- [ ] **Constraints** — Always / Ask First / Never tiers; security and performance requirements
- [ ] **Budget** — token limit and time limit defined?

### 2. Identify risks before starting
Flag any of the following:
- [ ] Any AC is vague or lacks a concrete expected output → escalate before starting
- [ ] Scope touches files that seem risky (auth, payments, shared utilities) → note for extra care
- [ ] "Ask First" tier is empty → confirm with human owner what requires confirmation
- [ ] No budget defined → ask before starting
- [ ] Spec references a file or pattern that doesn't exist → escalate before starting

### 3. Confirm understanding
Produce a brief kickoff summary before writing code:

```markdown
## Task Kickoff: [Task title]

**Objective:** [One sentence]

**I will change:**
- [file or module]: [what I'll do]

**I will NOT touch:**
- [explicitly listed out-of-scope items]

**Done when:**
- AC1: [description]
- AC2: [description]

**I will escalate if:**
- [stop condition 1]
- [stop condition 2]

**Risks / questions:**
- [Any ambiguity or risk identified — or "None"]
```

If there are unresolved risks or questions, send this summary to the human owner and wait for confirmation before proceeding.

If everything is clear, proceed to implementation.

## Gotchas
- A spec that seems clear often has one ambiguous AC — read every criterion carefully before starting
- "I'll figure it out as I go" is how tasks turn into failed runs — surface uncertainty now
- If the scope feels larger than expected, re-estimate blast radius before starting
- The kickoff summary is also useful for the PR description — save it
