# Software Delivery Team Structure in the Agentic AI Era

> Research date: May 2026 | Sources: 25 primary sources (Forrester, Gartner, GitHub, Wiley/TechXplore, Nielsen Norman Group, CircleCI, IBM, AWS, and others)

---

## The Big Picture

Agentic AI tools (GitHub Copilot, Cursor, Devin, Claude Code, Kiro, etc.) do not replace the team — they reshape it. The primary shift is from **execution** to **orchestration and governance**.

### Team Shape Change

| Era | Structure |
|---|---|
| Pre-AI (2020–2023) | 1 Architect → 2–3 Seniors → 4–6 Mid-level → 2–3 Juniors |
| AI-first (2026) | 1–2 Architects → 3–4 Seniors → 1–2 Mid-level → 0 Juniors → 3–5 AI Agents |

### Key Statistics
- 92% of US developers use AI coding tools daily (Stack Overflow, 2025)
- 27% of all production code is AI-authored in advanced teams (DX Research, n=121,000)
- AI writes ~30% of new code at Microsoft and Google
- GitHub Copilot study: 55.8% faster task completion; 15% more capacity without additional headcount
- IBM internal: 60% reduction in development time with AI-native workflows
- **Critical warning (CircleCI, 2026):** AI boosted feature branch throughput 59%, yet production output *declined* 7% for median teams. Code is written faster than it ships — the bottleneck moves from generation to review and integration. Team structure determines whether AI actually delivers.

---

## Role-by-Role Evolution

### Business Analyst

**Core shift:** From documentation-heavy requirements gathering to being the critical upstream translator. The BA's output now directly feeds AI agents — vague requirements produce bad agent output.

| | Detail |
|---|---|
| **Automated away** | Manual data collection, standard requirements docs from meeting notes, basic process mapping, gap analysis for common patterns |
| **More important** | Writing precise, structured specifications that AI agents can execute without ambiguity; stakeholder facilitation; domain expertise and business context AI lacks; validating AI-generated requirements for business intent |
| **New skills** | AI literacy, prompt engineering for requirements elicitation, data analysis, prototyping with AI tools (Figma AI, v0) |
| **Qualifications** | Business degree + domain expertise remains the foundation. Growing expectation for data literacy and AI tool fluency. The "pure documentation BA" role is under pressure. |

---

### Technical BA / Systems Analyst

**Core shift:** Converges toward a "bridge architect" role — translates business requirements into technical specifications precise enough for AI agents to execute, then validates that AI-generated systems match business intent.

| | Detail |
|---|---|
| **Automated away** | Data dictionaries and API specs from existing systems, standard integration mapping, generating test scenarios from requirements |
| **More important** | Writing agentic-ready specifications (explicit constraints, edge cases, acceptance criteria); setting realistic stakeholder expectations around AI limitations |
| **New skills** | API design, data modelling, understanding of LLM context and constraints, agentic workflow patterns |
| **Qualifications** | Technical background + domain expertise. Increasing overlap with both BA and developer roles. |

---

### Software Developer / Engineer

**Core shift:** From code *writer* to AI *orchestrator*. Daily loop: define intent → direct agents → review/validate → orchestrate testing → monitor.

| | Detail |
|---|---|
| **Automated away** | Boilerplate, CRUD, first-draft features, unit tests, docs, dependency updates, minor refactors. Fully autonomous ticket→PR workflows are standard for 55% of teams. |
| **More important** | Specification engineering (precise prompts/constraints for agents), architectural decomposition, reviewing AI output for correctness and security |
| **New skills** | Prompt/constraint engineering, multi-agent frameworks (LangGraph, AutoGen, CrewAI), agent governance (scope, guardrails, escalation), AI cost awareness as an architectural constraint |
| **Qualifications** | CS fundamentals remain essential. New hires increasingly expected to have non-programming skills (communication, domain knowledge, product thinking) *without* losing coding ability. Junior roles are shrinking — entry-level tasks are being automated. |

> **Security note:** AI-generated code has 1.7× more major issues and 2.74× higher security vulnerability rates than human-written code (CodeRabbit, 470 PRs, 2025). Human review is not optional.

---

### Tech Lead

**Core shift:** Becomes the primary human-in-the-loop for AI agent governance. Defines which tickets go autonomous, sets quality gates for AI PRs, owns the agentic workflow infrastructure.

| | Detail |
|---|---|
| **Automated away** | Routine code review of well-specified features, sprint task breakdown, first-pass architecture for standard patterns |
| **More important** | Reviewing AI *reasoning*, not just AI *output*; governing security posture of autonomous systems (prompt injection is a real attack vector); mentoring juniors — a looming risk since entry-level tasks are disappearing |
| **New skills** | Multi-agent system design, AI governance, security review for AI-generated code |
| **Qualifications** | Senior engineering experience + demonstrated ability to govern autonomous systems. |

---

### Solution Architect

**Core shift:** Role is *elevated*. As AI handles implementation, architectural decisions become the primary determinant of delivery quality.

