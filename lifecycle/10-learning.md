# Phase 10 — Learning

> **AIEL Phase Group: Intelligence Operations**
> Phase 10 closes the feedback loop. It captures signals from production — human review decisions, user feedback, outcome data — and uses them to improve the system's accuracy over time. Without a learning loop, an intelligence system is static.

---

## Purpose

The difference between an intelligence system and a fixed ML model is that intelligence systems improve through use. Phase 10 builds the infrastructure that makes this improvement systematic, measurable, and governed.

Learning without governance is dangerous. A system that updates itself in response to any feedback signal is vulnerable to adversarial manipulation — users who discover they can influence agent behaviour by submitting particular inputs. Phase 10 designs the learning pipeline with the same governance discipline as the rest of the system: structured signals, human oversight, and measurable criteria for what counts as an improvement.

---

## Learning Signal Types

**Correction Signals** — the highest-quality learning signal. When a human reviewer overrides an agent decision in the Phase 9 review queue, the override is a direct statement that the agent's reasoning was wrong. Correction signals contain: the original input, the agent's reasoning (if logged), the agent's proposed action with confidence score, and the human reviewer's correct action.

Correction signals feed directly into the evaluation dataset for the next evolution cycle. A system with a high correction rate (> 20% of reviewed decisions result in an override) is underperforming on decision accuracy and may need a Phase 11 Evolution cycle sooner than planned.

**User Feedback Signals** — explicit signals from users about the quality of system outputs. These are lower quality than correction signals because they reflect user satisfaction rather than correctness. They are valuable for identifying patterns (users consistently express dissatisfaction with a particular response type) but cannot be used directly to update agent logic.

**Outcome Signals** — signals derived from what happened after the system made a decision. If the system recommended an action and the outcome was positive (user adopted the recommendation, the blocked request was confirmed malicious), that is a positive outcome signal. If the outcome was negative, it is a negative signal. Outcome signals are often delayed and require an outcome tracking mechanism — connecting system decisions to downstream events.

**Memory Access Patterns** — which memories are retrieved frequently (high access count, high reinforce-on-read activity) and which are rarely accessed (low strength, approaching purge threshold). This signal identifies knowledge gaps (important topics have no strong memories) and knowledge staleness (frequently needed memories are decaying, suggesting the knowledge base is not refreshed frequently enough).

---

## Feedback Pipeline Architecture

The feedback pipeline has four stages:

**Stage 1: Signal Collection** — capture all learning signals in a structured format and store them in a dedicated feedback store (separate from the memory system). Each signal record must contain: signal type, timestamp, source (agent name, user ID if consented, system process), signal content, and quality rating (high for correction signals, medium for outcome signals, low for satisfaction signals).

**Stage 2: Signal Validation** — filter out signals that are invalid, adversarial, or too sparse to be meaningful. Validation rules: correction signals from reviewers with fewer than N reviews (new reviewers may be miscalibrated), outcome signals for decisions that were not recorded in the audit trail (orphaned signals — cannot be traced to a decision), satisfaction signals that are outliers relative to the aggregate pattern.

**Stage 3: Batch Aggregation** — aggregate validated signals over a defined period (default: weekly). Produce a summary report: total signals by type, correction rate by agent, outcome signal ratio by decision type, knowledge gap indicators from memory access patterns. This summary is the input to the learning review.

**Stage 4: Learning Action** — based on the aggregated signals, determine what learning action (if any) to take. Learning actions fall into three categories: evaluation dataset update (adding correction examples to the dataset for the next Phase 7 run), knowledge base update (adding or refreshing knowledge units to address identified gaps), and evolution trigger (if the correction rate exceeds threshold, initiate a Phase 11 Evolution cycle). All learning actions require review before execution — no automated updates to production system logic.

---

## Learning Rate Monitoring

Define the target learning rate: the rate at which the system's decision accuracy improves over time, measured from the evaluation dataset. At a minimum, measure accuracy against the Phase 7 evaluation dataset monthly. A system that is not improving (or is regressing) despite a healthy volume of correction signals has a broken learning pipeline.

Monitor the following metrics:

| Metric | Target | Action if Missed |
|---|---|---|
| Correction signal capture rate | 100% of human review decisions | Debug feedback pipeline connection to review queue |
| Signal validation pass rate | > 80% of signals pass validation | Investigate adversarial signal patterns |
| Evaluation dataset growth | Grows monthly from correction signals | Check signal-to-dataset pipeline |
| Accuracy trend (monthly) | Non-decreasing | Trigger Phase 11 Evolution if declining 2+ months |

---

## Governing the Learning Loop

**No automated model updates in production.** Learning signals update the evaluation dataset and the knowledge base. They do not directly update agent logic, prompt templates, or confidence thresholds. Changes to these require a Phase 11 Evolution cycle with full evaluation.

**Consent for feedback data.** If user feedback signals contain personal data (user-authored text), the feedback must be collected under explicit consent. The consent model must specify that user inputs may be used to improve the system, and users must be able to withdraw this consent independently of their memory consent.

**Feedback data is sensitive.** The feedback store contains agent reasoning traces, reviewer decisions, and potentially user content. It must be protected with the same access controls as the memory system. Feedback data is subject to GDPR data subject access requests.

---

## Phase Gate

Learning is an ongoing phase. The following must be in place and measured within 60 days of Phase 8 deployment:

- [ ] Signal collection pipeline is running for all three signal types (correction, feedback, outcome)
- [ ] Signal validation rules are implemented and tested
- [ ] Weekly batch aggregation is scheduled and producing summary reports
- [ ] Learning rate baseline is established from Phase 7 evaluation results
- [ ] Monthly accuracy measurement is scheduled
- [ ] Learning actions require human review before execution (no automated production updates)
- [ ] Feedback data consent model is confirmed with data steward
- [ ] `correction_signal_capture_rate` is confirmed at 100%

| Role | Name | Signature | Date |
|---|---|---|---|
| Engineering Lead | | | |
| Governance Officer | | | |

---

## Inputs

- Human review queue correction decisions (Phase 9)
- Outcome data from downstream systems (if available)
- Memory access pattern data from the memory system
- Phase 7 evaluation dataset (baseline for accuracy tracking)

## Outputs

- Growing evaluation dataset (correction signals added monthly)
- Weekly signal aggregation reports
- Monthly accuracy trend report
- Evolution trigger recommendations (when threshold is exceeded)

## Feeds Into

Phase 11 (Evolution) — when the learning pipeline signals that accuracy is declining or correction rate is too high, Phase 11 is triggered to address structural improvements.
