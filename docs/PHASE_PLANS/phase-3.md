# Phase 3: Transition Issue Tool Implementation

## Overview
Implement the transition_issue tool with support for resolve/wontfix/falsepositive. Why Now: Delivers write capability after read tool is stable.

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P3-T01 | Implement TransitionIssue func and tool with proper error handling | SoftwareEngineer | phase-2 audit pass | internal/tools/issues/transition.go | `go test ./internal/tools -run TestTransitionIssue && go vet` |