---
name: requirement-triage
description: >
  Triage an incoming requirement before writing a spec. Use this skill when
  a new requirement arrives and you need to decide whether it is already
  doable in the existing system, should be rejected as inconsistent with the
  architecture roadmap, or should be altered to fit the existing system.
  Also use when a stakeholder request conflicts with known ADRs or existing
  capabilities.
---

# Requirement Triage

Decide what to do with a requirement before writing a single line of spec.

## The three decisions

| Decision | Condition | Action |
|---|---|---|
| **Already doable** | Capability exists or can be configured | Redirect — no new spec needed |
| **Reject** | Conflicts with ADR or architecture roadmap | Escalate to Architect — do not alter silently |
| **Alter** | Right goal, wrong approach for the system | Reshape to fit, validate with stakeholder |

---

## Step 1: Check if it already exists

Before drafting anything, ask:
- Does the system already have this capability, even if unexposed?
- Is this a configuration or permission change rather than new development?
- Is there an existing ticket or feature that overlaps?

**How to check:**
- Ask the Senior Engineer: "does anything like this already exist in the codebase?"
- Review the current API surface and data model
- Search the backlog for duplicate or overlapping items

**If it exists:** The ticket is a duplicate or a configuration task. Close it, redirect to docs/training, or write a narrow "expose/configure X" spec — not "build X."

---

## Step 2: Check for architectural conflicts

Compare the requirement against:
- Accepted ADRs (check `docs/adr/`)
- The architecture roadmap
- Established integration patterns and ownership boundaries

**Conflict signals:**
- Requirement implies a new dependency the architecture has explicitly avoided
- Requirement crosses a service ownership boundary
- Requirement implies a data model change that contradicts an ADR
- Requirement introduces a pattern the team has decided not to use

**If a conflict exists:**
- Do NOT silently reshape the requirement to avoid the conflict
- Escalate to the Architect with a specific conflict statement:
  > "This requirement implies [X], but [ADR-NNN / roadmap direction] says [Y]. Do we reject, update the ADR, or carve out an exception?"
- Wait for the Architect's decision before proceeding

---

## Step 3: Alter to fit (if no conflict, but wrong approach)

This is the most common case. The business goal is valid but the stakeholder's proposed approach doesn't fit the existing system.

1. Identify the specific mismatch (wrong data model, wrong integration pattern, wrong layer)
2. Propose the alteration that achieves the same business goal within existing constraints
3. Validate with the stakeholder: "I'd like to achieve this by doing X instead of Y — does that still meet your need?"
4. Document the alteration in the spec under a "Constraints" or "Not Included" section with the reason

**Alter when:** The business goal can be fully achieved within existing architecture.
**Escalate when:** Achieving the goal requires bending the architecture — that's a roadmap decision, not a spec decision.

---

## Output: triage decision record

Before writing the spec, record the triage outcome:

```markdown
## Requirement Triage: [Requirement title]

**Decision:** Already doable | Reject (escalated) | Alter | Proceed as-is

**Rationale:**
[Why this decision was made]

**Action:**
[What happens next — redirect / escalate to Architect / alter as follows / proceed to spec]

**Stakeholder communication needed:**
[Yes/No — what to communicate and to whom]
```

## Gotchas
- Never silently reshape a requirement to avoid an architectural conflict — surface it explicitly
- "The stakeholder wants it" is not a reason to override an ADR — escalate the tension
- Alteration must preserve the business goal — if the altered version no longer meets the need, it's a rejection, not an alteration
- Discovery of existing capability is the highest-value triage outcome — it saves the entire cost of a sprint
