---
name: agent-job-spec
description: >
  Translate a feature request, ticket, or user story into a structured agent
  job spec. Use this skill when the user wants to prepare a task for an AI
  coding agent, write a spec for autonomous execution, define what an agent
  should and should not do, or set acceptance criteria and escalation triggers
  for an agent job. Also use before any significant agent invocation.
---

# Agent Job Spec

Translate intent into a structured spec that produces reliable agent output.

## Why this matters
Spec quality is the #1 determinant of agent output quality. Vague prompts produce vague code. This spec format enforces the INTENT → SPEC → PLAN pipeline before any agent executes.

## Workflow

### Step 0: Check for an existing BA spec
For any user-facing feature, check whether the BA has produced a spec:
- If a BA spec exists: derive the technical job spec from it — use the same objective, acceptance criteria, and scope boundary
- If no BA spec exists for a user-facing feature: request one before proceeding
- If this is a purely technical task (infra, refactor, dependency update): proceed directly to Step 1

### Step 1: Classify the task
Before writing the spec, assess:
- **Blast radius**: how many files will this touch? (Small <4 / Medium 4–15 / Large 15+)
- **Risk level**: Low (UI, config, docs) / Medium (features, APIs) / High (auth, payments, security, cross-service)
- **Novelty**: Is this a known pattern or genuinely new territory?

Large blast radius or High risk → escalate to human-first with agent assist only.

### Step 2: Write the spec

```markdown
## Agent Job Spec: [Short title]

**Risk level:** Low | Medium | High
**Blast radius:** Small | Medium | Large
**Assigned to:** [agent name or type]
**Human owner:** [name — accountable for this job's output]

### Objective
[One sentence: what should exist after this job completes that doesn't exist now]

### Not included (scope boundary)
- [Explicitly list what the agent must NOT build or change]
- [If it seems logical but isn't in scope, list it here]

### Context
- Key files to read: [list]
- Patterns to follow: [e.g., "follow the pattern in src/api/users.ts"]
- Relevant ADRs: [list]

### Requirements
- [Specific behavior requirement 1]
- [Specific behavior requirement 2]

### Acceptance criteria
| Input | Expected output |
|-------|----------------|
| [concrete input] | [concrete expected output] |
| [edge case] | [expected handling] |

### Constraints
- Always: [what the agent must always do]
- Ask first: [what requires human confirmation before proceeding]
- Never: [hard prohibitions]

### Stop conditions
- [Condition that should cause the agent to stop and escalate]
- Ambiguous requirement with two valid interpretations
- Required file does not exist
- Task requires touching files outside defined scope

### Budget
- Token limit: [e.g., 50k tokens]
- Time limit: [e.g., 15 minutes]
```

## Gotchas
- The "Not Included" section is as important as the objective — agents add features that seem logical
- Acceptance criteria must be concrete input/output pairs, not vague quality attributes ("handle errors gracefully" is not a criterion)
- "Ask First" items are the most commonly omitted and most costly when missing
- Don't include implementation hints — let the agent choose the approach within the constraints
- Large blast radius + parallel agents = merge conflicts and hard-to-reset failures
