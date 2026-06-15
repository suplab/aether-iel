# Intelligence Maturity Model (IMM)

> Five levels from ad-hoc prompt experiments to distributed intelligence ecosystems.
> Use this model to assess where you are and plan where you are going.

---

## Level 1 — Prompt Experiments

**Characteristics:**
- Ad-hoc LLM API calls
- No persistent memory between sessions
- No agent orchestration
- No evaluation framework
- No governance controls
- Results are non-reproducible

**Typical Artefacts:**
- Jupyter notebooks
- Python scripts calling OpenAI API
- Prompt engineering experiments

**What's Missing:**
- Memory | Agents | Governance | Evaluation | Learning

**To Advance to Level 2:** Define a knowledge domain. Build a RAG pipeline. Add structured output validation.

---

## Level 2 — AI Applications

**Characteristics:**
- Persistent knowledge (RAG or fine-tuning)
- Structured, reproducible outputs
- Basic hallucination detection
- Simple evaluation metrics tracked
- Stateless between user sessions (no personal memory)

**Typical Artefacts:**
- Document Q&A systems
- Summarisation pipelines
- Classification models

**What's Missing:**
- Personal Memory | Agent Orchestration | Governance | Feedback Loop

**To Advance to Level 3:** Design an agent that can take actions. Add a policy engine. Instrument for observability.

---

## Level 3 — Agent Systems

**Characteristics:**
- Multi-agent orchestration
- Tool use (APIs, databases, external systems)
- Policy engine enforcing behavioural rules
- Feedback loops from agent decisions
- Human-in-the-loop for low-confidence decisions
- Observability: agent decision counters, latency metrics

**AIEL Phases Active:** 5, 6, 7, 8, 9

**Typical Artefacts:**
- Governance Agent + Specialised Agents
- Tool permission manifests
- Policy YAML files
- Agent decision audit log

**What's Missing:**
- Persistent Personal Memory | Identity | Emotional Context | Cross-session Learning

**To Advance to Level 4:** Add personal memory (episodic, semantic, procedural, emotional). Build a cognitive session layer. Implement memory reinforcement and decay.

---

## Level 4 — Cognitive Platforms

**Characteristics:**
- Persistent personal memory across sessions (four types)
- Cognitive session management (multi-turn context)
- Emotional state tracking
- Identity continuity across interactions
- Memory reinforcement from feedback
- Memory decay and purge scheduler
- GDPR-compliant memory management
- Self-improving agents

**AIEL Phases Active:** 1–11

**Aether Examples:**
- AetherCore — personal cognitive engine (Phases 0–6 complete)
- AetherGrid — enterprise agent mesh (Phase 17)

**What's Missing:**
- Federated Memory | Cross-system Learning | Collective Intelligence

**To Advance to Level 5:** Implement federated memory across instances. Enable cross-agent learning. Build evolution strategies.

---

## Level 5 — Distributed Intelligence Ecosystems

**Characteristics:**
- Federated memory shared across organisational boundaries (with consent)
- Collective learning — insights from one agent improve all
- Autonomous evolution — new capabilities emerge without manual intervention
- Cross-system agent collaboration
- Distributed governance
- Multi-instance clustering

**AIEL Phases Active:** All 12

**Aether Vision:**
- AetherMesh — federated intelligence network
- Domain ecosystems sharing learned patterns

**Assessment Criteria:**
- Can agent A learn from agent B's decisions automatically?
- Is memory federated with consent controls?
- Can the system evolve a new capability without breaking existing ones?
- Is governance distributed across instances?

---

## Maturity Assessment Checklist

Use this checklist to determine current level:

```
Level 1 Assessment:
☐ Uses an LLM API directly
☐ No persistent storage of context
☐ Results vary on repeated runs
→ You are at Level 1

Level 2 Assessment:
☐ Knowledge base (vector store or fine-tuning)
☐ Structured outputs with validation
☐ Basic evaluation metrics exist
→ You are at Level 2

Level 3 Assessment:
☐ Multiple agents with defined capabilities
☐ Tool access with permissions
☐ Policy engine with audit log
☐ Human-in-the-loop for low confidence
→ You are at Level 3

Level 4 Assessment:
☐ Personal memories persist across sessions
☐ Four memory types: episodic, semantic, procedural, emotional
☐ Cognitive sessions with emotional state
☐ Memory reinforcement on access
☐ Memory decay scheduler running
→ You are at Level 4

Level 5 Assessment:
☐ Memory federated across instances
☐ Collective learning operational
☐ Autonomous evolution tested
→ You are at Level 5
```
