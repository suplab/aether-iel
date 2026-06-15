# Phase 5 — Agent Engineering

> **AIEL Phase Group: Intelligence Design**
> Phase 5 defines every agent that will operate within the system: its contract, its authority, its tools, its confidence gate, and its failure modes. No agent is built without a signed Agent Contract.

---

## Purpose

Agents are the actors of an intelligence system. They receive inputs, reason over context, invoke tools, and produce decisions or outputs. Poorly defined agents are the single most common source of intelligence system failures: agents that overstep their authority, agents that operate with misplaced confidence, agents that have no defined failure mode, and agents that are impossible to audit after the fact.

Phase 5 prevents these failures by treating every agent as a governed entity with a formal contract. The Agent Contract is not documentation produced after implementation — it is the specification from which implementation follows. An agent without a contract does not get built.

---

## What an Agent Contract Contains

Each Agent Contract specifies:

**Identity** — agent name, version, role description, owning team.

**Authority Boundaries** — what the agent is permitted to do autonomously (SUGGEST, ALERT, RATE_LIMIT, BLOCK) and what requires human review. Every agent has a confidence gate. Every action type below the threshold routes to the human review queue — no exceptions.

**Input Contract** — what the agent receives as input, in what format, and what constitutes a valid vs. invalid input. Invalid inputs should be rejected at the boundary, not handled mid-reasoning.

**Output Contract** — what the agent produces, in what format, and what every field means. Agents that return free text are not governed agents. Agents that return structured JSON with named fields, confidence scores, and action types are.

**Tool Permissions** — which tools the agent is authorized to call, and under what conditions. Tools not in the permission list are unreachable by the agent. This is enforced in the agent runtime, not by convention.

**Memory Access** — which memory types the agent can read from and write to, and what it is prohibited from storing. An agent that classifies intent should not be writing user personal data to episodic memory.

**Failure Modes** — explicit handling for: low confidence, missing context, tool failure, timeout, and unexpected input format. An agent with undefined failure modes will behave unpredictably under load.

**Observability** — what the agent logs, what metrics it emits, and what constitutes an auditable event. Every agent action that affects a user must be auditable.

---

## Key Activities

**1. Agent Role Definition**

Start from the agent topology sketched in Phase 2 and refine each role into a full specification. For each agent, answer: What problem does this agent uniquely solve? Why can't this be done by an existing agent or by the orchestration layer? What would go wrong if this agent were absent?

If these questions do not have clear answers, the agent should not exist. Agent proliferation is a governance liability — more agents means more contracts, more failure modes, more audit surface.

**2. Authority Hierarchy**

Map all agents to authority levels from the decision hierarchy defined in Phase 2. Authority levels are:

| Level | Name | Description | Confidence Requirement |
|---|---|---|---|
| 1 | OBSERVE | Read-only — no action | None |
| 2 | SUGGEST | Produces recommendations for human review | Any (always human review) |
| 3 | ALERT | Flags events in the observability layer | ≥ 0.80 |
| 4 | RATE_LIMIT | Throttles access or degrades service | ≥ 0.85 |
| 5 | BLOCK | Rejects or terminates actions | ≥ 0.95 |

An agent must not be granted a higher authority level than what was approved in Phase 1's intelligence scope. Authority escalation requires a Phase 1 amendment, not a code change.

**3. Tool Registry**

Create a tool registry listing every tool available in the system and which agents are authorized to use each. Tools include: memory read/write operations, external API calls, database queries, file system access, LLM calls, and any other side-effecting action.

The tool registry is the governance boundary for agent capabilities. A tool not in the registry does not exist from the agent's perspective. Adding a new tool requires a registry update, a governance review, and a new or revised Agent Contract for each agent that will use it.

**4. Confidence Gate Implementation**

The confidence gate is implemented in the agent runtime, not in each agent. Every agent invocation returns a result with a confidence score. The runtime evaluates the score against the action type's threshold before allowing execution. If below threshold, the result is routed to the human review queue with full context (agent input, reasoning, confidence score, proposed action).

This architecture ensures that: no agent can bypass the confidence gate by modifying its own output, the gate logic is in one place and independently testable, and the human review queue receives consistent structured data regardless of which agent produced the result.

**5. Agent Interaction Model**

Define how agents communicate and in what sequence. Supported patterns (established in Phase 2's cognitive design):

*Sequential Pipeline*: Agent A → Agent B → Agent C. Each agent receives the output of the previous agent. Simple to reason about, but the slowest pattern and most vulnerable to upstream failure.

*Parallel Fan-out*: Multiple agents receive the same input simultaneously. An aggregator collects results and produces a final output. Increases resilience but requires conflict resolution when agents disagree.

*Supervisor + Workers*: A supervisor agent decomposes a task, delegates to specialist workers, and synthesizes their outputs. The supervisor holds no tool permissions — it only coordinates. This is the primary pattern for complex, multi-step tasks in Aether.

*Debate / Consensus*: Multiple agents produce independent outputs; a judge agent selects the most appropriate or synthesizes a consensus view. Used for high-stakes decisions where individual agent accuracy is insufficient.

Document which pattern applies to each agent group and justify the choice.

**6. Agent Testing Strategy**

Each Agent Contract must include a testing strategy before Phase 6 implementation begins. At minimum: unit tests for the decision logic with mocked tools, integration tests that exercise the full agent with real tool dependencies, adversarial tests that probe failure modes (low confidence, invalid input, tool timeout), and contract tests that verify input/output format compliance.

Test coverage for agent decision logic must meet the ecosystem minimum of 80% branch coverage. Untested decision branches in agents are governance gaps.

---

## Primary Artifact: Agent Contracts

One Agent Contract per agent. Each contract should be filed at `/aether-iel/artifacts/{project-name}/agent-contracts/` with filename `{agent-name}-contract.md`.

The template is available at `/aether-iel/templates/agent-contract.md` (to be written). Each contract is versioned independently — a change to one agent's contract does not require re-approval of others.

---

## Phase Gate

Before Phase 6 (Implementation) begins, the following must be true:

- [ ] All agents identified in the Phase 2 topology have Agent Contracts
- [ ] Every Agent Contract specifies authority level consistent with Phase 1 scope
- [ ] Every Agent Contract specifies confidence gates for each action type
- [ ] Tool registry is complete — every tool used by any agent is listed
- [ ] Agent interaction model (pipeline, fan-out, supervisor, or debate) is documented
- [ ] Agent failure modes are defined for all contracts
- [ ] Observability requirements (logs, metrics, audit events) are defined for all contracts
- [ ] Testing strategy is included in all contracts
- [ ] All contracts are reviewed and signed by architect and governance officer

| Role | Name | Signature | Date |
|---|---|---|---|
| Architect | | | |
| Governance Officer | | | |

---

## Inputs

- Approved Cognitive Design with agent topology (Phase 2)
- Tool inventory (what tools are available or must be built)
- Intelligence scope from Phase 1 (authority boundaries cannot exceed this)

## Outputs

- **Agent Contracts** (one per agent, signed, version-controlled)
- **Tool Registry** (tool name, description, authorized agents, side effects)
- **Agent Interaction Diagrams** (for each agent group)

## Feeds Into

Phase 6 (Implementation) — engineers implement each agent from its contract.
Phase 7 (Evaluation) — agent decision accuracy and confidence gate behaviour are evaluated against contract specifications.
Phase 9 (Governance) — the tool registry and authority levels become inputs to the Policy Engine configuration.
