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

## Project Structure (Monorepo)
- backend and frontend MUST be in separate root directories
- backend: `backend/` (contains cmd/, internal/, pkg/, migrations/, go.mod, Makefile, Dockerfile)
- frontend: `frontend/` (contains src/, package.json, tsconfig.json, Dockerfile)
- shared configs (docker-compose, CI/CD) at project root
- MUST NOT mix backend Go code with frontend TypeScript/JavaScript code
- each layer (backend/frontend) MUST be independently buildable and deployable
- project structure:
  ```
  project-root/
  ├── backend/
  │   ├── cmd/server/main.go
  │   ├── internal/ (handler, service, repository, dto, model, middleware, pkg)
  │   ├── pkg/
  │   ├── migrations/ (SQL + Go migration files)
  │   ├── go.mod
  │   ├── go.sum
  │   ├── Makefile
  │   └── Dockerfile
  ├── frontend/
  │   ├── src/ (app, components, services, types, utils)
  │   ├── package.json
  │   ├── tsconfig.json
  │   └── Dockerfile
  ├── reports/ (gitignored — auto-generated test reports)
  │   ├── backend/ (coverage.out, coverage.html, coverage-summary.txt, test-results.json)
  │   ├── frontend/ (test results, coverage)
  │   ├── testcases/ ({feature-name}-testcases.md — testcase documents)
  │   └── e2e/ (execution reports, screenshots/, captures/)
  ├── docker-compose.yml
  ├── kiro/ (blueprint)
  ├── README.md
  └── project_intake.yaml
  ```

---

# 🔄 TRANSACTION RULES

- ALL database write operations MUST use transaction manager
- transaction MUST be implemented in service layer ONLY
- transaction MUST NOT exist in handler
- transaction MUST NOT exist in repository

- repository MUST use transaction from context if available

## Transaction Manager Interface
- MUST define a `TransactionManager` interface in the service layer or shared package
- interface MUST have at minimum: `WithTransaction(ctx context.Context, fn func(ctx context.Context) error) error`
- service MUST use the interface, NOT concrete database implementation
- repository MUST extract transaction from context using a helper function
- transaction manager MUST support nested transaction detection (prevent double-begin)
- transaction manager MUST handle rollback on panic
- example interface (Go):
  ```
  type TransactionManager interface {
      WithTransaction(ctx context.Context, fn func(ctx context.Context) error) error
  }
  ```
- example usage in service:
  ```
  func (s *OrderService) CreateOrder(ctx context.Context, req CreateOrderRequest) error {
      return s.txManager.WithTransaction(ctx, func(txCtx context.Context) error {
          // all repository calls use txCtx — transaction is implicit
          order, err := s.orderRepo.Create(txCtx, ...)
          if err != nil { return err }
          return s.paymentRepo.Create(txCtx, ...)
      })
  }
  ```

---

# 🧠 CONTEXT RULES

- context MUST be passed from handler → service → repository
- context MUST NOT be recreated arbitrarily
- context MUST propagate:
  - request_id
  - tracing data

## Context Cancellation
- all long-running operations MUST respect context cancellation
- database queries MUST use context-aware methods (e.g., `db.WithContext(ctx)`)
- HTTP client calls MUST pass context (e.g., `http.NewRequestWithContext(ctx, ...)`)
- goroutines MUST receive parent context and stop when cancelled
- MUST NOT ignore `ctx.Done()` in loops or blocking operations
- timeout MUST be set on all external calls (HTTP, gRPC, DB) — default 30 seconds
- MUST use `context.WithTimeout` or `context.WithDeadline` for operations with SLA

---

# 🛡️ RESILIENCE PATTERNS

## Circuit Breaker
- external service calls (HTTP, gRPC, third-party API) MUST use circuit breaker pattern
- circuit breaker MUST have three states: closed (normal), open (failing), half-open (testing recovery)
- thresholds MUST be configurable: failure count, timeout duration, half-open max requests
- WHEN circuit is open, MUST return fallback response or error immediately (no waiting)
- MUST log circuit state changes (closed→open, open→half-open, half-open→closed)
- recommended library: `sony/gobreaker` or equivalent

