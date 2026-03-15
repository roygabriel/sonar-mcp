# Phase 4: Gateway Registration and Framework Parity

## Overview
Register tools with MCP gateway. Why Now: Required before deployment and E2E validation per dependency edge gateway-registration → tools-issues (risk: 0.15).

## Files to Copy Verbatim from cruvero-mcp-k8s@dev
- internal/gateway/registration.go (update only tool list and schemas)

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P4-T01 | Update gateway registration to expose both search_issues and transition_issue with exact JSON schemas, descriptions, risk classification (read-only vs write) and hints from reference | PlatformArchitect | phase-3 audit pass | internal/gateway/registration.go | `go test ./internal/gateway && go vet ./internal/gateway` |
| P4-T02 | Verify OTEL, logging, healthcheck, transport and capability exporter components are identical to k8s reference | PlatformArchitect | P4-T01 | internal/gateway/registration.go | `go test ./internal/... -run 'TestOTEL|TestLogging|TestHealth|TestTransport'` |
| P4-T03 | Add E2E-style test to confirm both tools are registered with correct metadata | PlatformArchitect | P4-T02 | internal/gateway/registration_test.go | `go test ./internal/gateway -run TestRegistration` |
| P4-T04 | Ensure no k8s-specific strings remain in registration or tool descriptions | PlatformArchitect | P4-T03 | internal/gateway/registration.go | `go test ./internal/gateway` |

## Exit criteria checklist
- Both tools correctly registered and discoverable by MCP gateway
- AC-03 fully satisfied with framework parity
- All validation commands produce clean output