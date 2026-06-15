---
name: aiel-patterns
description: Repeating patterns observed across intelligence engineering projects
type: reference
---

# AIEL Patterns

## Pattern 1: Brief Before Code

Every intelligence system starts with a written brief (Phase 1 artifact) before any code is written.
The brief defines: why it exists, what decisions it can make, what risks it introduces.

**Why:** Systems built without a brief consistently over-scope, under-govern, and drift from their original purpose.

## Pattern 2: Memory Schema First

Memory types, retention policies, and decay strategies are designed before implementation.
Retrofitting memory is expensive and often requires full re-embedding.

**Why:** The 384-dim vector schema is immutable after first migration. Memory type taxonomy affects all downstream agent queries.

## Pattern 3: Governance at Phase 1, Not Phase 9

Governance is not an audit step. It is a design constraint that shapes the system from day one.
Human-in-the-loop thresholds, audit requirements, and policy structures are documented in Phase 1.

**Why:** Adding governance to a running intelligence system is 10x harder than designing it in.

## Pattern 4: Evaluation Before Deployment

Evaluation Engineering (Phase 7) must complete before Deployment (Phase 8).
Key metrics: correctness, hallucination rate, memory recall quality, decision safety.

**Why:** Intelligence systems that skip evaluation enter production with unknown failure modes.

## Pattern 5: Confidence Gate Non-Negotiable

Agent blocking decisions with confidence < 0.8 must never auto-enforce.
This constraint is designed at Phase 5 (Agent Engineering) and implemented in Phase 6.

**Why:** Low-confidence blocks are the most common source of false positives in production agent systems.

## Pattern 6: Feedback Loop is Phase 1 Concern

How the system will learn from feedback is designed in Phase 1, not added in Phase 10.
The data model must support feedback ingestion from day one.

**Why:** Retrofitting a feedback loop requires schema migrations, Kafka topic creation, and agent refactoring.
