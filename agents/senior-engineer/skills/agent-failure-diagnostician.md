---
name: agent-failure-diagnostician
description: >
  Diagnose why an AI agent run failed, looped, went off-scope, or produced
  wrong output. Use this skill when an agent produced unexpected results,
  when a task needs to be re-run after a failure, when an agent is looping
  or not making progress, or when you need to understand what went wrong
  before trying again.
---

# Agent Failure Diagnostician

Diagnose agent failures systematically instead of prompting and praying.

## Failure categories

### 1. Spec gap
**Symptoms**: Agent implemented something plausible but wrong; added unrequested features; ignored a constraint
**Diagnosis**: The spec was ambiguous, incomplete, or missing a scope boundary
**Fix**: Add the missing constraint to the spec; add an explicit "Not Included" item; make acceptance criteria more concrete

### 2. Context gap
**Symptoms**: Agent reinvented a pattern that already exists; used wrong naming convention; hallucinated an API
**Diagnosis**: The agent didn't know about an existing pattern, convention, or API
**Fix**: Add the missing fact to AGENTS.md; add "follow the pattern in [file]" to the spec; point to the existing implementation

### 3. Blast radius too large
**Symptoms**: Agent made changes across many files; introduced inconsistencies; merge conflicts
**Diagnosis**: The task scope was too large for a single agent session
**Fix**: Break into smaller tasks; define explicit file scope boundaries; use single agent with check-ins instead of parallel

### 4. Wrong tool / model
**Symptoms**: Agent couldn't complete terminal operations; IDE-specific features failed; long autonomous run timed out
**Diagnosis**: Wrong tool selected for the task type
**Fix**: Match tool to task — terminal work → Claude Code; IDE work → Cursor; long autonomous → Devin/OpenHands

### 5. Model limitation
**Symptoms**: Agent consistently fails at a specific type of reasoning; produces plausible but logically wrong code
**Diagnosis**: The task exceeds the model's reliable capability for this problem type
**Fix**: Break into smaller reasoning steps; provide worked examples; consider human-first with agent assist

### 6. Loop / no progress
**Symptoms**: Agent keeps trying the same approach; token budget consumed without completion
**Diagnosis**: Agent is stuck on a failing test or ambiguous requirement
**Fix**: Abort; identify the specific blocker; either fix the test, clarify the requirement, or implement that part manually

## Diagnostic workflow

```
1. What was the expected output?
2. What did the agent actually produce?
3. At what step did it diverge?
4. Which category does this match? (spec gap / context gap / blast radius / wrong tool / model limit / loop)
5. What is the minimal fix?
6. Update the spec or context file before re-running
```

## Output format

```
## Agent Failure Diagnosis

**Task:** [original task]
**Failure type:** [category]

### What went wrong
[Specific description of the divergence]

### Root cause
[Spec gap / context gap / etc. — with specific missing element]

### Fix
[Exact change to spec or context file]

### Updated spec section
[The corrected spec text]
```

## Gotchas
- The fix is almost always in the spec or context file, not the prompt phrasing
- Don't re-run with the same spec hoping for a different result — fix the root cause first
- If the same failure recurs across multiple tasks, it's a context file issue, not a spec issue
- Abort early when an agent is heading wrong — the longer it runs off-course, the more cleanup required
