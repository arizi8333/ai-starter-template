# 🧩 BACKEND RULES (STRICT - NON-NEGOTIABLE)

---

# 🧠 ARCHITECTURE RULES

- handler MUST NOT contain business logic
- handler MUST only handle request/response
- handler MUST call service layer

- service MUST contain business logic
- service MUST orchestrate repository calls

- repository MUST only handle database operations
- repository MUST NOT contain business logic

- MUST follow clean architecture layering strictly

---

# 🔄 TRANSACTION RULES

- ALL database write operations MUST use transaction manager
- transaction MUST be implemented in service layer ONLY
- transaction MUST NOT exist in handler
- transaction MUST NOT exist in repository

- repository MUST use transaction from context if available

---

# 🧠 CONTEXT RULES

- context MUST be passed from handler → service → repository
- context MUST NOT be recreated arbitrarily
- context MUST propagate:
  - request_id
  - tracing data

---

# 📊 OBSERVABILITY RULES

## Logging
- MUST use structured logging (JSON)
- MUST include request_id in all logs
- MUST log all errors with context
- MUST NOT log sensitive data

## Metrics
- critical operations SHOULD expose metrics

---

# ⚡ APM & TRACING RULES

- APM tracing MUST start at handler level
- tracing MUST propagate to service & repository
- each request MUST be traceable end-to-end

---

# 🧪 TESTING RULES

- every service MUST have unit tests
- MUST test:
  - success case
  - failure case
- integration test REQUIRED for endpoints

- test MUST pass before feature completion

---

# 🧪 TESTING STANDARDS (ENTERPRISE)

## Unit Testing
- every service method MUST have unit tests
- MUST test: success case, failure case, edge cases, boundary values
- MUST use table-driven tests for multiple input scenarios (Go convention)
- MUST mock external dependencies (database, HTTP clients, message queues)
- MUST NOT depend on external services or network
- minimum coverage target: 80% for service layer
- test naming convention: `Test{FunctionName}_{Scenario}_{ExpectedResult}`

## Integration Testing
- every API endpoint MUST have integration tests
- MUST test full request-response cycle (handler → service → repository)
- MUST use test database (separate from development)
- MUST setup and teardown test data per test (no shared state between tests)
- MUST test authentication and authorization flows
- MUST test error responses (400, 401, 403, 404, 500)
- MUST validate response schema matches API contract

## Benchmark Testing
- critical path operations MUST have benchmark tests
- database queries on large datasets MUST be benchmarked
- serialization/deserialization of large payloads MUST be benchmarked
- benchmark naming convention: `Benchmark{FunctionName}_{Scenario}`
- MUST establish baseline metrics and detect regressions
- benchmark results SHOULD be tracked over time
- MUST run benchmarks with realistic data volumes

## Load Testing
- production-critical endpoints MUST have load test scenarios
- MUST define expected throughput (requests per second) per endpoint
- MUST define acceptable latency thresholds (p50, p95, p99)
- MUST test under normal load, peak load, and stress conditions
- MUST identify breaking point (max concurrent users/requests)
- MUST test with realistic user patterns (not just single endpoint)
- load test tools: k6, Locust, or equivalent
- MUST run load tests before major releases
- MUST NOT run load tests against production environment

## Test Organization
- test files MUST be co-located with source files (Go convention: `_test.go`)
- test fixtures MUST be in `testdata/` directory
- shared test helpers MUST be in `testutil/` or `testhelper/` package
- MUST NOT use production data in tests
- MUST NOT hardcode test credentials — use test fixtures or environment variables

## Test in CI/CD
- unit tests MUST run on every commit
- integration tests MUST run on every PR
- benchmark tests SHOULD run on PR (with regression detection)
- load tests MUST run before production deployment (staging environment)
- test failure MUST block merge/deployment

---

# 📜 API & SWAGGER RULES

- every endpoint MUST have swagger documentation
- request/response MUST match contract
- MUST NOT break existing API

---

# 🔐 SECURITY RULES

- all input MUST be validated
- MUST NOT trust client input
- MUST NOT log sensitive information
- MUST use safe query methods (no raw SQL injection)

---

# 📈 PERFORMANCE RULES

- MUST NOT use SELECT *
- MUST use pagination for list endpoints
- MUST avoid N+1 queries
- SHOULD suggest index for heavy queries

---

# 📦 DTO RULES

- handler MUST use DTO for request/response
- MUST NOT expose domain model directly
- mapping MUST be explicit

