# Phase 6 — Implementation

> **AIEL Phase Group: Intelligence Construction**
> Phase 6 builds the system from the approved design artifacts. Code follows contracts; contracts do not follow code. Every significant implementation decision that deviates from design must be recorded as an ADR.

---

## Purpose

Phase 6 is where design becomes working software. The defining discipline of this phase is that implementation follows the artifacts produced in Phases 1–5 — it does not improvise them. When an implementation decision conflicts with a design decision, the implementation stops, the conflict is surfaced, and the design is either confirmed or formally revised. Code that silently diverges from the Cognitive Design, Agent Contracts, or Memory Schema creates a governance debt that compounds until it breaks.

---

## Implementation Sequence

The order of implementation matters. Dependencies between components must be respected to avoid building layers that cannot be tested independently.

**Stage 1: Foundation** — Database schema, migration scripts, and vector index configuration. This is the first thing built because everything else depends on it. Run all Flyway migrations. Verify the pgvector extension and ivfflat index can handle the embedding dimensions specified in Phase 2. No application code until this layer is stable.

**Stage 2: Domain Layer** — Core domain entities, port interfaces, and domain service implementations with no infrastructure dependencies. The hexagonal architecture rule: the domain layer must compile and pass unit tests with no Spring, no database, no HTTP. If domain logic requires a database call, there is a missing port interface.

**Stage 3: Memory System** — Memory repository implementations (adapters), embedding service integration, retrieval pipeline, and the decay/reinforcement scheduler. Build against the schema designed in Phase 4. Verify that: similarity search returns correct results, reinforce-on-read increments strength correctly, the decay job runs on schedule, and the GDPR deletion endpoint removes all records for a user.

**Stage 4: Knowledge Pipeline** — Ingestion pipeline (document loading, chunking, embedding, storage), retrieval layer, and reranking logic. Build against the RAG configuration from Phase 3. Run the test query set from Phase 3 to get an early precision@K reading — do not wait until Phase 7 to discover retrieval quality problems.

**Stage 5: Agent Implementations** — One agent at a time, in dependency order. Each agent is implemented from its Agent Contract. The contract is the specification; the implementation is not complete until it satisfies every clause. Implement the confidence gate in the agent runtime first — before any agent goes live, the gate must be in place.

**Stage 6: API Layer** — REST controllers, request/response DTOs, OpenAPI spec, and security configuration. Build against the API contract documented in the Cognitive Design. No undocumented endpoints.

**Stage 7: Integration** — Wire all components together. Run the full integration test suite. Verify end-to-end flows for each agent interaction pattern.

---

## Non-Negotiable Implementation Rules

**Confidence gate is code, not configuration.** The `autoEnforced = false` rule for confidence < 0.8 is enforced in the agent runtime class, not in a properties file or database table. Operators cannot disable it by changing configuration. If the threshold needs to change, it requires a code review, an ADR, and a Phase 5 contract amendment.

**GDPR deletion is hard delete.** The `DELETE /v1/users/{id}/memories` endpoint (and any equivalent) must execute a hard delete — `DELETE FROM memories WHERE user_id = $1` — with no soft-delete, archival, or backup exclusion. Confirm with the data steward that backup retention policies are also addressed.

**No domain dependencies on infrastructure.** The domain layer (ports and entities) must not import Spring, JDBC, JPA, Ollama, or any external library. Domain logic is tested with plain JUnit without a running application context. This is not a style guideline — it is an architecture constraint.

**Test coverage before merge.** No feature branch merges to main with JaCoCo line coverage below 80% or branch coverage below 80% for the changed modules. The CI pipeline enforces this. Do not merge with a coverage waiver pending "we'll add tests later."

**Conventional commits.** All commits follow `type(scope): description`. Types: `feat`, `fix`, `test`, `docs`, `refactor`, `chore`. The scope is the module name. The description is imperative and lowercase. No merge commits on feature branches — rebase before merge. This is not a style preference; commit history is the audit trail for intelligence system changes.

**OpenAPI spec is authoritative.** The API spec is generated from annotations (`@Operation`, `@Schema`, `@ApiResponse`) and committed to the repository. It is not generated at runtime and not hand-edited. If the spec conflicts with the implementation, the build fails.

---

## Testing Approach

Tests are organized in three layers:

**Unit tests** — test domain logic, agent decision functions, memory mechanics, and retrieval ranking in isolation. No external dependencies. Use TestContainers for database tests that require a real schema. Every agent's core decision logic must have unit tests for: correct action at threshold, escalation below threshold, invalid input rejection, and tool failure handling.

**Integration tests** — test end-to-end flows with real infrastructure. Spin up PostgreSQL with pgvector via TestContainers. Test: memory retrieval by similarity, decay job execution, GDPR deletion cascade, agent pipeline execution, and confidence gate routing. Integration tests run in CI on every pull request.

**Contract tests** — verify that every API endpoint matches its OpenAPI spec. Use tools like Pact or a generated test suite from the spec. Any endpoint not covered by a contract test is a governance gap.

**Adversarial tests** — test failure modes documented in each Agent Contract: what happens when the LLM returns an unparseable response, when tool latency exceeds the timeout, when the vector index is unavailable, when the embedding service is down. These tests verify graceful degradation, not just happy path behaviour.

---

## Dependency and Build Standards

**Java 21** minimum. Use virtual threads (Project Loom) for I/O-bound operations (embedding calls, LLM calls, database queries). This enables the concurrency required to meet p99 ≤ 2000ms without thread pool exhaustion.

**Spring Boot 3.x** for the application layer only. No Spring annotations in the domain layer.

**pgvector** via the `pgvector-java` extension for the JDBC driver. The ivfflat index must be created with a `lists` parameter calibrated for the expected number of memory records.

**Flyway** for schema migrations. Every schema change is a numbered migration file. No `schema-create` in application properties. Migrations are irreversible — if a migration is wrong, a new migration fixes it.

**Micrometer + Prometheus** for metrics. Every agent invocation is a metric event: agent name, action type, confidence score, duration, outcome (auto-enforced vs. human-review). These metrics are the observability inputs for Phase 9 governance monitoring.

---

## Phase Gate

Before Phase 7 (Evaluation) begins:

- [ ] All database migrations run successfully on a clean schema
- [ ] All unit tests pass with ≥ 80% coverage on domain and agent modules
- [ ] All integration tests pass against real infrastructure (TestContainers)
- [ ] Confidence gate is implemented in the agent runtime and tested
- [ ] GDPR deletion endpoint is implemented and deletes all user records
- [ ] OpenAPI spec is committed and matches implementation
- [ ] All Agent Contract clauses are satisfied (verified by review against contract)
- [ ] Observability pipeline is running (metrics visible in Prometheus/Grafana)
- [ ] No OWASP CVSS High or Critical CVEs in the dependency tree

| Role | Name | Signature | Date |
|---|---|---|---|
| Architect | | | |
| Engineering Lead | | | |

---

## Inputs

- All Phase 1–5 design artifacts
- Technology stack decisions from Phase 2 ADRs
- Memory Schema and SQL migrations (Phase 4)
- Agent Contracts (Phase 5)
- RAG Configuration (Phase 3)

## Outputs

- Working system codebase with full test suite
- OpenAPI specification
- CI/CD pipeline configuration
- Deployment manifests (Docker Compose / Kubernetes)

## Feeds Into

Phase 7 (Evaluation) — the working system is evaluated against all thresholds defined in Phases 1–5.
