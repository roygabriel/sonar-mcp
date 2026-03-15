# Phase 1: SonarQube Client and Configuration Setup

## Overview
Implement the foundational sonarqube client package (forked from cruvero-mcp-k8s@dev) and configuration. Why Now: This phase must precede all later phases because it resolves the root dependency edges sonarqube-client → config (risk: 0.2) and helm-chart → config (risk: 0.3).

## Exit criteria checklist
- All new code in pkg/sonarqube/client.go passes `go test` and `go vet` with zero errors
- Config injects SONARQUBE_URL/SONARQUBE_TOKEN from Vault with no diff against reference pattern
- Phase audit confirms coverage of acceptance criteria in scope

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P1-T01 | Implement SearchIssues func in pkg/sonarqube/client.go that queries /api/issues/search filtered by projectKey, branch, types and severities | SoftwareEngineer | sonarqube-client → config (risk: 0.2) | pkg/sonarqube/client.go | `go test ./pkg/sonarqube -run TestSearchIssues && go vet ./pkg/sonarqube` |