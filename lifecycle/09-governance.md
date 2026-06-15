# Phase 9 — Governance

> **AIEL Phase Group: Intelligence Operations**
> Phase 9 operationalizes the governance framework established in Phases 1 and 5. It is not a one-time setup — it is an ongoing discipline that runs for the lifetime of the system. Governance that exists only in documents is not governance.

---

## Purpose

An intelligence system in production makes decisions that affect people. Those decisions must be: attributable (who or what made the decision?), auditable (what was the reasoning?), correctable (can a wrong decision be reversed?), and contestable (can affected parties challenge a decision?).

Phase 9 implements these properties in the running system through four control categories: Authority, Transparency, Correction, and Oversight. Each control is operational — it exists as running code, scheduled processes, and monitored metrics, not as a policy document.

---

## Control Category 1: Authority

Authority controls ensure that agents act only within the boundaries established in Phase 1 (intelligence scope) and Phase 5 (agent contracts).

**Policy Engine**: The policy engine enforces agent authority at runtime. Every agent action is evaluated against the agent's authority level and the action type's confidence threshold before execution. The policy engine is not advisory — it blocks unauthorized actions and logs every evaluation.

Configuration: the policy engine reads authority rules from the tool registry and agent contracts produced in Phase 5. These rules are immutable at runtime — they cannot be modified through the API or through feature flags. Changes to authority rules require a Phase 1 amendment, a Phase 5 contract update, and a code deployment.

**Tool Permission Enforcement**: The agent runtime enforces tool permissions from the tool registry. An agent attempting to call a tool not in its permission list receives an authorization error that is logged as a security event. This error is never silenced.

**Escalation Path Enforcement**: All decisions with confidence < 0.8 must route to the human review queue. Monitor `confidence_gate_bypass_total` metric — this should be permanently zero. If it is ever non-zero, it is a critical incident requiring immediate investigation.

---

## Control Category 2: Transparency

Transparency controls ensure that every consequential decision can be explained, attributed, and examined.

**Decision Audit Trail**: Every agent decision is logged with: timestamp, agent name, agent version, input summary (not full input — PII must not appear in audit logs), action type, confidence score, auto-enforced or human-review routing, and the human reviewer's decision (if applicable). The audit trail is append-only and immutable — no update or delete operations on audit records.

Retention: audit records are retained for a minimum of 2 years. They are stored separately from the memory system and are not subject to user-initiated deletion (audit records are not personal data — they are operational records of system behaviour).

**Explanation Endpoint**: For any decision that affects a user, the system must be able to produce a human-readable explanation on request. Minimum explanation: the decision type, the inputs that drove it, the confidence score, and whether a human reviewed it. This is the technical implementation of the right to explanation under GDPR Article 22.

**Metric Transparency**: The Grafana dashboards produced in Phase 8 must include a public-facing summary view for stakeholders: number of decisions per day, fraction auto-enforced vs. human-reviewed, average confidence score, and queue processing time. Stakeholders should not need to query the database to understand how the system is operating.

---

## Control Category 3: Correction

Correction controls ensure that wrong decisions can be reversed and that the system learns from corrections.

**Human Review Queue**: The human review queue is the primary correction interface. Reviewers see: the agent's proposed action, the inputs, the confidence score, and the reasoning. Reviewers can: approve the proposed action, override with a different action, or reject and escalate. Every reviewer decision is logged.

The queue must have a defined SLA: decisions in the queue must be reviewed within a defined time window (default: 4 hours for ALERT-level, 1 hour for BLOCK-level). A queue that exceeds SLA triggers an alerting rule.

**Correction Feedback Loop**: When a human reviewer overrides an agent decision, that override is a training signal. The correction must be captured in a structured format and stored in the feedback dataset used by Phase 10 (Learning). Corrections are the most valuable learning signal the system has — an uncaptured correction is wasted evidence.

**Memory Correction**: If a memory is found to be incorrect (it encodes a wrong fact or a misinterpreted event), there must be a process to correct or delete it. Provide an administrative endpoint for authorized operators to delete specific memories by ID. Log all memory corrections in the audit trail.

**Rollback Authority**: Define who can order a rollback of the running system and under what conditions. The governance officer has rollback authority. The engineering lead executes it. The conditions that trigger rollback consideration: confidence gate bypass detected, p99 latency consistently above 2000ms, human review queue backlog growing beyond sustainable levels, or a pattern of incorrect high-confidence decisions discovered through audit.

---

## Control Category 4: Oversight

Oversight controls ensure that governance is measured, reported on, and accountable.

**Governance Dashboard**: A dedicated Grafana dashboard showing governance-specific metrics: confidence gate bypass count (target: 0), human review queue depth and SLA compliance, correction rate (fraction of human reviews that result in an override), audit trail completeness (all actions logged?), and GDPR deletion success rate.

**Weekly Governance Review**: A standing 30-minute meeting with governance officer, engineering lead, and at least one domain stakeholder. Agenda: review governance dashboard metrics, discuss any anomalies, review pending corrections and their feedback loop status, confirm human review queue SLA was met.

**Monthly Compliance Audit**: A structured audit of the audit trail, reviewing a sample of decisions for: correct confidence gate behaviour, appropriate escalations, correct human reviewer decisions, and no unauthorized authority expansions. The audit results are recorded in `/aether-iel/artifacts/{project}/governance/audit-{date}.md`.

**Quarterly IMM Assessment**: Using the Intelligence Maturity Model assessment template, evaluate whether the system has progressed in maturity. This is the input to Phase 11 (Evolution) planning. A system that has been operating for 3 months without progressing in maturity is either already at its target level (expected) or stagnating (requires investigation).

---

## Non-Negotiable Governance Controls

These five controls are non-negotiable. They cannot be disabled, overridden, or deferred:

1. **Confidence gate**: confidence < 0.8 → human review. `confidence_gate_bypass_total` is permanently monitored and alerts at any non-zero value.
2. **Audit trail**: every consequential agent action is logged. The log is append-only. No exceptions for performance.
3. **Hard delete on erasure**: GDPR Article 17 requests result in complete record deletion. No soft-delete.
4. **Tool permission enforcement**: agents cannot call tools outside their registry. Attempts are security events.
5. **Authority immutability**: agent authority levels cannot be changed at runtime through configuration. Changes require code deployment with a Phase 5 contract amendment.

---

## Phase Gate

Governance is an ongoing phase — there is no terminal gate. However, the following must be in place and verified within 30 days of Phase 8 deployment completion:

- [ ] Policy engine is running and enforcing authority rules
- [ ] Tool permission enforcement is verified (attempted unauthorized tool call produces security log event)
- [ ] Human review queue is operational with defined SLA
- [ ] Audit trail is writing correctly and stored in append-only storage
- [ ] Governance dashboard is operational with all required metrics
- [ ] Weekly governance review cadence is established with named participants
- [ ] Monthly compliance audit schedule is confirmed
- [ ] Correction feedback loop is connected to Phase 10 learning pipeline

| Role | Name | Signature | Date |
|---|---|---|---|
| Governance Officer | | | |
| Engineering Lead | | | |

---

## Inputs

- Intelligence scope (Phase 1)
- Agent Contracts and Tool Registry (Phase 5)
- Deployed system with observability (Phase 8)

## Outputs

- Running Policy Engine
- Human Review Queue (operational)
- Audit Trail (append-only log store)
- Governance Dashboard
- Scheduled audit and review cadence

## Feeds Into

Phase 10 (Learning) — correction signals from the human review queue feed the learning pipeline.
Phase 11 (Evolution) — quarterly IMM assessments inform evolution planning.