---

# 🚨 ENFORCEMENT RULE

IF ANY RULE IS VIOLATED:
→ QA MUST FAIL
→ FEATURE MUST BE REJECTED
→ MUST RETURN TO DEV FOR FIX

---

# === ENTERPRISE ENHANCEMENT ===

---

# 📋 STRUCTURED LOGGING STANDARDS

## Log Format
- MUST use JSON structured format for all logs
- MUST include standard fields in every log entry:
  - `timestamp` (ISO 8601 format)
  - `level` (DEBUG, INFO, WARN, ERROR, FATAL)
  - `service` (service name)
  - `request_id` (correlation ID)
  - `trace_id` (distributed tracing ID)
  - `message` (human-readable description)
- optional fields: `user_id`, `endpoint`, `duration_ms`, `status_code`

## Log Levels
- DEBUG: detailed diagnostic info (disabled in production)
- INFO: normal operations (request received, process completed)
- WARN: unexpected but recoverable situations (retry, fallback)
- ERROR: operation failed but service continues (failed request, DB error)
- FATAL: service cannot continue (startup failure, critical dependency down)
- MUST NOT use INFO for errors
- MUST NOT use ERROR for expected business logic outcomes

## Sensitive Data Masking
- MUST mask PII in logs (email, phone, address, name)
- MUST NOT log passwords, tokens, API keys, or secrets
- MUST NOT log full credit card numbers or SSN
- MUST mask to format: `***masked***` or partial (e.g., `****1234`)

---

# 🔗 CORRELATION & TRACING STANDARDS

## Correlation ID
- every incoming request MUST generate or propagate a `request_id`
- `request_id` MUST be passed through all layers (handler → service → repository)
- `request_id` MUST be included in all log entries
- `request_id` MUST be returned in response headers (X-Request-ID)

## Distributed Tracing
- `trace_id` and `span_id` MUST be propagated across service boundaries
- MUST support W3C Trace Context or OpenTelemetry propagation format
- every outgoing HTTP/gRPC call MUST propagate trace context

---

# 📝 AUDIT LOGGING

- security-sensitive operations MUST produce audit log entries
- audit log MUST include: `who` (user_id), `what` (action), `when` (timestamp), `where` (endpoint/service), `result` (success/failure)
- audit events: login, logout, permission change, data access, data modification, admin actions
- audit logs MUST be stored separately from application logs
- audit logs MUST NOT be deletable by application

---

# 🔄 ERROR TRACKING & REPORTING

- unhandled errors MUST be reported to error tracking system (Sentry or equivalent)
- error reports MUST include: stack trace, request context, user context (anonymized), environment
- MUST NOT send sensitive data to error tracking
- MUST deduplicate repeated errors
- MUST set alerting thresholds for error rate spikes

---

# 🏥 HEALTH CHECK & READINESS STANDARDS

## Health Endpoints
- MUST expose `/health` endpoint (liveness — is the service running?)
- MUST expose `/ready` endpoint (readiness — is the service ready to accept traffic?)
- MUST expose `/metrics` endpoint (Prometheus-compatible metrics)

## Health Check Rules
- `/health` MUST return 200 if service process is alive
- `/ready` MUST check critical dependencies (database, cache, external services)
- `/ready` MUST return 503 if any critical dependency is unavailable
- health endpoints MUST NOT require authentication
- health endpoints MUST respond within 5 seconds

---

# 🛑 GRACEFUL SHUTDOWN

- service MUST handle SIGTERM and SIGINT signals
- on shutdown signal, service MUST:
  - stop accepting new requests
  - complete in-flight requests (with timeout)
  - close database connections cleanly
  - flush pending logs and metrics
  - exit with code 0
- shutdown timeout MUST be configurable (default: 30 seconds)
- MUST NOT force-kill active connections

---

# 🏷️ APPLICATION VERSIONING

- MUST follow semantic versioning (MAJOR.MINOR.PATCH)
- MUST expose `/version` or include version in `/health` response
- version info MUST include: version number, build timestamp, git commit hash
- MUST NOT deploy without version tag
- MUST update version on every release

---

# 📈 LOG RETENTION & ROTATION

- application logs MUST have rotation policy (max file size or time-based)
- log retention: minimum 30 days for application logs, minimum 90 days for audit logs
- production logs MUST be shipped to centralized log aggregation (ELK, Loki, CloudWatch)
- MUST NOT store logs only on local disk in production