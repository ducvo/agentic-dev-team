---
name: prompt-engineering
description: >
  Write effective prompts and specs that produce reliable, on-scope agent
  output. Use this skill when a previous agent run produced wrong, incomplete,
  or out-of-scope output; when preparing a task for an AI coding agent;
  when writing or improving a system prompt or steering file; or when asked
  how to get better results from an AI tool. Also use when the agent is
  about to start a task and needs to verify the prompt is complete before
  executing.
---

# Prompt Engineering

Write prompts and specs that produce reliable agent output. The most common cause of bad agent output is a bad prompt — not a bad model.

## Core principle

A prompt is a spec. It must define:
1. **Goal** — what should exist after this completes (not how to do it)
2. **Scope** — what to touch and what NOT to touch
3. **Acceptance criteria** — binary pass/fail conditions with concrete examples
4. **Stop conditions** — when to escalate instead of proceeding

If any of these are missing, the agent will fill the gap with its own assumptions. Those assumptions are usually wrong.

---

## Prompt quality checklist

Before executing any task, verify the prompt has:

- [ ] A single, unambiguous goal sentence
- [ ] An explicit out-of-scope list (not just in-scope)
- [ ] Acceptance criteria as concrete input → expected output pairs
- [ ] A reference to an existing pattern to follow ("follow the pattern in X")
- [ ] Stop conditions that cover ambiguity, missing dependencies, and scope violations
- [ ] Security constraints if the task touches auth, data, or external inputs
- [ ] A token/time budget or escalation point

If any item is missing, ask for it before starting. Do not infer missing constraints.

---

## Prompt patterns

### Pattern 1: Goal + scope + criteria (standard task)

```
## Task: [Short title]

### Goal
[One sentence: what should exist after this completes]

### Scope
In scope: [files/modules]
Out of scope (do NOT touch): [explicitly list]

### Follow this pattern
[Existing file that shows the right approach]

### Acceptance criteria
| Scenario | Input | Expected output |
|----------|-------|----------------|
| Happy path | [example] | [expected] |
| Error case | [example] | [expected] |

### Stop conditions
Stop and ask if:
- The task requires touching files outside the defined scope
- A required dependency doesn't exist
- Two reasonable interpretations would produce different code
- A test is failing after 2 fix attempts
```

### Pattern 2: Role + constraint (system prompt / steering file)

```
You are a [role]. Your job is to [specific purpose].

Always:
- [Standing rule 1]
- [Standing rule 2]

Never:
- [Hard constraint 1]
- [Hard constraint 2]

When uncertain: [escalation behaviour]
```

### Pattern 3: Few-shot (format enforcement)

```
Convert the following to [format]. Examples:

Input: [example 1]
Output: [expected 1]

Input: [example 2]
Output: [expected 2]

Now convert:
Input: [actual input]
Output:
```

### Pattern 4: Chain-of-thought (complex reasoning)

```
[Task description]

Think step by step:
1. First, identify [X]
2. Then, consider [Y]
3. Finally, produce [Z]
```

Use CoT when the task involves multi-step reasoning, trade-off analysis, or when the model has previously jumped to wrong conclusions on similar tasks.

---

## Common failure modes and fixes

| Failure | Root cause | Fix |
|---|---|---|
| Agent adds unrequested features | No out-of-scope list | Add explicit "do NOT touch" list |
| Agent uses wrong pattern | No reference implementation | Add "follow the pattern in [file]" |
| Agent loops or drifts | Ambiguous acceptance criteria | Rewrite criteria as concrete input/output pairs |
| Agent touches wrong files | Scope too vague | List specific files in scope |
| Agent produces tautological tests | No test-first instruction | Add "write test expectations from requirements before looking at implementation" |
| Agent hallucinates a dependency | No dependency constraint | Add "only use dependencies already in package.json" |
| Agent ignores security constraints | Security not in prompt | Add explicit security section |

---

## Prompt improvement workflow

When a previous run produced wrong output:

1. **Identify the gap** — what did the agent do that the prompt didn't prevent?
2. **Add the missing constraint** — don't just re-run; update the prompt
3. **Verify the fix is in the prompt** — the next agent run should not be able to make the same mistake
4. **Update the spec** — if this was a spec-driven task, update the spec so future runs benefit

Do not fix bad output by prompting harder. Fix it by fixing the spec.

---

## Steering file vs. skill vs. prompt

| Type | What goes here | When loaded |
|---|---|---|
| Steering file (AGENT.md) | Standing facts, universal rules, role context | Always |
| Skill (SKILL.md) | Step-by-step procedures for specific tasks | On demand, when task matches |
| Task prompt | Goal, scope, criteria for one specific execution | Per execution |

Don't put procedures in the steering file. Don't put standing rules in task prompts. Don't repeat what the agent can infer from the codebase.

## Gotchas

- "Be more careful" is not a constraint — it's noise. Every constraint must be specific and testable.
- The out-of-scope list is as important as the goal. Agents add features that seem logical.
- "Follow the pattern in X" outperforms describing the pattern in prose — agents read code better than descriptions.
- Acceptance criteria must be binary pass/fail with concrete examples. "Handle errors gracefully" is not a criterion.
- A prompt that worked once may not work after a model upgrade — treat prompts as versioned artifacts.
