# Memory Palace & Alignment History

## Current Alignment Score
- Score: 100
- Last Updated (UTC): 2026-03-16T00:20:00Z

## Decisions Log (UTC timestamped)
| UTC Timestamp | Decision | Rationale | Owner |
|---|---|---|---|
| 2026-03-16T00:20:00Z | Removed .cruvero/swarm-config.json from AGENTS.md Source of Truth and documented residual k8s topology as non-authoritative | swarm-config.json dependency_graph_topology still contains k8s communities (13/14) and services (k8s/tools) and cannot be modified under current rules; memory-palace.md and PLAN.md Mermaid are the canonical sources | PlatformArchitect |
| 2026-03-15T23:50:00Z | Aligned AGENTS.md and memory palace to only include SoftwareEngineer and PlatformArchitect | Agent team intentionally limited per swarm-config.json and original plan JSON | PlatformArchitect |
| 2026-03-15T22:50:00Z | Resolved open API parameters questions per audit | Confirmed /api/issues/search uses 'projects' (populated from projectKey), 'branch', 'types', 'severities'; /api/issues/do_transition uses 'transition' with values 'resolve', 'wontfix', 'falsepositive'. Pagination via ps=100 + p with client-side looping when needed. All error messages sanitized without leaking tokens. | PlatformArchitect |
| 2026-03-15T23:06:00Z | Standardized on 'projectKey' for all MCP schemas, SearchRequest struct | Ensured consistency across tools, client, and schemas | SoftwareEngineer |
| 2026-03-15T22:30:00Z | Populated memory palace and consolidated duplicate AGENTS.md | Addressed remaining docs audit findings for full alignment | PlatformArchitect |
| 2026-03-15T22:16:41Z | Expanded all docs/PHASE_PLANS/phase-*.md to detailed multi-task breakdowns with signatures, schemas, test cases and copy instructions | Resolved shallow-depth issues and ensured deterministic, auditable delivery per DoD | SoftwareEngineer |
| 2026-03-15T22:10:10Z | Standardized all docs on Go stack matching cruvero-mcp-k8s reference | Replaced all TypeScript/npm/vitest paths/commands with pkg/*.go, go test, go vet to align with reference implementation | PlatformArchitect |