| | Detail |
|---|---|
| **Automated away** | Generating architecture diagrams from existing code, standard ADRs for common patterns, capability mapping documentation |
| **More important** | Designing multi-agent architectures; defining guardrails and trust boundaries; ensuring coherence when AI generates code across multiple modules simultaneously; auditability as a first-class design concern |
| **New skills** | Agentic system design patterns, observability-as-code, LLM failure mode understanding at the architectural level |
| **Qualifications** | Deep distributed systems experience + demonstrated ability to design systems *around* AI agents. |

---

### QA / Tester

**Core shift:** The most dramatically transformed role. Traditional manual QA is largely automated. The role is evolving into SDET (Software Development Engineer in Test) with AI/ML focus.

| | Detail |
|---|---|
| **Automated away** | Writing regression scripts, running manual test cases, basic bug reporting, test data generation, selector maintenance (self-healing tools) |
| **More important** | Validating AI-generated code against *real business intent* (not just functional correctness); exploratory testing; designing test *strategies* not test *scripts*; security and compliance testing; testing the AI outputs themselves |
| **New skills** | AI/ML testing concepts, prompt engineering for test generation, test strategy design, static analysis and security scanning, Python or TypeScript for framework customisation |
| **Qualifications** | Pure manual testers face the most direct displacement. Coding ability is now expected. The market is bifurcating — upskilled quality engineers command significantly higher salaries. |

---

### DevOps / Infrastructure Engineer

**Core shift:** AI agents are moving from passive monitoring to active operators — handling incidents, optimising infrastructure, scaling resources proactively.

| | Detail |
|---|---|
| **Automated away** | Routine incident triage, infrastructure scaling decisions, standard pipeline configuration, log analysis, dependency vulnerability scanning. Teams report 70% MTTR reduction and 90% fewer alert fatigue incidents with AI-driven ops. |
| **More important** | Designing platforms *for* autonomous agent workflows; governing agent permissions and credentials (principle of least privilege for AI agents); platform engineering (Internal Developer Platforms); FinOps for AI workloads |
| **New skills** | AI agent permission architecture, platform engineering, FinOps for AI, observability for non-deterministic systems, policy-as-code |
| **Qualifications** | DevOps/SRE background + platform engineering mindset. Cloud certifications remain relevant. |

---

### Project Manager / Scrum Master

**Core shift:** AI automates administrative and coordination overhead. Human elements — trust, vision, strategic leadership — become more valuable, not less.

| | Detail |
|---|---|
| **Automated away** | Sprint velocity reporting, meeting notes and action items, risk register updates for standard patterns, status report generation, backlog grooming assistance. Teams report 25% reduction in meeting time and 30% improvement in action item completion with AI assistants. |
| **More important** | Stakeholder management and trust-building; managing the human impact of AI-driven restructuring; governing AI agent workflows as a delivery risk; shifting from activity metrics to value/outcome metrics |
| **New skills** | AI tool literacy (Jira AI, Linear, Notion AI), understanding of agentic development workflows, change management for AI adoption |
| **Qualifications** | PM/Scrum experience + change management capability. CSPO, PMI-ACP remain relevant. |

---

### UX / UI Designer

**Core shift:** AI commoditises pixel-level UI execution. The market is bifurcating into two high-value specialisations: the *Trust Designer* (research, ethics, human judgment) and the *AI Architect* (system orchestration, AI experience design).

| | Detail |
|---|---|
| **Automated away** | Low-fidelity wireframing, UI component generation, basic asset creation, responsive layout generation, accessibility audit automation |
| **More important** | User research and synthesis; designing for AI-driven interfaces (uncertainty, explainability, non-deterministic outputs); design systems governance; ethical design and bias mitigation |
| **New skills** | AI UX design patterns, prompt engineering for design tools (Figma AI, Midjourney, v0), prototyping with AI-generated code |
| **Qualifications** | Portfolio demonstrating strategic thinking, not just execution. AI tool fluency is now table stakes. Designers who "only push pixels" face 30–40% salary stagnation vs. specialised counterparts (ShiftUX, 5,200+ job postings, 2026). |

---

## Recommended Pod Structure

For each product stream:

| Role | Count | Notes |
|---|---|---|
| Senior AI Architect / Tech Lead | 1 | Agent governance, architecture, quality gates |
| Senior Engineers | 2–3 | AI orchestrators, complex problem-solving |
| Quality Engineer / SDET | 1 | AI output validation, test strategy |
| BA / Product Analyst | 1 | Requirements precision, stakeholder alignment |
| DevOps / Platform Engineer | shared | Owns agent infrastructure across pods |
| AI agents | 3–5 | Execution layer: coding, testing, docs |

**Shared across pods:** Solution Architect, UX/UI Designer, AI Workflow Engineer.

**Key principle:** Every AI agent needs a named human owner accountable for its outputs.

---

## New Roles Emerging

These roles are in active talent competition as of 2026:

