# Phase 3: Core UI Components Segment B Segment A

## Overview
This phase must precede later phases because it delivers the foundational sonarqube-client and tools-issues implementations that later phases (gateway registration, helm-chart deployment) directly depend on per the project's dependency graph: sonarqube-client → config (risk: 0.2), tools-issues → sonarqube-client (risk: 0.1), gateway-registration → tools-issues (risk: 0.1), and helm-chart → config (risk: 0.3). Executing this work now prevents downstream integration failures when the full MCP server is wired to the SonarQube REST API and deployed via Vault-injected secrets.

Execute the first scoped segment of this workstream with deterministic outputs and validation.

- All tests passing with >90% coverage for modified packages
- No new findings from go vet or staticcheck on changed code
- Successful E2E execution of transition_issue against dev SonarQube instance
- Dependency edges validated with zero contract breaks
- All framework components (OTEL, logging redaction, health checks) remain untouched

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P3-T01 | Implement TransitionIssue func in pkg/sonarqube/client.go (POST /api/issues/do_transition with resolve/wontfix/falsepositive) and expose via transition_issue tool in internal/tools/issues/transition.go with proper error handling for invalid states | SoftwareEngineer | sonarqube-client → config (risk: 0.2) | pkg/sonarqube/client.go, internal/tools/issues/transition.go | go test ./pkg/sonarqube -run TestTransitionIssue -count=1 && go test ./internal/tools/issues -run TestTransitionIssue |
| P3-T02 | Failure paths (invalid credentials, network errors, malformed params) return clear MCP-compatible errors without leaking secrets in pkg/sonarqube/client.go and internal/tools/issues/error.go; E2E tests confirm behavior | SoftwareEngineer | sonarqube-client → config (risk: 0.2) | pkg/sonarqube/client.go, internal/tools/issues/error.go | go test ./pkg/sonarqube -run TestClientErrorPaths && go vet ./pkg/sonarqube ./internal/tools/issues && go run cmd/mcp/main.go --dry-run |

## Phase-Specific Prompt
```text
You are executing Phase 3: Core UI Components Segment B Segment A.
Overview: Execute the first scoped segment of this workstream with deterministic outputs and validation.
Scope nodes: tools-issues, sonarqube-client
Required reading: docs/PLAN.md, docs/AUDIT/risk-matrix.md, docs/WORK_NOTES/memory-palace.md.
Output only phase-scoped changes with validation evidence and explicit assumptions.
Record every key decision in UTC under WORK_NOTES.
Reference dependency edges: sonarqube-client → config (risk: 0.2), tools-issues → sonarqube-client (risk: 0.1)
```

## Phase-Specific Audit Prompt
```text
Audit Phase 3: Core UI Components Segment B Segment A against acceptance criteria, risk controls, and deterministic behavior.
Return findings ordered by severity with explicit pass/fail gate decision and required remediations.
```

## Risks / Gates
| Risk | Trigger | Gate | Mitigation | Success Criteria |
|---|---|---|---|---|
| Dependency mismatch | integration check fails on sonarqube-client → config or tools-issues → sonarqube-client | phase-audit-3 | reconcile contracts and retest | all phase validation commands pass |
| Scope drift | non-phase changes appear in gateway-registration or helm-chart | phase-audit-3 | update PLAN + re-scope before proceeding | only phase-scoped deliverables remain |
| Quality regression | validation gate fails | release quality gate | patch and rerun full checks | clean quality signal |

## Estimated Swarm Duration
- 7h swarm time