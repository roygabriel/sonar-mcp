# Phase 5: Core UI Components Segment B Segment B Segment B

## Overview
This phase must precede later phases because it completes the tools-issues implementation that gateway-registration depends on (risk: 0.1) and enables safe integration with the helm-chart (risk: 0.3). Executing tools-issues now ensures the sonarqube-client → config edge (risk: 0.2) is stabilized before any deployment or gateway changes, preventing cascading integration failures across the MCP server fork from cruvero-mcp-k8s@dev.

Execute the second scoped segment of this workstream with deterministic outputs and validation.

Exit criteria checklist:
- search_issues tool passes go test with structured results matching SonarQube API shape for projectKey/branch filters
- transition_issue tool correctly handles resolve/wontfix/falsepositive transitions with error cases covered
- All OTEL tracing, structured logging, and health check components match reference implementation with zero diff
- Gateway registration exposes tools with accurate JSON schemas, risk labels, and hints
- go vet, go test, and integration curl checks return clean results with no new warnings

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P5-T01 | Implement search_issues (pkg/tools/issues.go:searchIssues) and transition_issue (pkg/tools/issues.go:transitionIssue) using sonarqube-client calls in client/search.go and client/issues.go; update tool registration in cmd/server/main.go and telemetry hooks in internal/telemetry/tracer.go. | SoftwareEngineer | sonarqube-client → config (risk: 0.2), tools-issues → sonarqube-client (risk: 0.1), phase-4 audit pass | implementation + evidence | `go test ./pkg/tools -run 'TestSearchIssues\|TestTransitionIssue' && go vet ./pkg/tools && go test ./internal/telemetry` |

## Phase-Specific Prompt
```text
You are executing Phase 5: Core UI Components Segment B Segment B Segment B.
Overview: Execute the second scoped segment of this workstream with deterministic outputs and validation.
Scope nodes: tools-issues
Required reading: docs/PLAN.md, docs/AUDIT/risk-matrix.md, docs/WORK_NOTES/memory-palace.md.
Output only phase-scoped changes with validation evidence and explicit assumptions.
Record every key decision in UTC under WORK_NOTES.
Reference dependency edges: sonarqube-client → config (risk: 0.2), tools-issues → sonarqube-client (risk: 0.1).
```

## Phase-Specific Audit Prompt
```text
Audit Phase 5: Core UI Components Segment B Segment B Segment B against acceptance criteria, risk controls, and deterministic behavior.
Return findings ordered by severity with explicit pass/fail gate decision and required remediations.
Validate: search_issues tool, transition_issue tool, framework components parity, gateway registration, and dependency edges (tools-issues → sonarqube-client risk 0.1).
```

## Risks / Gates
| Risk | Trigger | Gate | Mitigation | Success Criteria |
|---|---|---|---|---|
| Dependency mismatch | integration check fails | phase-audit-5 | reconcile contracts and retest | all phase validation commands pass, sonarqube-client → config (risk: 0.2) and tools-issues → sonarqube-client (risk: 0.1) edges stable |
| Scope drift | non-phase changes appear | phase-audit-5 | update PLAN + re-scope before proceeding | only phase-scoped deliverables remain |
| Quality regression | validation gate fails | release quality gate | patch and rerun full checks | clean quality signal, exit criteria checklist 100% met |

## Estimated Swarm Duration
- 6h swarm time