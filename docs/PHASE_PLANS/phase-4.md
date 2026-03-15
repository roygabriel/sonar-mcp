# Phase 4: Core UI Components Segment B Segment B Segment A

## Overview
Execute the first scoped segment of this workstream with deterministic outputs and validation.

Why Now: This phase must precede later phases because gateway registration is a prerequisite per the dependency edge gateway-registration → tools-issues (risk: 0.1) and must be completed before sonarqube-client → config (risk: 0.2) integration and helm-chart → config (risk: 0.3) validation can succeed. Without registered tools the downstream issue search/transition logic and dev environment deployment cannot be tested end-to-end.

Exit criteria checklist:
- All tools correctly registered with matching JSON schemas, risk classifications, and descriptions
- Validation commands pass cleanly with zero errors or warnings
- Phase audit confirms zero scope drift and full alignment with cruvero-mcp-k8s@dev patterns
- All key decisions recorded in WORK_NOTES with UTC timestamps
- Dependency edge contracts (especially gateway-registration → tools-issues) satisfied

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P4-T01 | Update pkg/gateway/registration.go:registerWithGateway() and internal/tools/definitions.go:buildToolSchemas() to register search_issues (sonarqube-client/issues.go:SearchIssues) and transition_issue (sonarqube-client/issues.go:TransitionIssue) with accurate descriptions, JSON schemas for projectKey/branch/issueKey, risk classification (read-only vs write), and hints | PlatformArchitect | phase-3 audit pass, gateway-registration → tools-issues (risk: 0.1) | implementation + evidence | `go vet ./pkg/gateway && go test ./pkg/gateway -run '^TestRegisterWithMCPGateway$'` |

## Phase-Specific Prompt
```text
You are executing Phase 4: Core UI Components Segment B Segment B Segment A.
Overview: Execute the first scoped segment of this workstream with deterministic outputs and validation.
Scope nodes: sonarqube-client
Required reading: docs/PLAN.md, docs/AUDIT/risk-matrix.md, docs/WORK_NOTES/memory-palace.md.
Output only phase-scoped changes with validation evidence and explicit assumptions.
Record every key decision in UTC under WORK_NOTES.
```

## Phase-Specific Audit Prompt
```text
Audit Phase 4: Core UI Components Segment B Segment B Segment A against acceptance criteria, risk controls, and deterministic behavior.
Return findings ordered by severity with explicit pass/fail gate decision and required remediations.
```

## Risks / Gates
| Risk | Trigger | Gate | Mitigation | Success Criteria |
|---|---|---|---|---|
| Dependency mismatch | integration check fails | phase-audit-4 | reconcile contracts and retest | all phase validation commands pass, sonarqube-client → config (risk: 0.2) and gateway-registration → tools-issues (risk: 0.1) satisfied |
| Scope drift | non-phase changes appear | phase-audit-4 | update PLAN + re-scope before proceeding | only phase-scoped deliverables remain |
| Quality regression | validation gate fails | release quality gate | patch and rerun full checks | clean quality signal |

## Estimated Swarm Duration
- 6h swarm time