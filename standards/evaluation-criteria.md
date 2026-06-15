# Evaluation Criteria Standard — AIEL Phase 7

> Applies to all intelligence systems built on the Aether ecosystem.
> These criteria are minimum thresholds. Teams may set stricter targets.

---

## 1. When Evaluation Applies

Evaluation (Phase 7) is **mandatory** before:
- Any production deployment
- Any significant model change (LLM swap, embedding change)
- Any agent authority boundary change
- Any memory schema migration
- Completion of each evolution cycle (Phase 11)

Evaluation is **not** a one-time gate. It is a repeating discipline.

---

## 2. Functional Accuracy Criteria

### 2.1 Memory Retrieval

| Metric | Minimum Threshold | Measurement Method |
|---|---|---|
| Precision @K (K=5) | ≥ 0.85 | Ground-truth labelled test set |
| Recall @K (K=5) | ≥ 0.80 | Ground-truth labelled test set |
| Mean cosine similarity | ≥ 0.75 | Average over correct retrievals |
| False positive rate | ≤ 0.10 | Irrelevant memories returned / total returned |

### 2.2 Agent Decision Quality

| Metric | Minimum Threshold | Notes |
|---|---|---|
| Decision accuracy | ≥ 0.85 | vs. human-reviewed ground truth |
| Confidence calibration (Brier score) | ≤ 0.15 | Lower is better |
| False BLOCK rate | ≤ 0.02 | Over-blocking harms user trust |
| False ALLOW rate | ≤ 0.05 | Under-blocking is a security risk |
| Low-confidence routing | 100% | All decisions with confidence < 0.8 must reach human review |

### 2.3 Response Quality

| Metric | Minimum Threshold | Notes |
|---|---|---|
| Factual accuracy | ≥ 0.90 | Human-rated random sample (n ≥ 50) |
| Relevance | ≥ 0.85 | Human-rated random sample |
| Hallucination rate | ≤ 0.05 | Measured against verifiable facts |
| JSON parse success | ≥ 0.99 | Structured output compliance |

---

## 3. Performance Criteria

| Metric | Minimum Threshold | Hard Limit |
|---|---|---|
| p50 end-to-end latency | ≤ 500ms | 1000ms |
| p95 end-to-end latency | ≤ 1500ms | 2500ms |
| p99 end-to-end latency | ≤ 2000ms | 3000ms |
| Memory retrieval p99 | ≤ 200ms | 500ms |
| Error rate (5xx) | ≤ 0.5% | 1.0% |
| Availability | ≥ 99.5% | 99.0% |

Exceeding a **Hard Limit** is an automatic evaluation REJECTION regardless of other scores.

---

## 4. Compliance Criteria

All compliance criteria are **binary** — pass or fail. A single fail is an automatic REJECTION.

| Criterion | Standard | Verification |
|---|---|---|
| GDPR right to erasure | All user data (memories, sessions, consent) hard-deleted on request | Integration test with Testcontainers |
| PII in logs | Zero PII in application logs | Log audit against PII regex patterns |
| Consent before write | Memory write blocked if consent not given | Unit test + integration test |
| Confidence gate | 100% of decisions < 0.8 confidence routed to human review | Decision audit log analysis |
| Agent authority | No agent executes outside its defined authority boundary | Agent contract compliance test |
| Audit trail | All BLOCK, RATE_LIMIT, and ALERT decisions logged | Audit log completeness check |

---

## 5. Code Quality Gate

Required before evaluation report can be marked APPROVED:

| Criterion | Threshold |
|---|---|
| JaCoCo line coverage | ≥ 80% |
| JaCoCo branch coverage | ≥ 70% |
| OWASP CVE scan | No CVSS ≥ 9.0 unresolved |
| Conventional commit compliance | 100% |
| Static analysis (SpotBugs / Checkstyle) | Zero blocker/critical findings |

---

## 6. Regression Criteria (Evolution Cycles)

When re-evaluating after a change:

| Metric | Maximum Allowed Regression |
|---|---|
| Decision accuracy | -0.02 (no more than 2 percentage points) |
| p99 latency | +200ms |
| Error rate | +0.1% |
| Confidence calibration (Brier) | +0.02 |
| Memory precision | -0.03 |

Any metric regressing beyond its threshold is an automatic REJECTION.

---

## 7. Test Dataset Requirements

| Requirement | Minimum |
|---|---|
| Total test cases | 200 |
| Ground-truth labelled cases (for accuracy metrics) | 50 |
| Human-rated cases (for quality metrics) | 50 |
| Failure scenario cases (LLM unavailable, DB down, etc.) | 10 |
| GDPR erasure test cases | 5 per MemoryType (20 total) |

Test datasets must not contain production PII. Use synthetic data or anonymised production replay.

---

## 8. Evaluation Authority

| Role | Responsibility |
|---|---|
| Lead Evaluator | Runs evaluation, owns the report |
| Architect | Reviews accuracy and performance findings |
| Governance Officer | Approves compliance section — veto power |
| Product Owner | Signs off on production readiness decision |

The Governance Officer holds veto power on any evaluation. A governance veto cannot be overridden by the team — it must be escalated to programme leadership.

---

## 9. Evaluation Output

A completed evaluation produces:
1. An **Evaluation Report** (Phase 7 template) with all sections completed
2. A **production readiness decision**: APPROVED / CONDITIONAL / REJECTED
3. If CONDITIONAL: a documented list of mitigations with owners and due dates
4. If REJECTED: a documented list of failures with owners and return-to-phase decision
5. A **sign-off record** with evaluator, architect, and governance officer signatures