## Retry with Backoff
- transient failures (network timeout, 503, connection refused) MUST be retried
- retry MUST use exponential backoff with jitter (prevent thundering herd)
- max retry attempts MUST be configurable (default: 3)
- MUST NOT retry on permanent errors (400, 401, 403, 404, 422)
- MUST NOT retry on context cancellation or deadline exceeded

## Timeout Management
- every external call MUST have explicit timeout
- database queries: max 5 seconds (configurable)
- HTTP client calls: max 30 seconds (configurable)
- gRPC calls: max 10 seconds (configurable)
- MUST propagate remaining timeout from parent context to child calls
- MUST NOT use infinite timeout in production

## Rate Limiting
- public API endpoints MUST have rate limiting
- rate limit MUST be configurable per endpoint or per user/API key
- MUST return HTTP 429 (Too Many Requests) with `Retry-After` header when limit exceeded
- rate limit state SHOULD be stored in Redis for distributed systems

## Bulkhead Pattern
- critical services MUST be isolated from non-critical services
- MUST use separate connection pools for different external dependencies
- MUST use separate goroutine pools or semaphores for heavy operations
- failure in one dependency MUST NOT cascade to others

## Fallback Strategy
- WHEN external service is unavailable, MUST provide fallback:
  - cached response (if applicable)
  - degraded response (partial data)
  - default response (safe defaults)
  - error response with clear message
- MUST NOT return raw error from external service to client

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

## Test Reporting & Documentation
- test results MUST be stored in `reports/backend/` directory (at project root)
- frontend test results MUST be stored in `reports/frontend/` directory
- report files generated automatically by test hook:
  - `reports/backend/test-results.json` — full test output in JSON format (go test -json)
  - `reports/backend/coverage.out` — raw coverage profile
  - `reports/backend/coverage-summary.txt` — coverage per function summary
  - `reports/backend/coverage.html` — visual HTML coverage report
- reports directory is gitignored — regenerated on every test run
- coverage report MUST be generated on every test run (`go test -coverprofile`)
- Makefile MUST have targets:
  - `make test` — run all unit tests with coverage
  - `make test-integration` — run integration tests
  - `make test-coverage` — generate HTML coverage report
  - `make test-report` — generate test result report (JSON/JUnit)
- test report MUST include: total tests, passed, failed, skipped, duration, coverage percentage
- coverage percentage MUST be tracked over time (compare with previous run)
- MUST fail CI if coverage drops below threshold (default: 80%)
- test results SHOULD be published as PR comment or CI artifact
- failed test MUST include: test name, error message, expected vs actual, file location

## Test Automation Standards
- tests MUST run automatically:
  - unit tests: on every file save (via hook) and every commit
  - integration tests: on every PR
  - E2E tests: on PR to main/production branch
  - load tests: before production deployment
  - benchmark tests: on PR (with regression comparison)
- test environment MUST be isolated (separate DB, separate config)
- test data MUST be created and cleaned up per test (no shared state)
- flaky tests MUST be marked and fixed within 1 sprint — MUST NOT be ignored
- test timeout: unit test max 10 seconds, integration test max 60 seconds

---

# 📜 API & SWAGGER RULES

- every endpoint MUST have swagger documentation
- request/response MUST match contract
- MUST NOT break existing API

## Swagger Annotation Standards (Go — swaggo/swag)
- every handler function MUST have swagger annotations as comments above the function
- annotations MUST include at minimum:
  - `@Summary` — short description
  - `@Description` — detailed description
  - `@Tags` — API group/category
  - `@Accept` — request content type (json)
  - `@Produce` — response content type (json)
  - `@Param` — for each parameter (path, query, body)
  - `@Success` — success response with status code and schema
  - `@Failure` — error responses (400, 401, 403, 404, 500)
  - `@Router` — endpoint path and HTTP method
  - `@Security` — auth requirement (if applicable)
