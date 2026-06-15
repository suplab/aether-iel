# Phase 1 — Strategy Engineering

> **AIEL Phase Group: Intelligence Design**
> The first phase defines what you are building, why it needs to be intelligent, and whether the organization is ready to govern it. Nothing is implemented until this phase is approved.

---

## Purpose

Phase 1 produces the **Intelligence Brief** — the governing document for everything that follows. It answers three questions: What problem requires persistent intelligence to solve? What does success look like, measured precisely? Who is accountable for the system's decisions?

If you cannot answer all three, you are not ready to begin.

---

## What This Phase Is Not

Phase 1 is not a requirements document in the traditional sense. It does not specify screens, endpoints, or data models. It establishes the **intelligence contract**: the scope of autonomous decision-making, the human oversight model, and the criteria by which the system will be judged — before a line of code is written.

Skipping this phase and returning later to bolt governance on is one of the most common and costly mistakes in intelligence engineering. Governance retrofitted is governance that never fully works.

---

## Key Activities

**1. Problem Framing**

Define what makes this problem require persistent memory and autonomous reasoning rather than conventional software. A system is a candidate for intelligence engineering if it exhibits one or more of these properties: it must learn from interactions over time; it must reason under uncertainty with incomplete information; it must operate across sessions without re-establishing context; or it must make decisions that require judgment, not just rule-matching.

Document the problem in plain language first, then identify which of these properties apply and why conventional approaches fall short.

**2. Stakeholder Analysis**

Identify every group that will be affected by the system's decisions. This includes the direct users, the people whose data the system will process, the teams responsible for operating and governing it, and any external parties subject to its outputs. For each group, document their interests, their concerns, and what they need from the system in order to trust it.

This analysis is not optional. Systems that affect people who were not consulted during design consistently underperform on adoption and trust metrics.

**3. Intelligence Scope Definition**

Define the boundary of the system's authority. This is the most critical design decision in Phase 1. The scope establishes what the system is permitted to decide autonomously, what it must escalate for human review, and what is explicitly outside its authority. The confidence gate — decisions with confidence < 0.8 route to human review — applies inside this scope. Outside this scope, the system must not act at all.

Write the scope as a set of permission statements: "The system MAY autonomously..." and "The system MUST NOT..." This language will appear verbatim in the governance controls defined in Phase 9.

**4. Success Criteria**

Define what success means in measurable terms before the system is built. Targets should cover functional accuracy (what percentage of decisions must be correct?), latency (what is the maximum acceptable response time at p99?), safety (what is the acceptable rate of high-confidence incorrect decisions?), and user experience (what adoption and satisfaction targets are required?).

These criteria become the evaluation thresholds in Phase 7. If you cannot define them now, you cannot evaluate success later.

**5. Maturity Assessment**

Use the Intelligence Maturity Model to establish the current and target maturity level:

| Level | Name | Characteristic |
|---|---|---|
| L1 | Prompt Experiments | LLM calls with no memory or governance |
| L2 | Knowledge-Augmented | RAG pipeline, structured retrieval |
| L3 | Persistent Memory | Cross-session memory, user context |
| L4 | Cognitive Platform | Full memory system, governed agents |
| L5 | Distributed Intelligence | Multi-system coordination, collective learning |

Document the current level, the target level, and why achieving the target level is necessary for the stated problem. Aiming too high wastes resources; aiming too low produces a system that cannot solve the problem.

**6. Risk and Readiness Assessment**

Identify the risks the system poses — to users, to the organization, and to external parties — and assess the organization's readiness to govern those risks. Key readiness indicators include: Is there a designated governance officer? Is there a data stewardship model for the data the system will process? Is there an incident response process for intelligence failures?

If critical readiness gaps exist, Phase 1 should produce a readiness plan in addition to the Intelligence Brief. Do not proceed to Phase 2 with unresolved governance gaps.

---

## Primary Artifact: Intelligence Brief

The Intelligence Brief is the deliverable of Phase 1. It is a concise, signed document covering:

- Problem statement with evidence that persistent intelligence is required
- Stakeholder map with interests and concerns
- Intelligence scope as explicit permission and prohibition statements
- Success criteria with numeric targets for each dimension
- IMM current level, target level, and justification
- Risk assessment with severity ratings
- Readiness assessment with any gaps and remediation plan
- Phase 2 entry criteria confirmation

The Intelligence Brief is not an internal design document. It should be readable by a governance officer, a domain stakeholder, and a technical architect without translation.

---

## Phase Gate

Before Phase 2 (Cognitive Architecture) begins, the following must be true:

- [ ] Problem statement is written and validated by at least one domain stakeholder
- [ ] Intelligence scope is documented with explicit permission and prohibition statements
- [ ] Success criteria are defined with numeric targets for all dimensions
- [ ] IMM current and target levels are agreed
- [ ] Risk assessment is complete with severity ratings for all identified risks
- [ ] Governance officer is identified and named
- [ ] Readiness gaps (if any) are documented with owners and timelines
- [ ] Intelligence Brief is reviewed and signed by architect, governance officer, and at least one domain stakeholder

| Role | Name | Signature | Date |
|---|---|---|---|
| Architect | | | |
| Governance Officer | | | |
| Domain Stakeholder | | | |

---

## Common Failures

**Vague scope.** "The system will help users with their tasks" is not a scope. It specifies no authority boundary, no confidence model, and no escalation path. Every word in the scope statement must be actionable.

**Deferred governance.** "We'll add governance controls once the system is working" produces systems that are ungovernable. Governance constraints discovered late require architectural changes that become progressively more expensive.

**Missing stakeholders.** Systems designed without input from affected parties — particularly those whose data is processed — consistently face adoption failures and compliance challenges. If a stakeholder group is identified after Phase 1, the Intelligence Brief must be updated and re-approved.

**Optimistic success criteria.** Targets set without baseline data or industry benchmarks are usually wrong. If you have no baseline, run a discovery spike before finalizing targets.

---

## Inputs

- Business or mission context
- Existing system documentation (if replacing or augmenting)
- Regulatory or compliance requirements
- Data availability assessment

## Outputs

- **Intelligence Brief** (signed artifact, baseline version controlled in `/aether-iel/artifacts/`)
- Identified Phase 2 entry criteria
- Stakeholder RACI table
- Risk register (initial)

## Feeds Into

Phase 2 (Cognitive Architecture) — the Cognitive Design requires an approved Intelligence Brief as its first input. All decisions made in Phase 2 must be traceable to a statement in the Intelligence Brief.
