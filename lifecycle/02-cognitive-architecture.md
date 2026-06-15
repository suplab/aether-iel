# Phase 2 — Cognitive Architecture Engineering

> **AIEL Phase Group: Intelligence Design**
> Phase 2 defines how the system thinks. It selects the reasoning approach, memory model, agent topology, and LLM configuration before any construction begins. All later phases depend on decisions made here.

---

## Purpose

Phase 2 produces the **Cognitive Design** — the blueprint for the system's intelligence machinery. Where Phase 1 defines *what* the system will do, Phase 2 defines *how* it will reason, remember, and decide.

The Cognitive Design is not a high-level wish list. It is a concrete technical specification: specific reasoning strategies, specific memory types with retention rules, specific agent roles with named authority levels, and specific LLM choices with data residency confirmation. Ambiguity in the Cognitive Design propagates directly into implementation errors.

---

## Why This Phase Comes Before Knowledge and Agent Engineering

A common mistake is to define agents (Phase 5) before defining the cognitive model they operate within. Agents that are designed without a memory model cannot use memory correctly. Agents designed without a reasoning strategy tend to over-rely on LLM generation and under-rely on structured retrieval. The Cognitive Design provides the frame that all subsequent phases fill in.

Similarly, knowledge engineering (Phase 3) requires knowing what kind of knowledge the system needs — declarative facts, procedural workflows, episodic history, or affective context — before schemas can be designed. The memory architecture in Phase 2 answers that question.

---

## Key Activities

**1. Reasoning Strategy Selection**

Select the primary reasoning approach and justify the choice against the problem statement from Phase 1. The available strategies are not interchangeable — each has distinct failure modes and infrastructure requirements.

Retrieval-Augmented Generation (RAG) is appropriate when the system needs access to a large, structured knowledge base and factual accuracy is the primary concern. It fails when knowledge is incomplete and the system must reason across gaps.

Chain-of-Thought (CoT) is appropriate when the problem requires multi-step logical reasoning and the LLM context window is large enough to hold the full reasoning chain. It fails at long-horizon tasks where earlier reasoning is truncated.

ReAct (Reason + Act) combines thought generation with tool invocation, making it appropriate for agent systems that must take real-world actions in response to reasoning outputs. This is the primary pattern for Aether agents.

Multi-agent debate is appropriate when decisions are high-stakes and no single model has sufficient accuracy. It adds latency and complexity — use it deliberately, not by default.

Document the selected strategy and explicitly rule out the alternatives with brief justifications. This prevents later re-litigation of the decision.

**2. Decision Hierarchy Design**

Map every decision type the system will make to a confidence threshold and an escalation path. This is not optional and not configurable at runtime. The non-negotiable rule is: confidence < 0.8 means `autoEnforced = false` and the decision routes to human review.

Decision types should be named concretely (BLOCK, RATE_LIMIT, ALERT, SUGGEST, DEFER) rather than abstractly (high, medium, low). Each type should have a defined consequence, a confidence threshold, and a named escalation path. The governance controls in Phase 9 will operationalize these definitions — write them precisely enough that a policy engine can implement them.

**3. Memory Architecture**

Define which memory types the system requires and how each will be stored, retrieved, and maintained.

Episodic memory stores specific events with timestamps. It answers "what happened?" It decays with time because older events are less relevant. It is the foundation of cross-session context.

Semantic memory stores facts and general knowledge. It answers "what is true?" It is more stable than episodic memory and changes through deliberate updates rather than decay. It is the foundation of knowledge-augmented reasoning.

Procedural memory stores learned workflows and successful action sequences. It answers "how is this done?" It strengthens through repeated successful use and weakens when approaches are superseded.

Emotional memory stores affective context: user preferences, sentiment patterns, interaction quality signals. It answers "how does the user respond?" It is the most sensitive category from a privacy standpoint and requires explicit retention and deletion policies.

For each memory type the system will use, specify: the storage backend, the embedding model and dimensions, the indexing strategy, the retention period, the decay function, and the GDPR deletion classification. Do not defer these decisions to Phase 4 without at minimum specifying them here.

