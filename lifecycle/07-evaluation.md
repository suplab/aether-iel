# Phase 7 — Evaluation

> **AIEL Phase Group: Intelligence Construction**
> Phase 7 validates the system against every threshold defined in Phases 1–5. Evaluation is not QA. It measures intelligence quality, not software correctness. No system proceeds to deployment without a signed Evaluation Report.

---

## Purpose

Software testing asks: "Does the code do what it's supposed to do?" Intelligence evaluation asks: "Does the system reason correctly, remember accurately, decide reliably, and govern safely?"

These are fundamentally different questions. A system can pass all unit and integration tests and still fail evaluation — because its retrieval precision is below threshold, its agent decisions are too often wrong, or its confidence gate is routing too many decisions to human review (indicating the confidence calibration is misconfigured).

Phase 7 produces the **Evaluation Report** — the evidence-based case that the system is ready for production. Without it, Phase 8 (Deployment) does not begin.

---

## Evaluation Dimensions

**1. Functional Accuracy**

Measure the system's core intelligence capabilities against the thresholds defined in Phase 1 success criteria and Phase 3/4 specifications.

*Memory Retrieval Precision@K*: Using the test query set created in Phase 3, measure what fraction of the top-K retrieved memories or knowledge units are relevant. The ecosystem minimum is precision@5 ≥ 0.85. Below this threshold, the retrieval pipeline (K, similarity threshold, reranking) must be tuned before deployment.

*Agent Decision Accuracy*: Using a labeled evaluation set of agent inputs with known correct outputs, measure the fraction of decisions that match the expected outcome. The ecosystem minimum is ≥ 0.85. The evaluation set must cover all agent types and include adversarial inputs (low-confidence cases, edge cases, inputs near decision boundaries).

*Confidence Calibration*: Measure how well the agent's confidence scores predict actual accuracy. A confidence score of 0.85 should correspond to approximately 85% accuracy on that decision type. Significant miscalibration (e.g., decisions scored 0.9 with 60% accuracy) indicates the confidence model needs retraining or recalibration.

*Low-Confidence Routing Rate*: 100% of decisions with confidence < 0.8 must route to the human review queue. Test this by injecting known low-confidence inputs and verifying they appear in the queue — never as auto-enforced actions.

**2. Performance**

Measure latency under realistic load. The ecosystem hard limit is p99 ≤ 2000ms end-to-end. This limit applies to the complete request-response cycle, not individual component measurements.

Measure: p50, p95, and p99 latency under single-user load, then under expected concurrent load. Identify the latency breakdown by component (embedding, retrieval, agent execution, LLM call, response assembly). If p99 is over threshold, the fix must be in the architecture (caching, async processing, query optimization) not in relaxing the threshold.

**3. Reliability**

*Availability*: The system must meet its availability target under simulated failure conditions. Test: vector index unavailable, LLM API rate limited, database connection pool exhausted, embedding service down. In all cases, the system must degrade gracefully — returning informative errors, not stack traces or silent failures.

*Memory Decay Correctness*: Run the decay job and verify that memory strength values update correctly according to the decay function. Verify that memories below the purge threshold are correctly identified and deleted.

*Reinforce-on-Read Correctness*: Retrieve the same memory multiple times and verify that strength increases correctly, is capped at 1.0, and that `access_count` and `last_accessed` update correctly.

**4. Governance and Compliance**

*Confidence Gate*: Verify that no action with confidence < 0.8 is auto-enforced. This is tested programmatically — inject inputs designed to produce sub-threshold confidence and verify the human review queue receives them.

*GDPR Compliance*:
- Article 15 (access): call `GET /v1/users/{id}/memories` for a test user — verify all memories are returned.
- Article 17 (erasure): call `DELETE /v1/users/{id}/memories` — verify all memory records, embeddings, and derived data are removed. Query the database directly to confirm no records remain.
- Article 20 (portability): verify exported data is in a machine-readable format with all required fields.
- Consent withdrawal: trigger a consent withdrawal event and verify the cascade deletes the user's memories.

*Security*: Run OWASP dependency check. No CVSS High (≥ 7.0) or Critical (≥ 9.0) CVEs in the production dependency tree. SQL injection protection: all database queries use parameterized statements — verified by code review, not just automated scan.

*Code Coverage*: JaCoCo report must show ≥ 80% line and branch coverage for domain and agent modules. Coverage below threshold requires additional tests before evaluation closes.

---

## Evaluation Dataset Requirements

The evaluation dataset must be prepared before Phase 7 begins, not during it. It must include:

- A labeled memory retrieval set: queries with expected relevant memories (minimum 100 query-document pairs)
- A labeled agent decision set: inputs with expected outputs and confidence ranges (minimum 50 per agent type)
- Adversarial examples: inputs designed to trigger low-confidence routing (minimum 20 per agent type)
- GDPR test data: a test user with known memory records, for deletion and access testing

The evaluation dataset must not overlap with the training or development data used during Phase 6. If the system has been tuned against the same dataset used for evaluation, the results are not valid.

---

## Production Readiness Gate

| Criterion | Minimum Threshold | Result |
|---|---|---|
| Memory retrieval precision@5 | ≥ 0.85 | PASS / FAIL |
| Agent decision accuracy | ≥ 0.85 | PASS / FAIL |
| Low-confidence routing rate | 100% | PASS / FAIL |
| p99 latency (end-to-end) | ≤ 2000ms | PASS / FAIL |
| p99 latency (under load) | ≤ 2000ms | PASS / FAIL |
| GDPR Article 17 deletion | Complete, verified | PASS / FAIL |
| GDPR Article 15 access | Complete | PASS / FAIL |
| OWASP CVE scan | No High/Critical | PASS / FAIL |
| Code coverage (domain + agent) | ≥ 80% | PASS / FAIL |
| Graceful degradation | All failure modes tested | PASS / FAIL |

All criteria must be PASS. A CONDITIONAL pass (some criteria FAIL with a remediation plan) requires governance officer sign-off and a re-evaluation date commitment. REJECTED means Phase 6 re-opens.

---

## Phase Gate

Before Phase 8 (Deployment) begins:

- [ ] Evaluation dataset is validated as non-overlapping with development data
- [ ] All functional accuracy metrics are at or above threshold
- [ ] p99 latency is at or below 2000ms under expected load
- [ ] All governance and compliance checks are PASS
- [ ] Evaluation Report is complete and filed
- [ ] All FAIL criteria have remediation plans with owners and dates (if CONDITIONAL)
- [ ] Evaluation Report is signed by architect, governance officer, and evaluator

| Role | Decision | Signature | Date |
|---|---|---|---|
| Architect | APPROVED / CONDITIONAL / REJECTED | | |
| Governance Officer | APPROVED / CONDITIONAL / REJECTED | | |
| Evaluator | APPROVED / CONDITIONAL / REJECTED | | |

---

## Inputs

- Working system from Phase 6
- Evaluation criteria from Phase 1 success criteria and ecosystem standards
- Test query set from Phase 3
- Agent Contracts from Phase 5 (defines expected decision accuracy)

## Outputs

- **Evaluation Report** (use template at `/aether-iel/templates/evaluation-report.md`)
- Evaluation dataset (versioned, stored in `/aether-iel/artifacts/{project}/evaluation/`)
- Updated risk register

## Feeds Into

Phase 8 (Deployment) — only a PASS or CONDITIONAL Evaluation Report authorizes deployment.
Phase 11 (Evolution) — evaluation results form the baseline against which evolution-cycle regression is measured.
