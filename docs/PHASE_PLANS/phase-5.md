# Phase 5: Helm Chart, Deployment and Final Validation

## Overview
Finalize Helm chart and perform E2E validation. Why Now: Completes deployment after all code and registration is complete.

## Files to Copy Verbatim from cruvero-mcp-k8s@dev
- charts/cruvero-mcp-k8s/* (rename and adapt for sonarqube)

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P5-T01 | Update Helm chart values.yaml and templates/deployment.yaml to inject SonarQube URL/token via Vault secrets (no breaking changes to patterns) | PlatformArchitect | phase-4 audit pass | charts/cruvero-mcp-sonarqube/values.yaml, charts/cruvero-mcp-sonarqube/templates/deployment.yaml | `helm template charts/cruvero-mcp-sonarqube` |
| P5-T02 | Verify deployment manifests contain correct SONARQUBE_ env vars and sidecar configurations for OTEL | PlatformArchitect | P5-T01 | charts/cruvero-mcp-sonarqube/* | `helm template ... | grep -E 'SONARQUBE|OTEL'` |
| P5-T03 | Run full test suite and E2E validation against dev SonarQube instance confirming both tools | PlatformArchitect | P5-T02 | - | `go test ./... -run=TestSearchIssues|TestTransitionIssue && go vet ./...` |
| P5-T04 | Confirm CI/CD produces container image and no secrets are leaked in logs or errors | PlatformArchitect | P5-T03 | - | `go test ./... -cover` |

## Exit criteria checklist
- Helm chart deploys successfully to dev with secrets injected
- All ACs validated end-to-end
- Residual risks documented