# 🧪 QA PLANNING RULES (STRICT - NON-NEGOTIABLE)

---

# 🧠 CORE RESPONSIBILITY

QA Planning MUST:
- create comprehensive test plan BEFORE implementation begins
- define testcases for every feature based on acceptance criteria
- ensure all test types are covered (API, UI, performance, security, database)
- produce testcase document as deliverable
- track testcase execution and results

---

# 📋 TEST PLAN STRUCTURE

Every feature MUST have a test plan that includes:

## 1. Feature Overview
- feature name and ID
- description (what the feature does)
- acceptance criteria (from PM agent)
- dependencies (other features, external services)
- test environment requirements

## 2. Test Scope
- what is IN scope (features/endpoints/pages to test)
- what is OUT of scope (with justification)
- assumptions and constraints

## 3. Test Types Required
- API Testing (backend endpoints)
- UI Testing (frontend user flows) — if frontend enabled
- Integration Testing (cross-service, cross-feature)
- Performance Testing (load, stress, benchmark)
- Security Testing (auth, authorization, injection, XSS)
- Database Testing (migration, data integrity, query performance)

---

# 📝 TESTCASE DESIGN RULES

## Testcase Format
Every testcase MUST follow this structure:
- TC-ID: unique identifier (format: TC-{FEATURE}-{TYPE}-{NUMBER}, e.g., TC-AUTH-API-001)
- Title: short descriptive title
- Type: api | ui | integration | performance | security | database
- Priority: critical | high | medium | low
- Preconditions: what must be true before test runs
- Test Steps: numbered steps with action and expected result
- Test Data: specific data used in the test
- Expected Result: what should happen if test passes
- Postconditions: cleanup or state after test

## Testcase Coverage Rules
- every API endpoint MUST have at minimum:
  - 1 happy path testcase
  - 1 invalid input testcase (400)
  - 1 unauthorized testcase (401) — if auth required
  - 1 forbidden testcase (403) — if role-based
  - 1 not found testcase (404)
  - 1 server error scenario
- every UI page MUST have at minimum:
  - 1 happy path user journey
  - 1 form validation testcase
  - 1 error state testcase
  - 1 empty state testcase
- every database migration MUST have:
  - 1 migration up testcase
  - 1 migration down (rollback) testcase
  - 1 data integrity testcase

## Testcase Naming Convention
- API: TC-{FEATURE}-API-{NUMBER} (e.g., TC-AUTH-API-001)
- UI: TC-{FEATURE}-UI-{NUMBER} (e.g., TC-AUTH-UI-001)
- Integration: TC-{FEATURE}-INT-{NUMBER}
- Performance: TC-{FEATURE}-PERF-{NUMBER}
- Security: TC-{FEATURE}-SEC-{NUMBER}
- Database: TC-{FEATURE}-DB-{NUMBER}

---

# 🏃 TEST EXECUTION RULES

## Execution Order
Tests MUST be executed in this order:
1. Database tests (migration, schema)
2. API tests (backend endpoints)
3. Integration tests (cross-feature)
4. UI tests (frontend flows)
5. Security tests (auth, injection)
6. Performance tests (load, stress)

## Step-by-Step Execution
- every testcase MUST be executed step by step
- each step MUST capture:
  - step number
  - action performed
  - expected result
  - actual result
  - status: PASS | FAIL | SKIP | BLOCKED
  - duration (ms)
  - evidence: screenshot (UI) | API response dump (API) | DB query result (DB) | metrics (performance)
- failed step MUST include:
  - error message
  - stack trace or response body
  - screenshot (if UI)
  - suggestion for fix

## Capture Requirements
- EVERY STEP in EVERY testcase MUST have its own individual capture — NO EXCEPTIONS
- MUST NOT use 1 capture for entire testcase — each step gets its own evidence file
- capture naming: {TC-ID}-step-{N}-{type}.{ext} (e.g., TC-AUTH-API-001-step-1-request.json)
- API tests — EVERY step MUST capture:
  - request file: {TC-ID}-step-{N}-request.json (method, URL, headers, body)
  - response file: {TC-ID}-step-{N}-response.json (status, headers, body, duration_ms)
  - if step involves DB check: {TC-ID}-step-{N}-db-state.json (query, result, duration_ms)
- UI tests — EVERY step MUST capture:
  - screenshot: {TC-ID}-step-{N}.png (full page screenshot after action)
  - if step has console errors: {TC-ID}-step-{N}-console.json
  - if step involves API call: also capture request/response as above
- Database tests — EVERY step MUST capture:
  - query file: {TC-ID}-step-{N}-query.sql
  - result file: {TC-ID}-step-{N}-result.json (rows, execution_time_ms)
  - schema snapshot: {TC-ID}-step-{N}-schema.json (if migration test)
- Performance tests — EVERY step MUST capture:
  - metrics file: {TC-ID}-step-{N}-metrics.json (response_time_p50, p95, p99, throughput, error_rate)
  - if load test: {TC-ID}-step-{N}-load-profile.json (concurrent_users, ramp_up, duration)