| Role | Purpose |
|---|---|
| **AI Workflow Engineer** | Designs which tasks go autonomous vs. need human checkpoints; owns toolchain selection and impact measurement. Senior role requiring mastery, not just familiarity. |
| **Agent Ops** | Monitors agent performance, manages permissions/credentials, governs security posture of autonomous development systems. |
| **Prompt Architect** | Designs instruction sets and constraint frameworks governing agent decision-making at scale. Rare combination of technical depth and communication precision. |
| **AI Governance Officer** | Ensures compliance, auditability, ethical use. Critical in regulated industries. |
| **AI Champion** | Drives cultural adoption across teams. Often an existing senior engineer or PM. |

---

## On Headcount

The honest answer depends on the company's strategy:

- **Growth-stage companies** are reinvesting productivity gains into scope expansion, not headcount reduction (Wiley/TechXplore empirical study, 2026)
- **Mature/cost-focused companies** are reducing headcount: Salesforce paused engineering hires, Coinbase cut 14% citing AI efficiency
- **Klarna** halved its workforce (7,400 → 3,000), then reversed course when AI customer service quality failed — the lesson being that efficiency gains are real but wholesale replacement without quality governance fails
- **Forrester's recommendation:** "Reskill, don't downsize. The fastest ROI comes from upskilling existing teams."

The clearest structural shift: **junior/entry-level roles are shrinking fastest.** The talent pipeline problem this creates is a known risk that most organisations haven't solved yet.

---

## Skills Summary by Role

| Role | Must-Have New Skills | Qualifications Shift |
|---|---|---|
| Business Analyst | AI literacy, precise specification writing, data analysis | Add: data literacy, AI tool fluency |
| Technical BA | Agentic workflow patterns, LLM context understanding | Converging with developer role |
| Developer | Prompt engineering, agent orchestration, AI output review | CS fundamentals + non-programming skills |
| Tech Lead | AI governance, multi-agent system design, security review | Senior SWE + governance mindset |
| Solution Architect | Agentic system design, observability-as-code | Elevated importance; deep distributed systems |
| QA / Tester | AI testing concepts, test strategy design, coding | Coding now required; manual-only path closing |
| DevOps | Platform engineering, agent permission architecture, FinOps | SRE + platform engineering mindset |
| Project Manager | AI workflow understanding, change management, outcome metrics | Add: change management capability |
| UX / UI Designer | AI UX patterns, design systems governance, AI tool fluency | Strategic thinking portfolio over execution portfolio |

---

## Sources

1. Forrester — "AI Is Rewriting Software Work" (Nov 2025) — forrester.com/blogs/ai-is-rewriting-software-work
2. Wiley/TechXplore — GitHub Copilot workforce study (Apr 2026) — techxplore.com/news/2026-04-generative-ai-tools-reshape
3. Jellyfish — Copilot 15% capacity study (Mar 2025) — jellyfish.co/blog/with-copilot-engineers-get-15-more-capacity
4. UCI — "The Quiet Revolution in How We Work" (Apr 2026) — merage.uci.edu/news/2026/04
5. CircleCI — State of Software Delivery 2026 — circleci.com/resources/2026-state-of-software-delivery
6. CodeRabbit — AI code security vulnerability study (Dec 2025) — coderabbit.ai/blog
7. Gartner — "AI Agents Transforming Software Engineering" (May 2026) — gartner.com/en/articles/ai-agents-transforming-software-engineering
8. IBM — "Beyond Shift Left: AI Agents in DevOps" (Apr 2026) — ibm.com/think/insights/ai-in-devops
9. Forbes/AWS — "AWS Deploys AI Agents for DevOps" (Apr 2026) — forbes.com/sites/janakirammsv/2026/04/01
10. Nielsen Norman Group — "State of UX in 2026" (Apr 2026) — nngroup.com/articles/state-of-ux-2026
11. ShiftUX — "State of AI Design 2026" — shiftux.co/report
12. Particle41 — "Future of Software Dev Teams" (Apr 2026) — particle41.com/insights/future-software-development-teams-ai-first
13. Virtido — "Agentic AI Engineering Teams 2026" (Mar 2026) — virtido.com/blog/agentic-ai-engineering-team-2026
14. Fortune — "Klarna CEO halved workforce" (Oct 2025) — fortune.com/2025/10/10/klarna-ceo
15. Digital Applied — "Klarna Reverses AI Layoffs" (Mar 2026) — digitalapplied.com/blog/klarna-reverses-ai-layoffs
16. Stack Overflow — Developer Survey 2025 — survey.stackoverflow.co/2025
17. DX Research — AI code authorship study (2025) — getdx.com/research
18. QuashBugs — "AI in QA: The Complete Guide 2026" (Apr 2026) — quashbugs.com/blog/ai-in-qa-the-complete-guide-2026
19. Courseific — "2026 US Business Analyst Landscape" (Apr 2026) — courseific.com/blog/the-2026-us-business-analyst-landscape
20. Salesforce — "How Agentic AI Is Creating New Jobs" (May 2026) — salesforce.com/news/stories/how-agentic-ai-creates-new-jobs
