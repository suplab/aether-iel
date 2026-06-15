---
name: aiel-decisions
description: Key methodology decisions made for AIEL
type: project
---

# AIEL Key Decisions

## D-001: AIEL is Methodology-Only (No Runtime)

**Decision:** AIEL is a documentation and methodology framework. It has no Java runtime.
**Why:** The lifecycle framework applies across all Aether repositories. Embedding it in a runtime would couple it unnecessarily.
**How to apply:** When asked to "implement AIEL", produce documentation, templates, and checklists — not code.

## D-002: 12 Phases Cover the Full Intelligence Lifecycle

**Decision:** AIEL defines 12 engineering phases from Strategy through Retirement.
**Why:** Traditional SDLC phases (design, build, test, deploy) are insufficient for intelligence systems that remember, learn, and evolve.
**How to apply:** Every new intelligence system should be assessed against all 12 phases before any implementation begins.

## D-003: Intelligence Maturity Model Has 5 Levels

**Decision:** The IMM defines 5 maturity levels from Prompt Experiments to Distributed Intelligence Ecosystems.
**Why:** Maturity levels give teams a shared language for where they are and where they are going.
**How to apply:** Use the IMM to assess any AI initiative and recommend the appropriate AIEL phases for their maturity level.

## D-004: EEIK Bootstrap Applied to This Repository

**Decision:** aether-iel uses the same EEIK bootstrap as all other Aether repositories.
**Why:** Consistency across the ecosystem; CLAUDE.md + memory + agents in every repo.
**How to apply:** All new Aether repositories start with the EEIK bootstrap as commit 1.

## D-005: Governance is a Phase 1 Constraint, Not a Phase 9 Audit

**Decision:** Governance engineering begins at Phase 1 (Strategy), not Phase 9.
**Why:** Governance shapes agent authority, memory retention, and human-in-the-loop thresholds from the start.
**How to apply:** The intelligence brief template includes governance requirements as a mandatory section.
