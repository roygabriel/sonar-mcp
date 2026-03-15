# Risk Matrix

| Risk | Likelihood | Impact | Trigger | Mitigation | Gate | Owner |
|---|---|---|---|---|---|---|
| SonarQube REST API integration drift in search_issues tool | Medium | High | API response shape changes or auth failures | Mirror k8s client error handling; add contract tests with real payloads; enforce token redaction; monitor OTEL traces | component-test-gate | SoftwareEngineer |
| Framework component divergence (OTEL, logging, healthchecks) from k8s reference impl | Low | High | Manual fork edits instead of importing shared modules | Copy exact OTEL setup and redaction regex from reference; run differential test suite; include healthcheck e2e test | phase-audit-2 | PlatformArchitect |
| transition_issue tool state machine errors with SonarQube workflow | Medium | High | Invalid transition attempted or 400 response | Pre-flight status lookup before transition; explicit error handling; add test cases per transition type | phase-audit-3 | SoftwareEngineer |