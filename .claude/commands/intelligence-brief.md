# /intelligence-brief

Generate a new intelligence system brief (AIEL Phase 1 artifact).

## Instructions

When this command is invoked, prompt the user for:
1. System name and one-line description
2. Primary purpose (what decisions does it make?)
3. Target domain (healthcare, insurance, developer productivity, etc.)
4. Estimated user base
5. Key risks

Then produce a complete Intelligence Brief using the template at `templates/intelligence-brief.md`.

Include:
- Vision statement
- Business capability map
- Agent authority boundaries
- Memory requirements
- Governance requirements
- Success metrics
- Risk register
