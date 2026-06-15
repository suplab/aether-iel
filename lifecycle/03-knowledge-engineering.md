# Phase 3 — Knowledge Engineering

> **AIEL Phase Group: Intelligence Design**
> Phase 3 defines what the system knows and how it retrieves that knowledge. It produces the knowledge schema, the RAG pipeline configuration, and the ingestion and refresh strategy. Without structured knowledge, agents are guessing.

---

## Purpose

An intelligence system without curated knowledge is an LLM with no grounding. It will hallucinate, confuse domains, and produce outputs that are confident but wrong. Phase 3 prevents this by treating knowledge as a first-class engineering artifact: designed with a schema, ingested with a pipeline, evaluated for quality, and updated on a defined schedule.

Knowledge engineering is distinct from data engineering. Data pipelines move bytes. Knowledge pipelines transform raw information into retrievable, attributable, semantically indexed units that agents can reason over.

---

## Key Activities

**1. Knowledge Inventory**

Before any schema is designed, conduct a knowledge inventory: What does the system need to know to solve the problem defined in Phase 1? What information sources exist? What is the quality and currency of each source? What is missing?

Organize the inventory by knowledge type. Declarative knowledge (facts, policies, domain rules) is stable and high-value for semantic memory. Procedural knowledge (workflows, decision trees, how-to guides) feeds procedural memory. Historical knowledge (past decisions, outcomes, cases) feeds episodic memory and is particularly valuable for systems that must learn from experience.

For each source, record: format, update frequency, access method, trust level (authoritative vs. secondary vs. user-provided), and any PII or sensitivity classification. Sources with PII require explicit handling decisions before ingestion begins.

**2. Knowledge Schema Design**

Design the schema for the system's knowledge units. A knowledge unit is the smallest retrievable piece of information. Each unit should have: a unique identifier, a content field (the retrievable text), a vector embedding field (384 dimensions for all-MiniLM-L6-v2), metadata fields (source, created_at, updated_at, trust_level, domain_tags), and a strength or confidence field if the system will learn from usage.

Schema decisions made in Phase 3 are expensive to change after ingestion begins. Design for the retrieval patterns identified in Phase 2, not for storage convenience.

**3. Retrieval Pipeline Design**

Define how agents retrieve relevant knowledge given a query. The retrieval pipeline has three stages: query encoding (embed the incoming query using the same model used for indexing), candidate retrieval (find top-K nearest neighbours by cosine similarity from the vector index), and reranking (apply a secondary filter — by domain, recency, trust level, or cross-encoder score — before returning results to the agent).

The number of candidates (K) retrieved before reranking, the reranking strategy, and the minimum similarity threshold for inclusion are all system parameters that must be set here and validated in Phase 7. Default K=10 with a minimum cosine similarity of 0.7 is a reasonable starting point, but domain-specific calibration is required.

**4. Ingestion Pipeline Design**

Define how raw sources become indexed knowledge units. The ingestion pipeline includes: document loading (handling PDF, HTML, markdown, database records), chunking (splitting documents into units that are semantically coherent and fit within the embedding model's context window), embedding generation, metadata enrichment, deduplication, and storage.

Chunking strategy has outsized impact on retrieval quality. Chunks that are too small lose context; chunks that are too large dilute relevance signals. For most text sources, 200–500 token chunks with 50-token overlap perform well. Structured data (tables, JSON records) should not be chunked the same way as prose.

**5. Knowledge Quality Standards**

Define the minimum quality bar for ingested knowledge. At minimum: every unit must be attributable to a named source; units below a minimum length threshold should be filtered (isolated headings, navigation elements, and fragments are noise); units containing PII must be flagged and handled per the GDPR policy established in Phase 1; and stale units must be identifiable and purgeable by source and date.

Establish a retrieval quality evaluation process that will be used in Phase 7: a set of test queries with expected retrieval results (documents, not just topics) so that precision@K can be measured against the ecosystem minimum of ≥ 0.85.

**6. Knowledge Refresh Strategy**

Static knowledge bases degrade. Sources change, facts become outdated, and new knowledge becomes relevant. Define: how often each source is re-ingested, how updated units replace stale ones (upsert by ID, or delete-and-reinsert), how deletions in the source propagate to the index, and who is responsible for monitoring knowledge freshness.

For sources with GDPR implications, the refresh strategy must handle right-to-erasure requests: if a user's personal data was ingested as knowledge, it must be findable and erasable by user ID. Design this into the schema and ingestion pipeline now.

---

## Primary Artifact: Knowledge Schema + RAG Configuration

The Phase 3 artifacts are:

**Knowledge Schema** — the entity-relationship diagram and SQL/JSON schema for all knowledge tables, vector indexes, and metadata fields.

**RAG Configuration** — documented retrieval parameters: embedding model, chunk size and overlap, K for candidate retrieval, reranking strategy, minimum similarity threshold, and the test query set used for Phase 7 evaluation.

**Ingestion Pipeline Specification** — documented source list, chunking strategy per source type, PII handling, deduplication logic, and refresh schedule per source.

---

## Phase Gate

Before Phase 4 (Memory Engineering) begins, the following must be true:

- [ ] Knowledge inventory is complete with source list, trust levels, and sensitivity classification
- [ ] Knowledge schema is designed and documented
- [ ] Retrieval pipeline is specified with all parameters documented
- [ ] Ingestion pipeline is specified with chunking strategy and PII handling
- [ ] Knowledge refresh schedule is defined per source
- [ ] GDPR deletion path for personal data in the knowledge base is designed
- [ ] Test query set for Phase 7 precision@K evaluation is created
- [ ] Knowledge Schema and RAG Configuration artifacts are filed

| Role | Name | Signature | Date |
|---|---|---|---|
| Architect | | | |
| Data Steward | | | |

---

## Inputs

- Approved Cognitive Design (Phase 2)
- Knowledge inventory (raw sources, access credentials, format documentation)
- GDPR/compliance requirements for each source

## Outputs

- **Knowledge Schema** (SQL schema + vector index configuration)
- **RAG Configuration** (retrieval parameters + test query set)
- **Ingestion Pipeline Specification**
- Updated risk register

## Feeds Into

Phase 4 (Memory Engineering) — memory stores extend the knowledge schema with time-based and user-specific dimensions.
Phase 6 (Implementation) — engineers build the ingestion pipeline and retrieval layer from the specification produced here.
Phase 7 (Evaluation) — the test query set is used to measure retrieval precision@K.
