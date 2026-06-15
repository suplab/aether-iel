# Æ Aether IEL — Intelligence Engineering Lifecycle

> **Ecosystem Navigation**
>
> | Layer | Repo | Purpose |
> |---|---|---|
> | Philosophy & Hub | [suplab/aether](https://github.com/suplab/aether) | Vision, architecture, standards — ecosystem index |
> | **Methodology** | `suplab/aether-iel` ← **you are here** | Intelligence Engineering Lifecycle framework |
> | Cognitive Engine | [suplab/aether-core](https://github.com/suplab/aether-core) | Personal cognitive engine — L4 platform |
> | Agent Mesh | [suplab/aether-grid](https://github.com/suplab/aether-grid) | Distributed agent governance mesh |

---

**Aether IEL** is the methodological backbone of the Aether ecosystem — a repeatable 12-phase framework for engineering, operating, governing, and evolving persistent intelligence systems. It has no runtime code. It provides structure, standards, templates, and governance controls for every intelligence system built on the platform.

---

## The 12 AIEL Phases

| # | Phase | Key Question | Primary Artifact |
|---|---|---|---|
| 1 | Strategy Engineering | Why does this intelligence exist? | Intelligence Brief |
| 2 | Cognitive Architecture | How should it reason and remember? | Cognitive Design |
| 3 | Knowledge Engineering | What does it need to know? | Knowledge Graph + RAG Config |
| 4 | Memory Engineering | What persists, what decays? | Memory Schema + Retention Policy |
| 5 | Agent Engineering | What agents exist and what authority? | Agent Contracts |
| 6 | Implementation Engineering | Build it to spec | Code, APIs, Infra, Prompts |
| 7 | Evaluation Engineering | How well does it actually work? | Evaluation Report |
| 8 | Operational Engineering | Is it observable and reliable? | Observability Dashboard |
| 9 | Governance Engineering | Who controls it and how? | Policy + Audit Trail |
| 10 | Learning Engineering | Is it improving over time? | Learning Log |
| 11 | Evolution Engineering | How does it grow in capability? | Evolution Strategy |
| 12 | Retirement Engineering | How does it end gracefully? | Retirement Plan |

---

## Intelligence Maturity Model (IMM)

| Level | Name | Phases Applied |
|---|---|---|
| L1 | Prompt Experiments | — |
| L2 | AI Applications | 1 – 3 |
| L3 | Agent Systems | 1 – 7 |
| L4 | Cognitive Platforms | 1 – 10 |
| L5 | Distributed Intelligence Ecosystems | All 12 |

---

## Repository Structure

```
aether-iel/
├── CLAUDE.md                         ← EEIK project brief
├── README.md                         ← This file
├── aether.manifest.yaml              ← EEIK manifest
├── docs/
│   ├── index.html                    ← Visual lifecycle overview (start here)
│   └── phases/                       ← Per-phase detailed guides
├── lifecycle/
│   ├── overview.md                   ← Full lifecycle summary
│   ├── 01-strategy.md
│   ├── 02-cognitive-architecture.md
│   ├── 03-knowledge.md
│   ├── 04-memory.md
│   ├── 05-agent.md
│   ├── 06-implementation.md
│   ├── 07-evaluation.md
│   ├── 08-operations.md
│   ├── 09-governance.md
│   ├── 10-learning.md
│   ├── 11-evolution.md
│   └── 12-retirement.md
├── maturity/
│   └── model.md                      ← Full IMM definition + assessment checklist
├── standards/
│   ├── evaluation-criteria.md        ← Phase 7 evaluation thresholds
│   └── governance-controls.md        ← Phase 9 governance control catalogue
└── templates/
    ├── intelligence-brief.md         ← Phase 1 artifact template
    ├── cognitive-design.md           ← Phase 2 artifact template
    ├── agent-contract.md             ← Phase 5 artifact template
    └── evaluation-report.md          ← Phase 7 artifact template
```

---

## Golden Rules

1. **Documentation First** — no implementation without a completed Intelligence Brief
2. **Decisions in ADRs** — every significant decision is captured with context and consequences
3. **Brief Before Code** — Phase 1 artifact must exist before Phase 6 begins
4. **Confidence Gate at 0.8** — confidence < 0.8 → `autoEnforced = false` → human review. Always.
5. **Memory is First-Class** — memory schema designed in Phase 4, not discovered in Phase 6
6. **Explicit Agent Authority** — every agent has a signed Agent Contract
7. **Evaluation is Continuous** — Phase 7 repeats every evolution cycle
8. **Governance at Phase 1** — governance built in from the beginning, not added at the end

---

## Quick Start

**Starting a new intelligence system?**

1. Read [`lifecycle/overview.md`](lifecycle/overview.md)
2. Complete [`templates/intelligence-brief.md`](templates/intelligence-brief.md) (Phase 1)
3. Assess your current maturity level using [`maturity/model.md`](maturity/model.md)
4. Work through the AIEL phases in order — each phase has a gate before the next begins

**Using Claude Code with this repo?**

The EEIK bootstrap (`.claude/`) is pre-configured with:
- **Memory files** — context, glossary, patterns, decisions, session log
- **Agents** — `aiel-advisor`, `maturity-assessor`, `governance-officer`
- **Commands** — `/intelligence-brief`, `/cognitive-design`, `/agent-contract`, `/evaluation-report`, `/maturity-assessment`, `/adr`

---

## Ecosystem Links

| Resource | Link |
|---|---|
| Aether Ecosystem Hub | [github.com/suplab/aether](https://github.com/suplab/aether) |
| Aether Core | [github.com/suplab/aether-core](https://github.com/suplab/aether-core) |
| Aether Grid | [github.com/suplab/aether-grid](https://github.com/suplab/aether-grid) |
| Visual Overview | [docs/index.html](docs/index.html) |

---

*Bootstrapped with EEIK · Governed by [Aether Standards](https://github.com/suplab/aether)*
