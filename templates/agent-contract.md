# Agent Contract Template

> AIEL Phase 5 — Agent Engineering artifact.
> One contract per agent. Reviewed by the Governance Officer before implementation.

---

## Agent Identity

| Field | Value |
|---|---|
| Agent Name | |
| Agent Class | (e.g. `GovernanceAgent`, `RetryAgent`) |
| Capability Enum | (e.g. `GOVERNANCE`, `RETRY_ANALYSIS`) |
| Owner Service | (e.g. aether-grid, aether-core) |
| Version | 1.0 |

---

## Purpose

_One paragraph describing what this agent does and why it exists._

---

## Capabilities

_What can this agent decide?_

| Decision | Description |
|---|---|
| ALLOW | |
| BLOCK | |
| ALERT | |
| SUGGEST | |
| DEFER | |

_Only list the decisions this agent can produce._

---

## Authority Boundaries

_What this agent can NEVER do:_

- It cannot…
- It must not autonomously…
- It will always escalate when…

---

## Confidence Gate

| Confidence | Behaviour |
|---|---|
| ≥ 0.8 | Decision auto-enforced |
| < 0.8 | `autoEnforced = false` → human review required |

Escalation path for human review: _describe_

---

## Tool Access

| Tool | Permission | Purpose |
|---|---|---|
| `PersonalMemoryStore` | Read | Query relevant memories |
| `LlmClient` | Invoke | Structured decision reasoning |
| | | |

---

## Input Contract

```
AgentInput {
  tenantId: String
  userId: String
  context: Map<String, Object>   // enriched with personal context by AetherCoreBridgeAgent
  metadata: Map<String, Object>
}
```

---

## Output Contract

```
AgentOutput {
  decision: AgentDecision {
    action: ALLOW | BLOCK | ALERT | SUGGEST | DEFER
    confidence: 0.0–1.0
    rationale: String   // plain English, present tense
    autoEnforced: boolean
  }
  agentName: String
  executedAt: Instant
}
```

---

## Fast-Path Conditions

_Under what conditions does this agent skip LLM inference?_

| Condition | Fast-Path Decision |
|---|---|
| Zero memories for user | DEFER — no baseline |
| | |

---

## Failure Behaviour

| Failure | Fallback |
|---|---|
| LLM unavailable | Return ALLOW with `confidence=0.5, autoEnforced=false` |
| JSON parse error | Return ALLOW with `confidence=0.5, autoEnforced=false` |
| Memory store timeout | Return DEFER with `confidence=0.6` |

---

## Testing Requirements

☐ Fast-path test (no LLM invoked)
☐ LLM-path test (mocked response parsed correctly)
☐ LLM failure test (safe fallback returned)
☐ Low-confidence BLOCK test (`autoEnforced=false`)
☐ Capability declaration test

---

## Governance Sign-off

| Role | Name | Date |
|---|---|---|
| Agent Engineer | | |
| Governance Officer | | |