- example:
  ```
  // CreateOrder handles POST request to create a new order
  // @Summary Create a new order
  // @Description Create a new order with the provided details
  // @Tags orders
  // @Accept json
  // @Produce json
  // @Param request body dto.CreateOrderRequest true "Order creation request"
  // @Success 201 {object} dto.CreateOrderResponse "Order created successfully"
  // @Failure 400 {object} dto.ErrorResponse "Invalid request"
  // @Failure 401 {object} dto.ErrorResponse "Unauthorized"
  // @Failure 500 {object} dto.ErrorResponse "Internal server error"
  // @Security BearerAuth
  // @Router /api/v1/orders [post]
  ```
- swagger docs MUST be auto-generated using `swag init` or equivalent
- swagger generation MUST be part of the build process (Makefile target: `make swagger`)
- generated swagger files MUST be committed to repository
- swagger UI MUST be accessible at `/swagger/` endpoint in development and staging

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
  - stop all background workers gracefully
  - drain message queue consumers
  - close database connections cleanly
  - flush pending logs and metrics
  - exit with code 0
- shutdown timeout MUST be configurable (default: 30 seconds)
- MUST NOT force-kill active connections
- background workers MUST finish current job before stopping (or timeout)

---

# ⚙️ BACKGROUND PROCESS RULES

## Worker Pattern
- background workers MUST be implemented as separate goroutines with lifecycle management
- every worker MUST accept context for cancellation
- worker MUST stop gracefully when context is cancelled
- worker MUST log start, stop, and error events
- worker MUST NOT panic — recover and log errors
- worker struct MUST implement interface: `Start(ctx context.Context) error` and `Stop() error`

## Message Queue Consumer
- consumer MUST acknowledge message ONLY after successful processing
- consumer MUST use dead-letter queue (DLQ) for failed messages after max retries
- consumer MUST be idempotent — processing same message twice MUST produce same result
- consumer MUST include message ID and correlation ID in logs
- consumer MUST respect context cancellation (stop consuming on shutdown)
- consumer MUST handle poison messages (malformed data) without crashing
- max retry per message MUST be configurable (default: 3)

## Cron / Scheduled Jobs
- scheduled jobs MUST use distributed lock to prevent duplicate execution in multi-instance deployment
- lock MUST be stored in Redis or database (not in-memory)
- job MUST have configurable schedule (cron expression from config)
- job MUST log execution start, duration, and result
- job MUST have timeout — MUST NOT run indefinitely
- job MUST be idempotent (safe to re-run if previous execution was interrupted)
- MUST NOT use `time.Sleep` loops — use proper scheduler (e.g., `robfig/cron`)

## Event / Pub-Sub
- event publisher MUST NOT block the main request flow (publish async or use buffer)
- event MUST include: event_type, payload, timestamp, correlation_id, source_service
- subscriber MUST be idempotent
- subscriber MUST handle out-of-order events gracefully
- MUST define event schema/contract for each event type
- failed event processing MUST be retried with backoff or sent to DLQ

## Background Job Queue
- long-running tasks (email sending, report generation, file processing) MUST be offloaded to background job queue
- MUST NOT process heavy tasks in HTTP request handler
- job MUST have status tracking: pending → processing → completed / failed
- job MUST have timeout and max retry
- job result MUST be queryable (by job ID)
- MUST support job priority (high, normal, low)

## Worker Health & Monitoring
- every background worker MUST expose health status (running/stopped/error)
- worker health MUST be included in `/ready` endpoint check
- worker errors MUST be logged and reported to error tracking
- worker metrics MUST be exposed: jobs processed, jobs failed, processing duration, queue depth
- MUST alert when worker is stuck (no progress for configurable duration)

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