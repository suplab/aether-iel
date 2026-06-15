# Phase 4 — Memory Engineering

> **AIEL Phase Group: Intelligence Design**
> Phase 4 designs the persistent memory system: schema, lifecycle rules, decay and reinforcement mechanics, and the data migration plan. Memory is what separates an intelligence system from a stateless LLM call.

---

## Purpose

Memory is the defining characteristic of intelligence systems at IMM Level 3 and above. Without memory, every session starts from zero. The system cannot learn from experience, cannot maintain context across interactions, and cannot build the kind of persistent understanding that makes intelligence systems genuinely useful over time.

Phase 4 treats memory as an engineering discipline. It is not sufficient to decide "we will store memories in a database." The memory system must specify: what is stored, at what granularity, for how long, under what decay function, with what reinforcement mechanics, and with what deletion guarantees. Each of these decisions has performance, privacy, and quality implications.

---

## Key Activities

**1. Memory Schema Design**

Design the database schema for each memory type identified in Phase 2. The core memory table structure for Aether systems is:

```sql
CREATE TABLE memories (
    id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id     UUID NOT NULL,
    type        VARCHAR(20) NOT NULL CHECK (type IN ('EPISODIC','SEMANTIC','PROCEDURAL','EMOTIONAL')),
    content     TEXT NOT NULL,
    embedding   VECTOR(384),
    strength    FLOAT NOT NULL DEFAULT 1.0 CHECK (strength BETWEEN 0.0 AND 1.0),
    access_count INTEGER NOT NULL DEFAULT 0,
    created_at  TIMESTAMPTZ NOT NULL DEFAULT NOW(),
    last_accessed TIMESTAMPTZ,
    metadata    JSONB DEFAULT '{}',
    tags        TEXT[]
);

CREATE INDEX ON memories USING ivfflat (embedding vector_cosine_ops) WITH (lists = 100);
```

For each memory type, document any type-specific fields, metadata schema, and indexing strategy. Emotional memories typically require sensitivity classification in metadata. Procedural memories typically require a `success_count` field for reinforcement. Episodic memories require a precise `occurred_at` timestamp distinct from `created_at`.

**2. Memory Lifecycle Definition**

Every memory progresses through seven stages: Formation (encoded from an interaction), Consolidation (strength normalized and embedding stored), Retrieval (accessed by similarity search or key lookup), Reinforcement (strength increased on retrieval), Decay (strength reduced over time on non-access), Purge evaluation (below threshold check), and Deletion (hard delete, no soft-delete — GDPR Article 17 compliance).

Document the transitions and the rules governing each. The lifecycle cannot be partially implemented — a system that forms and retrieves memories but does not decay or purge them accumulates noise over time.

**3. Reinforcement Mechanics**

Define the mathematical model for memory reinforcement. The Aether standard is additive reinforcement on retrieval:

```
strength = min(1.0, strength + 0.1)
access_count = access_count + 1
last_accessed = NOW()
```

This is the default. If your system requires asymmetric reinforcement (some memory types reinforce faster) or bounded reinforcement with diminishing returns, document the deviation and justify it. Never use unbounded reinforcement — memories should not become permanently immune to decay.

**4. Decay Mechanics**

Define the decay function. The Aether standard applies exponential decay on non-access:

```
strength = strength * 0.95  -- applied daily for memories not accessed in 24h
```

Memories below the purge threshold (default: 0.05) are candidates for deletion in the weekly purge sweep. Document: the decay rate, the decay interval, the purge threshold, and the purge schedule. For emotional memories, consider slower decay — affective context is often more durable than episodic detail.

The decay function must be implemented in code, not configuration. A configurable decay rate is a governance gap — operators should not be able to disable memory decay.

**5. GDPR Memory Architecture**

Design the complete data subject rights implementation for the memory system. This is not optional for any system processing personal data.

Right of access (Article 15): the system must be able to retrieve all memories for a given `user_id` in a readable format. Implement a `GET /v1/users/{id}/memories` endpoint that returns the full memory set.

Right of erasure (Article 17): the system must hard-delete all memories for a given `user_id` — no soft-delete, no archival flag. Implement a `DELETE /v1/users/{id}/memories` endpoint that removes all records and confirms deletion. This endpoint must also clear any derived data: cached embeddings, session summaries, and aggregated profiles.

Right to portability (Article 20): the system should be able to export a user's memories in a structured format (JSON). This is the same data as the access endpoint, formatted for machine readability.

Consent withdrawal must cascade to memory deletion. If the system's consent model is managed separately (e.g., in a consent service), the memory system must subscribe to consent withdrawal events and trigger deletion automatically.

**6. Retention Policy**

Define the maximum retention period for each memory type. Retention periods must be documented and enforceable — the system must be able to purge memories past their retention date without requiring manual intervention.

| Memory Type | Default Retention | Maximum Retention |
|---|---|---|
| EPISODIC | 90 days (strength-dependent) | 1 year |
| SEMANTIC | Until explicitly deleted | Indefinite |
| PROCEDURAL | Until superseded | Indefinite |
| EMOTIONAL | 180 days | 2 years |

These are defaults. Document the actual retention periods for your system and justify any deviation. Longer retention requires explicit justification and additional governance controls.

**7. Migration Plan**

If this system replaces or augments an existing system with data, design the migration plan: what data is migrated, how it is transformed into the new schema, how embeddings are generated for historical content, how conflicts are resolved, and how migration success is validated. A migration plan with no rollback strategy is not a plan.

---

## Primary Artifact: Memory Schema + Retention Policy

The Phase 4 artifacts are:

**Memory Schema** — complete SQL schema including all tables, indexes, constraints, and migration scripts (Flyway or Liquibase format).

**Retention Policy Document** — memory type, retention period, decay parameters, purge schedule, and GDPR deletion implementation for each type.

**GDPR Implementation Checklist** — confirmation that Articles 15, 17, and 20 are implemented and testable.

---

## Phase Gate

Before Phase 5 (Agent Engineering) begins, the following must be true:

- [ ] Memory schema is designed and SQL migrations are written
- [ ] Reinforcement mechanics are defined with mathematical model
- [ ] Decay function is defined with rate, interval, and purge threshold
- [ ] Retention periods are defined for all memory types
- [ ] GDPR Article 15 (access), 17 (erasure), and 20 (portability) implementations are designed
- [ ] Consent withdrawal cascade is designed
- [ ] Migration plan is complete (if applicable) with rollback strategy
- [ ] Memory Schema and Retention Policy artifacts are filed

| Role | Name | Signature | Date |
|---|---|---|---|
| Architect | | | |
| Data Steward / Privacy Officer | | | |

---

## Inputs

- Approved Cognitive Design (Phase 2)
- Knowledge Schema (Phase 3)
- GDPR/data residency requirements
- Existing data inventory (for migration planning)

## Outputs

- **Memory Schema** (SQL migrations)
- **Retention Policy** document
- **GDPR Implementation Checklist**
- Migration plan (if applicable)

## Feeds Into

Phase 6 (Implementation) — engineers implement the memory system from the schema and mechanics defined here.
Phase 7 (Evaluation) — memory retrieval accuracy and decay/reinforcement behaviour are validated against the defined parameters.
Phase 9 (Governance) — retention policies and GDPR controls are operationalized in the governance layer.
