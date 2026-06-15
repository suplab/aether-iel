# CLAUDE.md — Aether Intelligence Engineering Lifecycle (AIEL)

> Read this at the start of every session. Single source of truth for what this project is, how it is structured, and what rules apply.

---

## What This Project Is

**Aether IEL** (`suplab/aether-iel`) is the **Intelligence Engineering Lifecycle** — a methodology framework defining how to engineer, operate, govern, and evolve persistent intelligence systems.

It is the methodological backbone of the entire Aether ecosystem.

> **Ecosystem Navigation**
>
> | Layer | Repo | Purpose |
> |---|---|---|
> | Aether Philosophy | [`suplab/aether`](https://github.com/suplab/aether) | Vision, architecture, standards — ecosystem index |
> | **Aether IEL** | `suplab/aether-iel` ← **you are here** | Intelligence Engineering Lifecycle methodology |
> | Aether Core | [`suplab/aether-core`](https://github.com/suplab/aether-core) | Personal cognitive engine |
> | Aether Grid | [`suplab/aether-grid`](https://github.com/suplab/aether-grid) | Distributed agent mesh |

**This repository has no runtime code.** It contains:
- Lifecycle methodology documentation
- Phase-by-phase engineering guides
- Standards and checklists for each phase
- Templates for artifacts
- Maturity model definitions
- EEIK governance bootstrap

---

## Repository Structure

```
aether-iel/
├── CLAUDE.md                         ← This file
├── README.md                         ← Entry point
├── aether.manifest.yaml              ← EEIK manifest
├── docs/
│   ├── index.html                    ← Visual lifecycle overview
│   └── phases/                       ← Detailed per-phase guides
├── lifecycle/
│   ├── overview.md                   ← Full lifecycle summary
│   ├── 01-strategy.md                ← Strategy Engineering
│   ├── 02-cognitive-architecture.md  ← Cognitive Architecture
│   ├── 03-knowledge.md               ← Knowledge Engineering
│   ├── 04-memory.md                  ← Memory Engineering
│   ├── 05-agent.md                   ← Agent Engineering
│   ├── 06-implementation.md          ← Implementation Engineering
│   ├── 07-evaluation.md              ← Evaluation Engineering
│   ├── 08-operations.md              ← Operational Engineering
│   ├── 09-governance.md              ← Governance Engineering
│   ├── 10-learning.md                ← Learning Engineering
│   ├── 11-evolution.md               ← Evolution Engineering
│   └── 12-retirement.md              ← Retirement Engineering
├── maturity/
│   └── model.md                      ← Intelligence Maturity Model (IMM)
├── standards/
│   ├── evaluation-criteria.md        ← Evaluation standards
│   └── governance-controls.md        ← Governance controls
└── templates/
    ├── intelligence-brief.md         ← New intelligence system brief
    ├── cognitive-design.md           ← Cognitive architecture template
    ├── agent-contract.md             ← Agent contract template
    └── evaluation-report.md          ← Evaluation report template
```

---

## The 12 AIEL Phases

| # | Phase | Key Question | Primary Artifact |
|---|---|---|---|
| 1 | Strategy Engineering | Why does this intelligence exist? | Intelligence Brief |
| 2 | Cognitive Architecture | How should it reason and remember? | Cognitive Design |
| 3 | Knowledge Engineering | What does it need to know? | Knowledge Graph + RAG Config |
| 4 | Memory Engineering | What persists, what decays? | Memory Schema + Retention Policy |
| 5 | Agent Engineering | What agents exist and what authority? | Agent Contracts |
| 6 | Implementation Engineering | Build it | Code, APIs, Infra, Prompts |
| 7 | Evaluation Engineering | How well does it work? | Evaluation Report |
| 8 | Operational Engineering | Is it observable? | Observability Dashboard |
| 9 | Governance Engineering | Who controls it? | Policy + Audit Trail |
| 10 | Learning Engineering | Is it improving? | Learning Log |
| 11 | Evolution Engineering | How does it grow? | Evolution Strategy |
| 12 | Retirement Engineering | How does it end? | Retirement Plan |

---

## Intelligence Maturity Model

| Level | Name | Characteristics |
|---|---|---|
| 1 | Prompt Experiments | One-off LLM calls, no memory, no agents |
| 2 | AI Applications | RAG or fine-tuned models, persistent knowledge |
| 3 | Agent Systems | Multi-agent orchestration, tool use, feedback loops |
| 4 | Cognitive Platforms | Persistent memory, identity, emotional context |
| 5 | Distributed Intelligence | Federated memory, collective learning, autonomous evolution |

---

## Golden Rules (from Aether Standards)

All Aether ecosystem standards apply here. Key rules:
1. Documentation First — no implementation without a design
2. Decisions must be captured in ADRs
3. Every intelligence system needs a brief (Phase 1 artifact) before any code is written
4. Confidence < 0.8 → human review always (from Agent Design Standard)
5. Memory and knowledge are first-class concerns, not afterthoughts
6. All agents must have explicit authority boundaries
7. Evaluation is continuous, not a final gate
8. Governance is built in from Phase 1, not added at the end

---

## Slash Commands

| Command | Purpose |
|---|---|
| `/intelligence-brief` | Generate a new intelligence system brief (Phase 1) |
| `/cognitive-design` | Generate a cognitive architecture template |
| `/agent-contract` | Generate an agent contract |
| `/evaluation-report` | Generate an evaluation report template |
| `/maturity-assessment` | Run a maturity level assessment |
| `/adr` | Create an Architecture Decision Record |

---

## Memory Files

| File | Contents |
|---|---|
| `project-context.md` | AIEL scope, current phase coverage, ecosystem links |
| `domain-glossary.md` | AIEL-specific terminology |
| `decisions.md` | Key methodology decisions |
| `patterns.md` | Repeating patterns across intelligence systems |
| `session-log.md` | Rolling session log |
