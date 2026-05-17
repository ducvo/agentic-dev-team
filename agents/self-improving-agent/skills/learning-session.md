---
name: learning-session
description: >
  Run a learning session to extract lessons from recent chat history and apply
  them to the agent's own steering files and skills. Use this skill after any
  session where the human corrected the agent, terminated an execution, redirected
  an approach, or said "don't do X" / "always do Y". Also use at the end of a
  sprint or after a significant project milestone to consolidate accumulated
  feedback into durable rules.
---

# Learning Session

Extract lessons from session history and apply them to the agent's steering files and skills.

## When to use this skill

- After a session with corrections, terminations, or redirections
- At the end of a sprint or milestone to consolidate feedback
- When the human explicitly asks the agent to "learn from this session"
- When the same mistake has occurred more than once

## Workflow

### Step 1: Collect the session history

Ask the human to provide (or point to) the session history to review. This may be:
- A chat transcript or exported conversation
- A summary of what went wrong and what corrections were made
- A list of "don't do X" / "always do Y" statements from the session

If no history is provided, ask: "Which session should I learn from? Please share the transcript or describe the key corrections."

### Step 2: Extract candidate lessons

Read the history and identify every feedback signal:

For each signal, record:
```
Signal: [what the human said or did]
Type: [tool-choice | architecture | escalation | process | factual]
Draft lesson: [specific, actionable rule in "prefer X over Y when Z" form]
Confidence: [high = explicit correction | low = inferred from redirect]
```

Discard:
- Signals with confidence = low and no corroborating signal
- Signals that are ambiguous or could be interpreted multiple ways
- Clarifying questions (not corrections)

### Step 3: Check for duplicates and conflicts

For each draft lesson:

1. Read the relevant steering file or skill
2. Check: does this lesson already exist (same rule, different wording)?
   - If yes: discard the draft, the rule is already captured
3. Check: does this lesson contradict an existing rule?
   - If yes: the new lesson wins — update the existing rule rather than adding a new one
   - Note the conflict explicitly in the commit message

### Step 4: Apply lessons

For each validated lesson, determine where it belongs:

| Lesson type | Destination |
|---|---|
| Always/never rule, applies every session | Steering file (`AGENT.md`) |
| Step-by-step correction to a workflow | Relevant skill (`skills/<skill>.md`) |
| New workflow not covered by any skill | New skill |

Apply the change:
- Add to the appropriate `## ` section (Universal rules, Gotchas, etc.)
- Use the exact form: "prefer X over Y when Z" or "always X" or "never X"
- Keep each rule to one line

### Step 5: Validate

After all changes:

- [ ] Every new rule is specific and actionable (no vague rules like "be more careful")
- [ ] No duplicate rules exist in the updated file
- [ ] No contradicting rules exist in the updated file
- [ ] Steering file is still under 200 lines
- [ ] Each updated skill is still under 500 lines
- [ ] Read the updated file top to bottom — does it read coherently?

### Step 6: Report

Summarise what was learned:

```
## Learning session summary

Sessions reviewed: [N]
Signals found: [N]
Lessons applied: [N]
Files updated: [list]

### Lessons applied
- [File]: [lesson added or rule updated]
- ...

### Signals discarded
- [Signal]: [reason discarded]
```

## Lesson quality checklist

A good lesson:
- [ ] Names a specific tool, pattern, or action (not a vague category)
- [ ] States the condition: "when X" or "for Y tasks"
- [ ] States the preferred behaviour: "use A" or "avoid B"
- [ ] Could be followed by a different agent with no additional context

A bad lesson (discard or rewrite):
- "Be more careful" — not actionable
- "Think before acting" — not actionable
- "Handle errors better" — not specific enough
- "The human prefers a different style" — too vague

## Gotchas

- **Don't infer lessons from silence.** The human accepting output without comment is not a positive signal — it's neutral. Only corrections count.
- **One correction ≠ one rule.** If the human corrected a one-off mistake caused by missing context (not a recurring pattern), document it as a gotcha, not a universal rule.
- **Lesson inflation degrades performance.** Every rule added to a steering file competes for attention. Add only lessons that will prevent a recurring mistake.
- **Conflicts must be resolved, not stacked.** If a new lesson contradicts an old rule, update the old rule. Two contradicting rules cause unpredictable behaviour.
- **Skills are for procedures, steering files are for rules.** A lesson that says "when doing X, follow these steps" belongs in a skill. A lesson that says "always/never do Y" belongs in the steering file.