- Security tests — EVERY step MUST capture:
  - request file: {TC-ID}-step-{N}-request.json (including malicious payload)
  - response file: {TC-ID}-step-{N}-response.json (verify rejection/sanitization)
- TOTAL captures per testcase = number of steps × captures per step type
- example: testcase with 5 steps (API) = minimum 10 files (5 request + 5 response)

---

# 📊 TEST REPORT RULES

## Testcase Document (BEFORE execution)
- MUST be saved as Markdown: reports/testcases/{feature-name}-testcases-{YYYYMMDD}.md
- MUST also generate HTML: reports/testcases/{feature-name}-testcases-{YYYYMMDD}.html
- HTML MUST have styled tables, color-coded priority (critical=red, high=orange, medium=yellow, low=green)
- MUST include all testcases with full detail (steps, data, expected results)
- MUST be reviewed before execution begins

## Execution Report (AFTER execution)
- MUST be saved as Markdown: reports/e2e/{feature-name}-report-{YYYYMMDD}.md
- MUST be saved as HTML: reports/e2e/{feature-name}-report-{YYYYMMDD}.html
- MUST be saved as JSON: reports/e2e/{feature-name}-results-{YYYYMMDD}.json
- HTML report MUST include:
  - styled summary dashboard (pass/fail counts with colors, pass rate percentage, total duration)
  - color-coded status badges per testcase (PASS=green, FAIL=red, SKIP=gray, BLOCKED=orange)
  - collapsible/expandable sections per testcase with step-by-step detail
  - embedded screenshots inline (for UI tests — img tags referencing screenshot files)
  - syntax-highlighted JSON for API request/response captures
  - clickable links to all capture files
  - table of contents with anchor links per feature and test type
- ALL report formats (MD, HTML, JSON) MUST include:
  - execution summary (total, passed, failed, skipped, blocked)
  - pass rate percentage
  - execution duration
  - environment info (OS, browser, API URL, DB)
  - per-testcase results with step-by-step detail
  - all captures (screenshots, API dumps, DB snapshots)
  - failed testcase analysis with root cause and suggestion
  - notes and observations
  - recommendations for next iteration

## Report Naming Convention
- ALL reports MUST be organized in date-based folders: {YYYYMMDD}/
- if multiple runs on same day, append run number: {YYYYMMDD}-run{N}/

- testcase documents:
  - reports/testcases/{YYYYMMDD}/{feature-name}-testcases.md
  - reports/testcases/{YYYYMMDD}/{feature-name}-testcases.html

- execution reports:
  - reports/e2e/{YYYYMMDD}/{feature-name}-report.md
  - reports/e2e/{YYYYMMDD}/{feature-name}-report.html
  - reports/e2e/{YYYYMMDD}/{feature-name}-results.json

- captures (inside date folder):
  - reports/e2e/{YYYYMMDD}/screenshots/{feature-name}/{TC-ID}-step-{N}.png
  - reports/e2e/{YYYYMMDD}/captures/{feature-name}/{TC-ID}-step-{N}-request.json
  - reports/e2e/{YYYYMMDD}/captures/{feature-name}/{TC-ID}-step-{N}-response.json
  - reports/e2e/{YYYYMMDD}/captures/{feature-name}/{TC-ID}-step-{N}-query.sql
  - reports/e2e/{YYYYMMDD}/captures/{feature-name}/{TC-ID}-step-{N}-result.json
  - reports/e2e/{YYYYMMDD}/captures/{feature-name}/{TC-ID}-step-{N}-metrics.json

- example structure for feature "auth" run on 2026-04-14:
  ```
  reports/
  ├── testcases/
  │   └── 20260414/
  │       ├── auth-testcases.md
  │       └── auth-testcases.html
  └── e2e/
      └── 20260414/
          ├── auth-report.md
          ├── auth-report.html
          ├── auth-results.json
          ├── screenshots/auth/
          │   ├── TC-AUTH-UI-001-step-1.png
          │   ├── TC-AUTH-UI-001-step-2.png
          │   └── ...
          └── captures/auth/
              ├── TC-AUTH-API-001-step-1-request.json
              ├── TC-AUTH-API-001-step-1-response.json
              └── ...
  ```

---

# 🚫 FAIL CONDITIONS

QA Planning MUST FAIL if:
- feature has no testcases defined
- testcase has no steps
- critical/high priority testcase is skipped without justification
- test report not generated after execution
- failed testcase has no error documentation
- no evidence/capture for executed steps
- ANY step is missing its own individual capture file
- testcase has only 1 capture for multiple steps (MUST be 1 capture PER step minimum)

---

# 🚨 ENFORCEMENT RULE

IF ANY RULE IS VIOLATED:
→ E2E_Test_Agent MUST FAIL
→ QA MUST FAIL
→ MUST RETURN TO QA PLANNING FOR FIX
