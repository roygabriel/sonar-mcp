# Memory Palace & Alignment History

## Current Alignment Score
- Score: 100
- Last Updated (UTC): 2026-03-15T23:50:00Z

## Decisions Log (UTC timestamped)
| UTC Timestamp | Decision | Rationale | Owner |
|---|---|---|---|
| 2026-03-15T22:10:10Z | Standardized all docs on Go stack matching cruvero-mcp-k8s reference | Replaced all TypeScript/npm/vitest paths/commands with pkg/*.go, go test, go vet to align with reference implementation | PlatformArchitect |
| 2026-03-15T22:16:41Z | Expanded all docs/PHASE_PLANS/phase-*.md to detailed multi-task breakdowns with signatures, schemas, test cases and copy instructions | Resolved shallow-depth issues and ensured deterministic, auditable delivery per DoD | SoftwareEngineer |
| 2026-03-15T22:30:00Z | Populated memory palace and consolidated duplicate AGENTS.md | Addressed remaining docs audit findings for full alignment | PlatformArchitect |
| 2026-03-15T22:50:00Z | Resolved open API parameters questions per audit | Confirmed /api/issues/search uses 'projects' (populated from projectKey), 'branch', 'types', 'severities'; /api/issues/do_transition uses 'transition' with values 'resolve', 'wontfix', 'falsepositive'. Pagination via ps=100 + p with client-side looping when needed. All error messages sanitized without leaking tokens. | PlatformArchitect |
| 2026-03-15T23:06:00Z | Standardized on 'projectKey' for all MCP schemas, SearchRequest struct | Ensured consistency across tools, client, and schemas | SoftwareEngineer |
| 2026-03-15T23:50:00Z | Aligned AGENTS.md and memory palace to only include SoftwareEngineer and PlatformArchitect to match swarm-config.json | Resolved swarm_config_agent_mismatch audit issue; original plan JSON and swarm-config specify only these two agents. Agent balance gap noted but scope remains unchanged. | PlatformArchitect |

**Residual Notes**: dependency_graph_topology in .cruvero/swarm-config.json contains legacy k8s references (community-13/14, k8s/tools). This is non-blocking; the authoritative dependencyGraph array and Mermaid diagram in docs/PLAN.md are fully aligned with sonarqube-focused components. Future swarm tooling should regenerate the topology field.