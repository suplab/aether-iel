# /agent-contract

Generate an agent contract (AIEL Phase 5 artifact).

## Instructions

When this command is invoked, prompt the user for:
1. Agent name and purpose
2. Capabilities (what can it decide?)
3. Authority limits (what can it NOT do?)
4. Tool access (which external systems)
5. Confidence threshold
6. Escalation path when confidence < threshold

Then produce a complete Agent Contract using `templates/agent-contract.md`.
