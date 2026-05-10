# Agentic Development Team

Resources for structuring and operating a software delivery team that uses agentic AI development tools (GitHub Copilot, Cursor, Kiro, Claude Code, Devin, etc.).

## Contents

```
├── team-structure.md        # Research report: how traditional roles evolve with agentic AI tools
├── agents/                  # Assisting agents for each team role
│   ├── ai-architect/        # Senior AI Architect / Tech Lead
│   ├── senior-engineer/     # Senior Engineer
│   ├── quality-engineer/    # Quality Engineer / SDET
│   ├── ba-product-analyst/  # BA / Product Analyst
│   ├── devops-platform/     # DevOps / Platform Engineer
│   └── ai-execution-agent/  # AI coding agent (execution layer)
└── skills/                  # Reusable agent skills (install into your AI tool)
    ├── deep-research/        # Rigorous, citation-backed research on any topic
    ├── create-agent/         # Design and configure a new AI agent
    └── create-agent-skill/   # Author new agent skills (SKILL.md format)
```

---

## team-structure.md

A research-backed report (25 sources, May 2026) covering:

- How each traditional role changes when the team adopts agentic AI tools
- What gets automated, what becomes more important, what new skills are required
- Recommended pod structure and team topology
- Headcount implications and real company examples (Salesforce, Klarna, Coinbase)

**Use this** when planning a team transition, hiring, or explaining role changes to stakeholders.

---

## agents/

Each subdirectory contains a ready-to-use agent configuration for one team role.

### File structure per agent

```
agents/<role>/
├── AGENT.md          # Steering file — always-on context, rules, and constraints
└── skills/
    └── <skill>.md    # On-demand procedures, loaded when the task matches
```

**AGENT.md** is the agent's steering file: facts about the role, universal rules, and standing constraints. Keep it loaded at all times for that agent.

**skills/*.md** are on-demand procedures: step-by-step workflows the agent follows for specific tasks. Load a skill when the task matches its description.

### Agents and their skills

| Agent | AGENT.md covers | Skills |
|---|---|---|
| `ai-architect` | Governance rules, risk tiers, SDLC pipeline | `agent-job-spec`, `ai-code-review`, `architecture-decision-record`, `tech-debt-triage`, `governance-checklist` |
| `senior-engineer` | Orchestration rules, blast radius guide | `spec-generator`, `agent-failure-diagnostician`, `context-file-curator`, `codebase-health-audit` |
| `quality-engineer` | Risk zones, six-layer testing strategy | `adversarial-code-review`, `eval-suite-builder`, `test-strategy-charter` |
| `ba-product-analyst` | Spec depth guide, 7-section spec structure | `spec-drafter`, `spec-completeness-auditor`, `impact-assessor`, `requirement-triage`, `acceptance-criteria-generator`, `stakeholder-communicator` |
| `devops-platform` | Model routing tiers, cost control levers | `agent-identity-governance`, `agent-cost-investigator`, `agentic-pipeline-architect` |
| `ai-execution-agent` | Capabilities, guardrails, escalation triggers, PR template | `pr-writer`, `escalation-handler` |

---

## skills/

Reusable agent skills used to build and maintain this repository. Install them into your AI tool to use them in your own projects.

| Skill | What it does |
|---|---|
| `deep-research` | Conduct rigorous, citation-backed research on any topic using iterative search and evidence grading |
| `create-agent` | Design and configure a new AI agent — steering file, skills, and the balance between them |
| `create-agent-skill` | Author new agent skills conforming to the agentskills.io open standard (SKILL.md format) |

### Installing skills

**Kiro** — copy to your skills directory:
```bash
# User-scoped (available in all projects)
cp -r skills/deep-research skills/create-agent skills/create-agent-skill ~/.kiro/skills/

# Project-scoped (available in this repo only)
cp -r skills/deep-research skills/create-agent skills/create-agent-skill .kiro/skills/
```

**Claude Code** — copy to your Claude skills directory:
```bash
cp -r skills/deep-research skills/create-agent skills/create-agent-skill ~/.claude/skills/
```

**Cursor / other tools** — paste the relevant `SKILL.md` content into the conversation when starting a task that matches the skill's description.

---

## How to use the agents

### With Kiro, Claude Code, or Cursor

1. Copy the relevant `AGENT.md` content into your agent's system prompt or steering file
2. Copy the `agents/<role>/skills/` files into your agent's skills directory
3. The agent will load skills automatically when the task matches the skill's description

### With any other AI tool

1. Paste the `AGENT.md` content as the system prompt
2. When starting a task that matches a skill, paste the relevant skill content into the conversation

### Adapting to your project

Each `AGENT.md` contains placeholder conventions. Before using:

1. Replace build/test commands with your actual commands
2. Add your project's naming conventions and architectural constraints
3. Add your team's escalation paths and review policies
4. Remove any sections that don't apply to your stack

---

## Key concepts

**Steering file vs skill** — The `AGENT.md` is always loaded (standing facts and rules). Skills are loaded on demand (step-by-step procedures). Don't put procedures in the steering file; don't put standing rules in skills.

**Every agent needs a human owner** — Each AI agent job must have a named person accountable for its outputs. The `ai-execution-agent` AGENT.md enforces this.

**Spec quality is the primary quality lever** — Vague specs produce bad agent output at machine speed. The BA and Senior Engineer agents both have spec-writing skills for this reason.

**AI-generated code has higher defect rates** — 1.7× more major issues, 2.74× more security vulnerabilities than human-written code (CodeRabbit, 2025). The `quality-engineer` and `ai-architect` agents are configured with this in mind.
