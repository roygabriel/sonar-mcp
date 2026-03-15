# Phase 2: Core UI Components Segment A Segment B

## Overview
Execute the second scoped segment of this workstream with deterministic outputs and validation.

## Why Now
This phase must precede later phases because it establishes full framework parity (OTEL tracing, structured logging with token redaction, health checks, stdio/HTTP transports, capability exporter) with cruvero-mcp-k8s@dev before SonarQube-specific logic is introduced. Implementing gateway-registration now mitigates downstream risk on the dependency edge gateway-registration → tools-issues (risk: 0.1) and ensures config patterns are stable for sonarqube-client → config (risk: 0.2) and helm-chart → config (risk: 0.3).

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P2-T01 | All framework components (OTEL tracing in internal/otel/tracer.go, structured logging with token redaction in internal/logging/redactor.go for sonarqube tokens, health checks in internal/health/server.go, stdio/HTTP transports in internal/transport/stdio.go and internal/transport/http.go, capability exporter in internal/capability/exporter.go) function identically to reference | PlatformArchitect | phase-1 audit pass, config package | implementation + evidence | `go test ./internal/... -run 'TestOTEL|TestLogging|TestHealth|TestTransport' && go vet ./internal/...` |
| P2-T02 | CI/CD pipeline produces container image, coverage meets reference threshold, and rollout to staging verifies tools are usable by downstream agents (validate gateway-registration in cmd/server/gateway.go) | PlatformArchitect | phase-1 audit pass, config package | implementation + evidence | `go test ./... && go vet ./... && curl -f http://localhost:8080/health && helm test cruvero-mcp-sonarqube --namespace dev` |

## Phase-Specific Prompt
```text
You are executing Phase 2: Core UI Components Segment A Segment B.
Overview: Execute the second scoped segment of this workstream with deterministic outputs and validation.
Scope nodes: gateway-registration
Required reading: docs/PLAN.md, docs/AUDIT/risk-matrix.md, docs/WORK_NOTES/memory-palace.md.
Output only phase-scoped changes with validation evidence and explicit assumptions.
Record every key decision in UTC under WORK_NOTES.
```

## Phase-Specific Audit Prompt
```text
Audit Phase 2: Core UI Components Segment A Segment B against acceptance criteria, risk controls, and deterministic behavior.
Return findings ordered by severity with explicit pass/fail gate decision and required remediations.
```

## Risks / Gates
| Risk | Trigger | Gate | Mitigation | Success Criteria |
|---|---|---|---|---|
| Dependency mismatch | integration check fails on sonarqube-client → config (risk: 0.2) or gateway-registration → tools-issues (risk: 0.1) | phase-audit-2 | reconcile contracts and retest | all phase validation commands pass |
| Scope drift | non-phase changes appear | phase-audit-2 | update PLAN + re-scope before proceeding | only phase-scoped deliverables remain |
| Quality regression | validation gate fails | release quality gate | patch and rerun full checks | clean quality signal |

## Exit Criteria Checklist
- [ ] All framework components in internal/ package match reference behavior (confirmed via `go test` and manual diff excluding SonarQube files)
- [ ] Validation commands execute cleanly with zero failures and coverage ≥ reference threshold
- [ ] Gateway registration in cmd/server/gateway.go exposes tools with correct JSON schemas, risk labels, and descriptions
- [ ] Helm chart in charts/cruvero-mcp-sonarqube/ deploys without errors using Vault-injected SONARQUBE_URL and SONARQUBE_TOKEN
- [ ] No breaking changes to existing deployment patterns or dependency edges

## Estimated Swarm Duration
- 7h swarm time