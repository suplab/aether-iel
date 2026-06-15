# Governance Controls Standard — AIEL Phase 9

> Applies to all intelligence systems at Intelligence Maturity Level 3 and above.
> Governance is not added after the fact — it is designed in Phase 1 and enforced from Phase 6.

---

## 1. Core Principle

Governance in Aether is not an audit function. It is a **design constraint**.

Every intelligence system must answer these questions **before code is written**:
- Who can instruct this system to act?
- What decisions can it make autonomously?
- What must always route to human review?
- Who can see what it decided and why?
- How is it stopped if it behaves incorrectly?

These answers belong in the Phase 1 Intelligence Brief and Phase 5 Agent Contracts.

---

## 2. Control Categories

### 2.1 Authority Controls

Define and enforce what the system is allowed to do.

| Control | Description | Implementation |
|---|---|---|
| Agent Contract | Explicit authority boundary per agent | Phase 5 artifact — signed by architect + governance officer |
| Decision Hierarchy | Ordered set of outcomes (BLOCK > RATE_LIMIT > ALERT > SUGGEST > DEFER > ALLOW) | Enforced in `Agent.execute()` return type |
| Confidence Gate | Decisions < 0.8 confidence must not be auto-enforced | `autoEnforced = false` when `confidence < 0.8` |
| Scope Boundary | System cannot act outside its defined domain | Validated on every `AgentInput` |
| Escalation Path | For each decision type, define who reviews low-confidence decisions | Documented in Intelligence Brief |

**Non-negotiable:** No agent may self-escalate its authority. An agent cannot change its own authority boundary at runtime.

### 2.2 Transparency Controls

Ensure all decisions are visible, explainable, and auditable.

| Control | Description | Implementation |
|---|---|---|
| Decision Audit Log | All agent decisions logged with: agent name, input hash, decision type, confidence, outcome, timestamp | Append-only audit table — no FK constraints |
| Explanation | Every BLOCK and ALERT decision includes a human-readable reason | Required field in `AgentOutput` |
| PII Redaction | PII removed before any audit log write | Regex patterns: email, phone, CC, SSN, JWT, API key |
| Trace Context | Distributed trace ID attached to every decision | OpenTelemetry trace propagation |
| Immutability | Audit log entries cannot be modified or deleted | DB constraint: no UPDATE/DELETE on audit table |

### 2.3 Correction Controls

Ensure the system can be corrected when it fails.

| Control | Description | Implementation |
|---|---|---|
| Human Override | Any auto-enforced decision can be reversed by an authorised human | Override endpoint with audit trail |
| Kill Switch | System can be stopped without code deployment | `@ConditionalOnProperty` or feature flag |
| Feedback Loop | Decision outcomes feed back to improve confidence calibration | `CORRECT` / `INCORRECT` / `PARTIALLY_CORRECT` signal |
| Memory Erasure | Any user's data can be fully erased (GDPR Article 17) | Hard-delete: memories + sessions + consent |
| Rollback | Any model or agent change can be reverted within 15 minutes | Feature flags + versioned model registry |

### 2.4 Oversight Controls

Ensure humans remain in control of the system's evolution.

| Control | Description | Implementation |
|---|---|---|
| Maturity Gate | System cannot advance to higher IMM level without governance approval | IMM assessment + governance sign-off |
| Evolution Review | Every Phase 11 evolution requires a Governance Officer review | Evolution Strategy artifact + approval |
| Drift Detection | Model or agent behaviour drift triggers alert, not auto-correction | Prometheus alert rule on decision distribution |
| Human-in-the-Loop Threshold | Configurable % of decisions that must route to humans | `aether.governance.human-review-rate-min` |

---

## 3. Phase-by-Phase Governance Requirements

### Phase 1 — Strategy Engineering

- [ ] Governance requirements section completed in Intelligence Brief
- [ ] Human-in-the-loop threshold defined
- [ ] Escalation path named for each decision type
- [ ] Data residency requirements documented
- [ ] GDPR applicability determined

### Phase 5 — Agent Engineering

- [ ] Agent Contract signed by architect and governance officer
- [ ] Confidence gate threshold confirmed (0.80 minimum)
- [ ] Decision hierarchy documented per agent
- [ ] Tool access list is explicit (no wildcard permissions)
- [ ] Authority boundary cannot be exceeded at runtime

### Phase 6 — Implementation Engineering

- [ ] Confidence gate is in code, not in configuration (cannot be feature-flagged off)
- [ ] Audit log writes are synchronous — no fire-and-forget
- [ ] PII redaction applied before audit log write
- [ ] Kill switch implemented and tested
- [ ] Human override endpoint implemented and access-controlled

### Phase 7 — Evaluation Engineering

- [ ] Governance compliance section of Evaluation Report is complete
- [ ] Confidence gate is 100% enforced across all test cases
- [ ] Audit log completeness verified
- [ ] GDPR erasure tested for all memory types

### Phase 9 — Governance Engineering

- [ ] Governance Officer assigned and documented
- [ ] Policy document published and version-controlled
- [ ] Audit log reviewed — no gaps
- [ ] Override log reviewed — all overrides have justification
- [ ] Drift baseline established

### Phase 10 — Learning Engineering

- [ ] Feedback loop signal volume is sufficient for calibration
- [ ] Learning changes require governance review if they alter decision distribution
- [ ] No autonomous model updates in production without approval

### Phase 11 — Evolution Engineering

- [ ] Governance Officer must approve Evolution Strategy before implementation
- [ ] Agent Contract re-signed if authority boundaries change
- [ ] Evaluation Report (Phase 7) must be re-run after evolution

---

## 4. Non-Negotiable Controls

These controls **cannot** be disabled, overridden, or bypassed under any circumstance:

| Control | Rule |
|---|---|
| Confidence Gate | `confidence < 0.8` → `autoEnforced = false`. Always. |
| GDPR Erasure | Hard-delete of all user data on verified erasure request. No soft-delete. |
| Audit Log Immutability | No UPDATE or DELETE on audit log records. Append-only. |
| PII in Logs | Zero tolerance. PII in application logs is a P1 incident. |
| Agent Authority Self-Escalation | Agents cannot grant themselves additional authority at runtime. |

---

## 5. Governance Roles

| Role | Responsibilities |
|---|---|
| Governance Officer | Owns governance policy, holds veto power on Phase 7 sign-off, approves all evolution strategies |
| Architect | Responsible for technical enforcement of governance controls in code |
| Product Owner | Responsible for escalation path definitions in Intelligence Brief |
| Operations | Responsible for drift monitoring and human review queue SLA |

**Governance Officer veto is final.** It cannot be overridden by the team. Escalation goes to programme leadership, not back to the team.

---

## 6. Governance Audit Schedule

| Activity | Frequency | Owner |
|---|---|---|
| Audit log review | Weekly | Governance Officer |
| Override log review | Weekly | Governance Officer |
| Confidence distribution review | Monthly | Architect + Governance Officer |
| Full governance audit | Quarterly | Governance Officer |
| Post-evolution governance review | After every evolution | Governance Officer |
| GDPR compliance review | Annually + on any data schema change | Governance Officer |
