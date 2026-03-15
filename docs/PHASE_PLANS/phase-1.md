# Phase 1: SonarQube Client and Configuration Setup

## Overview
Implement the foundational sonarqube client package (forked from cruvero-mcp-k8s@dev) and configuration. Why Now: This phase must precede all later phases because it resolves the root dependency edges sonarqube-client → config (risk: 0.2) and helm-chart → config (risk: 0.3).

## Files to Copy Verbatim from cruvero-mcp-k8s@dev
- pkg/config/config.go (update env var names to SONARQUBE_URL / SONARQUBE_TOKEN)
- internal/logging/redaction.go (token redaction regex)
- internal/otel/setup.go (tracing provider)
- internal/errors/classifier.go (error classification)

## Exact Function Signatures to Implement
```go
// pkg/sonarqube/client.go
type Client struct {
    httpClient *http.Client
    baseURL    string
    token      string
}

func NewClient(cfg *config.Config) (*Client, error)

type SearchRequest struct {
    ProjectKey string   `json:"projectKey"`
    Branch     string   `json:"branch"`
    Types      []string `json:"types,omitempty"`
    Severities []string `json:"severities,omitempty"`
}

func (c *Client) SearchIssues(ctx context.Context, req SearchRequest) ([]Issue, error)

func (c *Client) TransitionIssue(ctx context.Context, issueKey, transition string) error
```

## Detailed Task List
| Task ID | Task | Owner/Agent | Depends On | Deliverable | Validation Command |
|---|---|---|---|---|---|
| P1-T01 | Copy config package verbatim and adapt for SONARQUBE_URL/SONARQUBE_TOKEN + Vault injection | SoftwareEngineer | - | pkg/config/config.go | `go test ./pkg/config -cover && go vet ./pkg/config` |
| P1-T02 | Create pkg/sonarqube/client.go with NewClient, base URL + token setup, OTEL span for client creation | SoftwareEngineer | P1-T01 | pkg/sonarqube/client.go | `go test ./pkg/sonarqube -run TestNewClient` |
| P1-T03 | Implement SearchIssues: call /api/issues/search with query params (projectKeys, branch, types, severities), parse response into Issue structs matching SonarQube API shape, add OTEL tracing + structured logging | SoftwareEngineer | P1-T02 | pkg/sonarqube/client.go | `go test ./pkg/sonarqube -run TestSearchIssues -cover` |
| P1-T04 | Implement TransitionIssue: POST to /api/issues/do_transition with parameters, classify 400/404 responses, redact token in all logs | SoftwareEngineer | P1-T02 | pkg/sonarqube/client.go | `go test ./pkg/sonarqube -run TestTransitionIssue` |
| P1-T05 | Add unit tests with httptest server covering AC-01 edge cases (missing project, network error, invalid token) and >=85% coverage | SoftwareEngineer | P1-T04 | pkg/sonarqube/client_test.go | `go test ./pkg/sonarqube -coverprofile=coverage.out && go tool cover -func=coverage.out` |
| P1-T06 | Verify token redaction and OTEL propagation identical to reference implementation | SoftwareEngineer | P1-T05 | - | `go test ./internal/otel ./internal/logging -run TestRedaction` |

## Exit criteria checklist
- All new code in pkg/sonarqube/* passes `go test ./pkg/... -cover` (>=85%) and `go vet ./pkg/...` with zero errors
- Config injects SONARQUBE_URL/SONARQUBE_TOKEN from Vault with no diff against reference pattern
- Phase audit confirms coverage of acceptance criteria in scope