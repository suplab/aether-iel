# Cognitive Design Template — Phase 2

> **AIEL Phase 2: Cognitive Architecture Engineering**
> Complete this template before any knowledge engineering (Phase 3) or agent definition (Phase 5) begins.
> Requires a completed Intelligence Brief from Phase 1.

---

## System Identity

| Field | Value |
|---|---|
| System Name | |
| Intelligence Brief Reference | `[Link to Phase 1 artifact]` |
| Cognitive Design Version | 0.1 |
| Author | |
| Date | |
| Status | Draft / Under Review / Approved |

---

## 1. Cognitive Strategy

### 1.1 Reasoning Approach

_Describe how the system forms conclusions. Choose one primary strategy and justify it._

| Strategy | Selected | Justification |
|---|---|---|
| Retrieval-Augmented Generation (RAG) | ☐ | |
| Chain-of-Thought (CoT) | ☐ | |
| ReAct (Reason + Act) | ☐ | |
| Tool-calling Loop | ☐ | |
| Multi-agent Debate | ☐ | |
| Fine-tuned Specialist Model | ☐ | |
| Hybrid (describe below) | ☐ | |

**Hybrid Description (if applicable):**

---

### 1.2 Decision Hierarchy

_Map decision types to outcomes. All decisions with confidence < 0.8 must route to human review._

| Decision Type | Confidence Threshold | Auto-Enforce? | Escalation Path |
|---|---|---|---|
| (e.g., BLOCK — reject request) | ≥ 0.95 | Yes | N/A |
| (e.g., RATE_LIMIT — throttle) | ≥ 0.85 | Yes | Supervisor agent |
| (e.g., ALERT — flag for review) | ≥ 0.80 | Yes | Ops dashboard |
| (e.g., SUGGEST — recommendation) | < 0.80 | **No** | Human review queue |
| (e.g., DEFER — needs more context) | Any | No | Human review queue |

**Non-negotiable rule:** confidence < 0.8 → `autoEnforced = false`. This cannot be overridden.

---

## 2. Memory Architecture

### 2.1 Memory Types Required

_Mark all types this system will use. Detail retention and decay in Phase 4._

| Type | Required | Description |
|---|---|---|
| EPISODIC | ☐ | Specific events and interactions with timestamps |
| SEMANTIC | ☐ | Facts, knowledge, and general truths |
| PROCEDURAL | ☐ | How-to knowledge and learned workflows |
| EMOTIONAL | ☐ | Affective context, user preferences, sentiment |

### 2.2 Memory Access Patterns

| Pattern | Required | Notes |
|---|---|---|
| Similarity search (vector) | ☐ | Top-K nearest neighbours by cosine similarity |
| Exact lookup (relational) | ☐ | Key-based retrieval |
| Temporal decay | ☐ | Strength decreases on non-access |
| Reinforce-on-read | ☐ | Strength increases on retrieval |
| Cross-session persistence | ☐ | Memory survives session close |

### 2.3 Embedding Model

| Field | Value |
|---|---|
| Model | (e.g., all-MiniLM-L6-v2 via Ollama) |
| Dimensions | (e.g., 384) |
| Similarity Metric | (e.g., cosine) |
| Hosting | (e.g., local Ollama / remote API) |
| Fallback when unavailable | (e.g., disable, degrade, error) |

---

## 3. Knowledge Architecture

### 3.1 Knowledge Sources

_Detail in Phase 3. List what this system needs to know and where it comes from._

| Source | Type | Update Frequency | Trust Level |
|---|---|---|---|
| | Internal docs | Static / Daily / Real-time | High / Medium / Low |
| | External API | | |
| | User-provided | | |
| | Agent-generated | | |

### 3.2 Knowledge Boundaries

_What this system explicitly does NOT know (prevents hallucination):_

- 
- 
- 

---

## 4. Agent Topology

_High-level. Detail in Phase 5 Agent Contracts._

| Agent | Role | Confidence Gate | Authority Level |
|---|---|---|---|
| (e.g., classifier-agent) | Classifies incoming intent | 0.85 | ALERT / BLOCK |
| (e.g., retrieval-agent) | Fetches relevant context | 0.80 | SUGGEST |
| (e.g., synthesis-agent) | Generates final response | 0.80 | SUGGEST |
| (e.g., governance-agent) | Enforces policy constraints | 0.95 | BLOCK |

**Agent orchestration pattern:**
☐ Sequential pipeline  ☐ Parallel fan-out  ☐ Supervisor + workers  ☐ Debate/consensus

---

## 5. LLM Selection

### 5.1 Model Specification

| Field | Value |
|---|---|
| Primary Model | |
| Provider | (e.g., Anthropic / OpenAI / Ollama / AWS Bedrock) |
| Hosting | (e.g., cloud API / local / VPC) |
| Context Window | (tokens) |
| Fallback Model | |
| Data residency requirement | (e.g., EU-only, US, no restriction) |

### 5.2 Prompt Architecture

| Component | Strategy |
|---|---|
| System prompt | (e.g., role + constraints + output format spec) |
| Few-shot examples | (e.g., yes / no / n examples) |
| Output format | (e.g., structured JSON / free text / markdown) |
| JSON parse failure handling | (e.g., retry once, then return DEFER) |

---

## 6. Context Management

| Concern | Approach |
|---|---|
| Context window overflow | (e.g., sliding window / summarisation / truncation) |
| Multi-turn session context | (e.g., `CognitiveSession` with turn summaries) |
| Cross-session memory injection | (e.g., top-5 similar memories by cosine) |
| PII in context | (e.g., redact before injection) |

---

## 7. Performance Requirements

| Metric | Target | Hard Limit |
|---|---|---|
| p50 response latency | | |
| p99 response latency | | |
| Availability | | |
| Concurrent sessions | | |
| Max memory retrieval time | | |

---

## 8. Technology Decisions

| Component | Selected Technology | Justification |
|---|---|---|
| Vector store | (e.g., pgvector / Pinecone / Weaviate) | |
| Relational store | (e.g., PostgreSQL) | |
| Embedding runtime | (e.g., Ollama local) | |
| LLM runtime | | |
| Session management | (e.g., Aether Core `CognitiveSession`) | |
| Observability | (e.g., Micrometer + Prometheus + OpenTelemetry) | |

---

## 9. ADRs Required

_Architecture decisions that must be recorded before Phase 3 begins._

| Decision | Options Considered | Recommendation |
|---|---|---|
| Vector store selection | | |
| Embedding model | | |
| LLM provider | | |
| Context window strategy | | |
| Memory decay approach | | |

---

## 10. Phase Gate

Before Phase 3 (Knowledge Engineering) begins, confirm:

- [ ] Intelligence Brief (Phase 1) is approved and linked
- [ ] Reasoning approach is selected and justified
- [ ] Memory types are defined
- [ ] Agent topology is sketched (detail in Phase 5)
- [ ] LLM is selected with data residency confirmed
- [ ] All ADRs listed in Section 9 are filed
- [ ] Cognitive Design is reviewed and signed off

| Role | Name | Signature | Date |
|---|---|---|---|
| Architect | | | |
| Governance Officer | | | |