**4. Agent Topology**

Sketch the agent topology at a level sufficient to validate the cognitive model. Full agent specifications are Phase 5 work, but the Phase 2 topology establishes: how many agents, what their roles are, how they communicate (sequential pipeline, parallel fan-out, supervisor model, or debate), and what authority level each agent class holds.

Authority levels must be consistent with the intelligence scope from Phase 1. An agent cannot have authority to BLOCK if blocking was not included in the scope. An agent cannot SUGGEST without a defined escalation path if confidence falls below threshold.

**5. LLM Selection**

Select the primary LLM, confirm data residency requirements, define the fallback model, and specify the prompt architecture. Data residency is not a detail — if the system processes data subject to GDPR, HIPAA, or sector-specific regulation, the LLM provider must be confirmed as compliant before Phase 2 closes.

Prompt architecture covers: system prompt strategy (role, constraints, output format), few-shot example policy, structured output format, and JSON parse failure handling. These decisions affect agent reliability more than model selection in most cases. A mediocre model with a well-designed prompt consistently outperforms a powerful model with a poorly structured one.

**6. Performance Requirements**

Establish latency, throughput, and availability targets that flow from the success criteria in Phase 1. For each target, specify the measurement point (end-to-end, per-agent, per-retrieval) and the hard limit at p99. The hard limit at p99 ≤ 2000ms is an ecosystem non-negotiable. Systems that cannot meet this must change their architecture in Phase 2, not after evaluation in Phase 7.

---

## Primary Artifact: Cognitive Design

The Cognitive Design documents all decisions from this phase. Use the template at `/aether-iel/templates/cognitive-design.md`. The completed document should be version-controlled in `/aether-iel/artifacts/` with the project name and date.

The Cognitive Design must remain stable after Phase 2 approval. Changes during later phases require a formal revision with a new version number and re-approval. This stability is what makes later phases tractable — teams in Phase 5 (Agent Engineering) should not be discovering cognitive model changes that affect agent design.

---

## ADRs Required Before Phase 3

The following architecture decisions must be recorded in the project's `/docs/adr/` directory before Phase 2 closes:

| Decision | What to Record |
|---|---|
| Vector store selection | Why pgvector / Pinecone / Weaviate — tradeoffs evaluated |
| Embedding model | Model, dimensions, hosting, latency, fallback |
| LLM provider | Model, provider, data residency confirmation, fallback |
| Context window strategy | How overflow is handled — sliding window, summarization, truncation |
| Memory decay approach | Mathematical function, parameters, purge threshold |
| Agent orchestration pattern | Why sequential / fan-out / supervisor — not just which one |

---

## Phase Gate

Before Phase 3 (Knowledge Engineering) begins, the following must be true:

- [ ] Intelligence Brief (Phase 1) is approved and linked
- [ ] Reasoning approach is selected and alternatives explicitly ruled out
- [ ] Decision hierarchy is complete with named types, thresholds, and escalation paths
- [ ] Memory types are defined with storage, retention, and GDPR deletion classification
- [ ] Agent topology is sketched with named roles and authority levels
- [ ] LLM is selected with data residency confirmed
- [ ] Performance targets are defined with hard limits
- [ ] All ADRs listed above are filed and linked
- [ ] Cognitive Design is reviewed and signed off

| Role | Name | Signature | Date |
|---|---|---|---|
| Architect | | | |
| Governance Officer | | | |

---

## Inputs

- Approved Intelligence Brief (Phase 1)
- Technology constraints (existing infrastructure, approved vendors, data residency requirements)
- Domain knowledge about expected query patterns and knowledge volume

## Outputs

- **Cognitive Design** (signed artifact, version-controlled)
- ADRs for all architecture decisions
- Updated risk register with architecture-level risks

## Feeds Into

Phase 3 (Knowledge Engineering) — knowledge schema and retrieval pipeline are designed within the memory and reasoning model established here.
Phase 5 (Agent Engineering) — agent contracts are scoped by the topology and authority model established here.
Phase 4 (Memory Engineering) — retention policies and schema details are built on the memory architecture established here.
