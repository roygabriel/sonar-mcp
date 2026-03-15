# Phase 4: Gateway Registration and Tool Exposure

## Overview
Register tools with MCP gateway. Why Now: Required before deployment and E2E validation per dependency edge gateway-registration → tools-issues (risk: 0.15).

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P4-T01 | Update gateway registration and tool schemas for both new tools | PlatformArchitect | phase-3 audit pass | internal/gateway/registration.go | `go test ./internal/gateway && go vet` |