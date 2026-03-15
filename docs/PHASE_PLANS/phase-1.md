# Phase 1: Core UI Components Segment A Segment A

## Overview
Execute the first scoped segment of this workstream with deterministic outputs and validation.

This phase establishes the foundational sonarqube-client package (forked from cruvero-mcp-k8s@dev) and initial Helm chart updates. Why Now: This phase must precede all later phases because it resolves the root dependency edges sonarqube-client → config (risk: 0.2) and helm-chart → config (risk: 0.3); without these, tools-issues → sonarqube-client (risk: 0.1) and gateway-registration → tools-issues (risk: 0.1) cannot be implemented.

Exit criteria checklist:
- All new code in pkg/sonarqube/client.go and pkg/tools/issues/search.go passes `go test` and `go vet` with zero errors
- Helm dry-run succeeds and injects SONARQUBE_URL/SONARQUBE_TOKEN from Vault with no diff against reference k8s pattern
- Phase-specific validation commands execute cleanly and produce expected structured output
- Phase audit confirms 100% coverage of the two acceptance criteria in scope
- WORK_NOTES contains UTC-timestamped decisions referencing the four dependency edges

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P1-T01 | Implement search_issues in pkg/tools/issues/search.go (using SearchIssues func and NewClient from pkg/sonarqube/client.go) that queries SonarQube REST API /api/issues/search filtered by projectKey, branch, types and severities returning structured results matching API shape | SoftwareEngineer | sonarqube-client → config (risk: 0.2) | pkg/sonarqube/client.go, pkg/tools/issues/search.go + tests | `go test ./pkg/tools/issues -run=^TestSearchIssues$ -v && go vet ./pkg/sonarqube` |
| P1-T02 | Helm chart deploys successfully to dev environment with SonarQube URL/token injected via Vault secrets (update charts/cruvero-mcp-sonarqube/values.yaml, templates/deployment.yaml and secrets.yaml); no breaking changes to existing deployment patterns | SoftwareEngineer | helm-chart → config (risk: 0.3) | updated Helm chart artifacts + evidence | `helm lint charts/cruvero-mcp-sonarqube && helm template charts/cruvero-mcp-sonarqube \| kubectl --dry-run=client apply -f - && curl -s http://localhost:8080/health` |

## Phase-Specific Prompt
```text
You are executing Phase 1: Core UI Components Segment A Segment A.
Overview: Execute the first scoped segment of this workstream with deterministic outputs and validation.
Scope nodes: sonarqube-client, config
Required reading: docs/PLAN.md, docs/AUDIT/risk-matrix.md, docs/WORK_NOTES/memory-palace.md.
Output only phase-scoped changes with validation evidence and explicit assumptions.
Record every key decision in UTC under WORK_NOTES.
```

## Phase-Specific Audit Prompt
```text
Audit Phase 1: Core UI Components Segment A Segment A against acceptance criteria, risk controls, and deterministic behavior.
Return findings ordered by severity with explicit pass/fail gate decision and required remediations.
```

## Risks / Gates
| Risk | Trigger | Gate | Mitigation | Success Criteria |
|---|---|---|---|---|
| Dependency mismatch | integration check fails on sonarqube-client → config (risk: 0.2) | phase-audit-1 | reconcile contracts and retest | all phase validation commands pass |
| Scope drift | non-phase changes appear | phase-audit-1 | update PLAN + re-scope before proceeding | only phase-scoped deliverables remain |
| Quality regression | validation gate fails | release quality gate | patch and rerun full checks | clean quality signal |

## Estimated Swarm Duration
- 7h swarm time