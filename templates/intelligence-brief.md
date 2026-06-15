# Intelligence Brief Template

> AIEL Phase 1 — Strategy Engineering artifact.
> Complete this before any code, design, or infrastructure work begins.

---

## System Identity

| Field | Value |
|---|---|
| Name | |
| One-line description | |
| Domain | (e.g. insurance, healthcare, developer productivity) |
| Owner | |
| Date | |
| Version | 1.0 |

---

## Why This Intelligence Exists

_What problem does this intelligence solve that software alone cannot?_

---

## What Decisions Can It Make?

_List every decision the system can make autonomously. Be specific._

| Decision | Authority Level | Auto-enforce? |
|---|---|---|
| | | |

**Authority Level:** `SUGGEST` (recommendation only) · `ALERT` (flag for human) · `BLOCK` (prevent action, human review required)

---

## What It Cannot Do

_Explicit boundaries. What is out of scope or forbidden?_

- It must not…
- It cannot access…
- It will never autonomously…

---

## Memory Requirements

| Memory Type | What to Remember | Retention | Decay |
|---|---|---|---|
| Episodic | Specific events | 365 days | 5%/week idle |
| Semantic | Facts and knowledge | Indefinite | 2%/week idle |
| Procedural | How-to knowledge | Indefinite | 1%/week idle |
| Emotional | Emotional states | 90 days | 10%/week idle |

---

## Agent Topology

_Which agents will exist in this system?_

| Agent | Capability | Authority | Confidence Threshold |
|---|---|---|---|
| | | | 0.8 |

---

## Governance Requirements

- Human-in-the-loop threshold: **confidence < 0.8**
- Audit trail: ☐ Required / ☐ Not required
- GDPR applicability: ☐ Yes / ☐ No
- PII handling: _describe approach_
- Escalation path: _who reviews low-confidence decisions?_

---

## Success Metrics

| Metric | Target | Measurement |
|---|---|---|
| Decision accuracy | > 90% | Feedback loop |
| Hallucination rate | < 2% | Evaluation pipeline |
| Memory recall quality | > 85% relevance | Cosine similarity audit |
| Latency (p99) | < 2s | Prometheus |

---

## Risk Register

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Hallucination in high-stakes decision | Medium | High | Confidence gate + human review |
| Memory contamination (cross-user) | Low | Critical | user_id in all SQL WHERE clauses |
| LLM provider unavailability | Medium | Medium | Multi-provider fallback via LlmClient |

---

## AIEL Phases Applicable

☐ Phase 1 — Strategy (this document)
☐ Phase 2 — Cognitive Architecture
☐ Phase 3 — Knowledge Engineering
☐ Phase 4 — Memory Engineering
☐ Phase 5 — Agent Engineering
☐ Phase 6 — Implementation
☐ Phase 7 — Evaluation
☐ Phase 8 — Deployment
☐ Phase 9 — Governance
☐ Phase 10 — Learning
☐ Phase 11 — Evolution
☐ Phase 12 — Retirement

---

## Sign-off

| Role | Name | Date | Signature |
|---|---|---|---|
| Intelligence Owner | | | |
| Architecture Review | | | |
| Governance/Compliance | | | |
