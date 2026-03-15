# Phase 3: Transition Issue Tool Implementation

## Overview
Implement the transition_issue tool with support for resolve/wontfix/falsepositive. Why Now: Delivers write capability after read tool is stable.

## Files to Copy Verbatim from cruvero-mcp-k8s@dev
- internal/tools/tool.go (base scaffolding)

## MCP Tool JSON Schema (to be used in registration)
```json
{
  "name": "transition_issue",
  "description": "Transition a SonarQube issue to resolved, wontfix or falsepositive",
  "inputSchema": {
    "type": "object",
    "properties": {
      "issueKey": {"type": "string"},
      "transition": {"type": "string", "enum": ["resolve", "wontfix", "falsepositive"]}
    },
    "required": ["issueKey", "transition"]
  }
}
```

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P3-T01 | Implement transition_issue handler calling client.TransitionIssue with proper transition mapping | SoftwareEngineer | phase-2 audit pass | internal/tools/issues/transition.go | `go test ./internal/tools -run TestTransitionIssue -cover` |
| P3-T02 | Integrate OTEL tracing, structured logging and token redaction for write operations | SoftwareEngineer | P3-T01 | internal/tools/issues/transition.go | `go test ./internal/tools -run TestOTEL|TestLogging` |
| P3-T03 | Add pre-flight status check and explicit error handling for invalid state transitions per AC-02 | SoftwareEngineer | P3-T01 | internal/tools/issues/transition.go | `go test ./internal/tools -run TestTransitionIssue` |
| P3-T04 | Write tests covering all allowed transitions + error paths (already resolved, bad issueKey, 400 responses) | SoftwareEngineer | P3-T03 | internal/tools/issues/transition_test.go | `go test ./internal/tools -coverprofile=coverage.out` |
| P3-T05 | Classify risks and ensure write operation is marked with appropriate risk level in metadata | SoftwareEngineer | P3-T04 | internal/tools/issues/transition.go | `go vet ./internal/tools` |

## Exit criteria checklist
- All AC-02 edge cases tested and passing
- No secret leakage in error messages
- Coverage and vet checks pass