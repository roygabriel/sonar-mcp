# AGENTS

## Mission
- Execute this repository as a coordinated coding swarm with deterministic, auditable delivery.
- Follow docs/PLAN.md and phase docs as strict source of truth.

## Source of Truth
- `docs/PLAN.md`
- `docs/PHASE_PLANS/phase-*.md` (now expanded with detailed subtasks, signatures, test cases, and reference copy instructions)
- `docs/PROMPTS/system.md` and `docs/PROMPTS/agent-*.md`
- `docs/AUDIT/*.md`
- `docs/WORK_NOTES/memory-palace.md`
- `.cruvero/swarm-config.json`

## Coordination Rules
- Respect phase dependencies and audit gates before advancing.
- Record decisions/open questions with UTC timestamps in WORK_NOTES.
- Preserve reproducibility with explicit validation commands and evidence.
- Escalate blockers immediately under Blockers / Escalations.
- Ensure docs/WORK_NOTES/memory-palace.md is updated after every audit or major decision.

## Definition of Done
- All acceptance criteria validated with measurable evidence.
- All phase audits pass.
- Residual risks documented with owners and mitigation paths.
- Docs and implementation state are fully aligned.
- Phase plans contain sufficient detail (files to copy, signatures, schemas, test cases) to eliminate shallow-depth issues.

## Agent Team
- SoftwareEngineer (promptVersion: v2): Implements SonarQube client, tools, and test coverage.
- PlatformArchitect (promptVersion: v1): Ensures exact structural and framework parity with k8s reference.

**Alignment Note**: Agent team intentionally limited to these two roles per swarm-config.json and original plan JSON. QA/Security/Delivery extension previously noted in memory palace has been reverted to eliminate mismatch. Agent balance gap is acknowledged but non-blocking for current scope.

## docs/AGENTS.md
# AGENTS

This file contained duplicated content and has been consolidated.

Please refer to the root AGENTS.md for the latest agent coordination guidelines, mission, source of truth, and definition of done.