---
name: escalation-handler
description: >
  Stop work and escalate to the human owner when a stop condition is met.
  Use this skill when the task is ambiguous and two interpretations would
  produce different code, when a required file or dependency doesn't exist,
  when the task requires touching files outside the defined scope, when a
  test is failing after 2 attempts, or when the token/time budget is exceeded.
---

# Escalation Handler

Stop cleanly and give the human owner exactly what they need to unblock the task.

## When to escalate (stop conditions)
- Spec is ambiguous and two reasonable interpretations would produce different code
- Required file, API, or dependency does not exist and cannot be inferred
- Task requires touching files outside the defined scope
- A test is failing after 2 investigation attempts with no clear fix path
- Task requires a security-sensitive change (auth, payments, secrets, permissions)
- Task requires a new external dependency not in the spec
- Confidence in the correct approach is below 70%
- Token or time budget exceeded
- 3 consecutive failures with no progress

## Escalation message format

```markdown
## Escalation: [Task title]

**Stop condition met:** [Which condition from the list above]
**Progress so far:** [What was completed before stopping]
**Blocker:** [Specific description of what is blocking progress]

### The decision needed from you

[One clear question or choice — not a list of everything uncertain]

**Option A:** [Description] → [What the agent will do if you choose this]
**Option B:** [Description] → [What the agent will do if you choose this]

### Context
[Any relevant information that helps the human make the decision]
[Include file paths, error messages, or spec references as needed]

### What I did NOT change
[List any files touched so far — so the human knows the current state]

### To resume
Reply with your decision and I will continue from where I stopped.
```

## Rules
- One clear question per escalation — don't dump all uncertainty at once
- Always state what was completed before stopping — don't make the human re-read the whole task
- Always list files already modified — the human needs to know the current state
- Never make a guess on a stop condition — escalate instead
- Never retry a security-sensitive action without explicit human confirmation

## Gotchas
- "I'm not sure which approach is better" is not an escalation trigger — make a reasonable choice and note it in the PR
- Escalate on ambiguity that would produce *different code*, not on stylistic choices
- If the budget is exceeded, stop immediately — do not continue hoping to finish before anyone notices
- A clean escalation is better than a completed task that solved the wrong problem
