# Phase 2: Search Issues Tool Implementation

## Overview
Implement the search_issues MCP tool on top of the client. Why Now: Establishes read capability and framework parity (OTEL, logging, health checks) before write operations.

## Files to Copy Verbatim from cruvero-mcp-k8s@dev
- internal/tools/tool.go (base tool scaffolding)
- internal/tools/mcp.go (schema registration helpers)

## MCP Tool JSON Schema (to be used in registration)
```json
{
  "name": "search_issues",
  "description": "Search SonarQube issues by projectKey and branch",
  "inputSchema": {
    "type": "object",
    "properties": {
      "projectKey": {"type": "string"},
      "branch": {"type": "string"},
      "types": {"type": "array", "items": {"type": "string"}},
      "severities": {"type": "array", "items": {"type": "string"}}
    },
    "required": ["projectKey", "branch"]
  }
}
```

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P2-T01 | Copy tool scaffolding and implement search_issues handler that calls sonarqube-client.SearchIssues | SoftwareEngineer | phase-1 audit pass | internal/tools/issues/search.go | `go test ./internal/tools -run TestSearchIssues -cover` |
| P2-T02 | Add OTEL tracing span around tool execution and structured logging (with token redaction) | SoftwareEngineer | P2-T01 | internal/tools/issues/search.go | `go test ./internal/tools -run TestOTEL` |
| P2-T03 | Implement error classification and MCP-compatible error responses for all edge cases listed in AC-01 | SoftwareEngineer | P2-T01 | internal/tools/issues/search.go | `go test ./internal/tools -run TestSearchIssues` |
| P2-T04 | Add unit tests exercising the full tool path with mocked client for happy path + all AC edge cases | SoftwareEngineer | P2-T03 | internal/tools/issues/search_test.go | `go test ./internal/tools -cover` |
| P2-T05 | Ensure tool description, risk classification (read-only), and hints match reference patterns | SoftwareEngineer | P2-T04 | internal/tools/issues/search.go | `go vet ./internal/tools` |

## Exit criteria checklist
- Tool passes all AC-01 validation including edge cases
- OTEL and logging behave identically to reference
- Coverage >=85% on modified packages