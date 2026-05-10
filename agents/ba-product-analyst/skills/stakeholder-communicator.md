---
name: stakeholder-communicator
description: >
  Draft stakeholder communications for requirement decisions. Use this skill
  when communicating a requirement rejection, scope alteration, change impact,
  sprint review summary, or delay to a non-technical stakeholder. Also use
  when translating technical constraints into business language or when a
  stakeholder needs to understand why their request was changed or declined.
---

# Stakeholder Communicator

Translate technical decisions into clear business communications.

## Communication types

### 1. Requirement rejection
When a requirement conflicts with the architecture roadmap and the Architect has decided to reject it.

```markdown
Subject: [Requirement title] — not proceeding

Hi [name],

Thank you for raising [requirement]. After reviewing it with the technical team,
we're not able to proceed with this as described.

**Why:** [Plain-language explanation of the architectural conflict — no jargon]

**What this means for you:** [Business impact of not doing it]

**Alternatives we can offer:**
- [Option A: what we can do instead]
- [Option B: if applicable]

If you'd like to discuss further, I'm happy to set up a call.

[BA name]
```

### 2. Scope alteration
When a requirement has been reshaped to fit the existing system.

```markdown
Subject: [Requirement title] — proceeding with adjustment

Hi [name],

We're moving forward with [requirement] with one adjustment to fit our
existing system.

**What you asked for:** [Original request in plain language]

**What we're building:** [Altered version in plain language]

**Why the change:** [Plain-language explanation — no jargon]

**Does this still meet your need?** [Confirm this achieves the business goal]

Please confirm you're happy with this approach before we begin.

[BA name]
```

### 3. Change impact communication
When a mid-sprint change request has been assessed and has a cost.

```markdown
Subject: Impact of [change request] on current sprint

Hi [name],

You've requested a change to [requirement]. Here's what that means:

**The change:** [What's being added/modified/removed]

**Impact:**
- Additional effort: [estimate]
- Items at risk: [what might not be delivered this sprint]
- Dependencies affected: [list or "none"]

**Options:**
1. Proceed with the change — [item X] moves to next sprint
2. Defer the change — deliver as originally scoped
3. Descope [item Y] to make room for the change

Which would you prefer?

[BA name]
```

### 4. Sprint review summary
A plain-language summary of what was delivered, for non-technical stakeholders.

```markdown
## Sprint [N] Summary — [Date]

### Delivered
- [Feature/change]: [one sentence on what it does and why it matters]
- [Feature/change]: [one sentence]

### Not delivered (moved to next sprint)
- [Item]: [plain-language reason]

### Decisions needed from you
- [Decision]: [context and options]

### Next sprint focus
[2–3 sentences on what's coming next]
```

## Rules for all stakeholder communications
- No technical jargon — if you must use a technical term, define it in parentheses
- Lead with the decision, not the reasoning — stakeholders read the first sentence
- Always state what the stakeholder needs to do next (confirm, decide, or just be informed)
- Never communicate a rejection without offering an alternative or next step

## Gotchas
- "We can't do that because of our architecture" is not a stakeholder communication — translate it
- Stakeholders who don't understand why their request was changed will re-raise it — explain the business reason, not the technical one
- A change impact communication without options puts the stakeholder in an impossible position — always give them a choice
