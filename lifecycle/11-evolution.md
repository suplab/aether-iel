# Phase 11 — Evolution

> **AIEL Phase Group: Intelligence Evolution**
> Phase 11 is how the system improves structurally over time. It is a return to the Design phases, informed by operational evidence. Evolution is triggered by data — not by intuition, roadmap timelines, or stakeholder requests alone.

---

## Purpose

Every intelligence system has a maturity ceiling — a level at which its current architecture can no longer improve without structural change. When operational evidence (from Phase 10) shows that accuracy is declining, correction rates are climbing, or new capabilities are required that exceed the current agent topology, the system enters an Evolution cycle.

An Evolution cycle is not a bug fix sprint. It is a controlled return through some or all of the Design phases (1–5) — re-examining the cognitive model, expanding the memory system, redefining agent authority, or updating the knowledge schema — followed by a full Phase 7 evaluation before the evolved system replaces the current one. The current system continues to serve production while evolution is in progress.

---

## Evolution Triggers

An Evolution cycle is initiated when any of the following conditions is met:

**Accuracy decline**: decision accuracy drops below the Phase 7 threshold (< 0.85) for two consecutive monthly measurements. This indicates that the current cognitive model is no longer adequate for the incoming request patterns.

**Sustained high correction rate**: human review overrides exceed 25% of all reviewed decisions for a sustained period (4+ weeks). This indicates the agent's confidence calibration is significantly miscalibrated or the decision logic is consistently wrong in a specific domain.

**IMM advancement opportunity**: the quarterly IMM assessment identifies that the system has the operational maturity to advance to the next level, and the advancement is aligned with the roadmap. This is a positive trigger — the system has earned its next capability tier.

**New capability requirement**: a business or domain requirement cannot be met by the current agent topology, memory model, or knowledge schema. A new capability that requires a new agent type, a new memory type, or a new knowledge domain requires an Evolution cycle to be properly designed.

**Latency degradation**: p99 latency consistently exceeds 1800ms (approaching the 2000ms hard limit) over a 30-day period. This indicates the current architecture is under stress and structural optimization is required.

---

## Evolution Cycle Process

**Step 1: Evolution Assessment**

Before re-entering any Design phase, conduct an Evolution Assessment. This is a structured analysis of the operational evidence that triggered the cycle:

- What does the Phase 10 learning data show about where the system is failing?
- Which agents have the highest correction rates?
- Which knowledge domains have the most retrieval failures?
- What does the IMM assessment say about current maturity?
- What is the minimal structural change that would address the triggering condition?

The Evolution Assessment prevents over-engineering — teams that jump straight into redesign often solve more than the problem that triggered the cycle, introducing new failure modes unnecessarily.

**Step 2: Scope Definition**

Based on the assessment, define the scope of the Evolution cycle: which Design phases will be revisited? A typical evolution cycle scopes to one or two phases. For example:

- Accuracy decline in a specific agent → return to Phase 5 (Agent Engineering) to redesign the agent contract and decision logic.
- Knowledge retrieval failures in a new domain → return to Phase 3 (Knowledge Engineering) to extend the knowledge schema.
- IMM advancement to Level 4 → return to Phase 2 (Cognitive Architecture) and Phase 4 (Memory Engineering) to enable persistent memory.
- New capability requiring new agent type → return to Phase 5, possibly Phase 2 for topology changes.

The scope definition must be approved by the governance officer before work begins. Scope creep in Evolution cycles is a common failure — define the boundary explicitly.

**Step 3: Design**

Execute the scoped Design phases. All the same standards apply as in the original design: artifacts must be produced, ADRs must be filed for new decisions, and contracts must be updated. Evolution work is not exempt from design discipline.

**Step 4: Implementation**

Implement the evolution changes in a branch of the production codebase. The production system continues to serve requests while evolution is implemented. Use the same implementation standards from Phase 6.

**Step 5: Regression Evaluation**

Before the evolved system can replace the current one, it must pass a regression evaluation using the Phase 7 criteria — with an additional constraint: the evolved system must not perform worse than the current system on any metric.

The regression criteria from the ecosystem evaluation standards:

| Metric | Maximum Allowed Regression |
|---|---|
| Decision accuracy | -0.02 (from current production baseline) |
| Memory retrieval precision@K | -0.02 |
| p99 latency | +200ms from current production baseline |

Any regression beyond these limits is a blocking failure. The Evolution Assessment must be revisited.

**Step 6: Deployment**

Deploy the evolved system using the same Phase 8 deployment strategy: canary first, then full rollout. The old system version is retained in the container registry for rollback for a minimum of 30 days.

---

## IMM Advancement

When an Evolution cycle targets IMM level advancement, the advancement must be confirmed through a formal IMM Assessment after the evolved system has been running in production for 30 days. The assessment evaluates whether the system is genuinely operating at the higher level — not whether it has the features of the higher level.

| Advancement | Primary Design Phases | Evaluation Focus |
|---|---|---|
| L1 → L2 | Phase 3 (Knowledge) | Retrieval precision@K |
| L2 → L3 | Phase 4 (Memory) + Phase 2 | Cross-session context accuracy |
| L3 → L4 | Phase 5 (Agent) + Phase 9 | Agent governance compliance rate |
| L4 → L5 | Phase 11 (multi-system topology) | Inter-system coordination accuracy |

---

## Versioning Through Evolution

Each Evolution cycle produces a new system version. Versioning follows: `MAJOR.MINOR.PATCH`.

- `MAJOR` increment: IMM level advancement or significant cognitive architecture change.
- `MINOR` increment: new agent, new memory type, new knowledge domain.
- `PATCH` increment: accuracy improvement within existing architecture.

The Intelligence Brief is updated with each Evolution cycle — it is the living specification of the system's intelligence contract. The update records: version, evolution trigger, scope, and what changed. Previous versions of the Intelligence Brief are retained as historical record.

---

## Phase Gate

An Evolution cycle is complete when:

- [ ] Evolution Assessment is documented and signed
- [ ] Scope is defined and approved by governance officer
- [ ] All scoped Design phases have been executed with artifacts produced
- [ ] Implementation is complete with tests passing
- [ ] Regression evaluation is PASS — no metric regresses beyond allowed limits
- [ ] Canary deployment is stable for minimum 24 hours
- [ ] Full deployment is complete
- [ ] Intelligence Brief is updated with the new version record
- [ ] Learning pipeline dataset is updated with any new evaluation examples
- [ ] IMM Assessment is complete (if advancement was part of the scope)

| Role | Name | Signature | Date |
|---|---|---|---|
| Architect | | | |
| Governance Officer | | | |

---

## Inputs

- Phase 10 learning reports (accuracy trends, correction rates)
- IMM Assessment results
- Business requirements for new capabilities
- Performance monitoring data (latency trends)

## Outputs

- Updated Intelligence Brief (new version)
- Updated design artifacts for all phases touched
- Regression Evaluation Report
- New system version in production
- Updated evaluation dataset

## Feeds Into

Phase 7 (Evaluation) — regression evaluation re-runs Phase 7 criteria.
Phase 8 (Deployment) — evolved system deployed through the same process.
Phase 9 (Governance) — governance controls updated for any new agents or authority changes.
Phase 12 (Retirement) — if evolution reveals that the system has exceeded its useful life or that a fundamental redesign is more appropriate than incremental evolution.
