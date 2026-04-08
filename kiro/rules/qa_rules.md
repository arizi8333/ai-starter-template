# 🧪 QA RULES (STRICT - NON-NEGOTIABLE)

---

# 🧠 CORE RESPONSIBILITY

QA MUST:
- validate feature using full checklist
- calculate quality score (0–100)
- determine PASS or FAIL
- store result in memory
- provide actionable feedback to dev agent

---

# ✅ VALIDATION RULES

QA MUST validate:

## Architecture
- no business logic in handler
- service layer used correctly
- repository only handles DB

## Transaction
- DB write uses transaction manager
- no transaction in handler
- no transaction in repository

## Context
- context is properly propagated
- request_id exists

## API & Swagger
- swagger exists
- contract matches implementation

## Testing
- unit test exists
- integration test exists
- success & failure cases tested

## Observability
- structured logging exists
- request_id included in logs
- error logging implemented

## APM
- tracing implemented
- tracing propagated across layers

## Security
- input validation exists
- no sensitive data logged

## Performance
- no SELECT *
- pagination implemented
- no N+1 query

---

# 📊 SCORING RULES

QA MUST:

- assign score per category
- calculate weighted score

Categories:
- Architecture: 15
- Transaction: 10
- Testing: 15
- Observability: 10
- APM: 5
- Security: 10
- Performance: 10
- Code Quality: 5
- Database: 10
- Infrastructure: 5
- Security Audit: 5

---

# 🚫 FAIL CONDITIONS (AUTO REJECT)

QA MUST FAIL if:

- total score < 80
- missing transaction in DB write
- missing test
- missing swagger
- breaking existing API contract

---

# 🔁 FEEDBACK RULES

QA MUST:

- list all failed checks
- provide clear suggestions
- return feedback to dev agent

Dev Agent MUST:
- fix ONLY failed checks
- MUST NOT rewrite unrelated logic

---

# 💾 MEMORY RULES

QA MUST:

- store result in `quality_history.json`
- include:
  - feature name
  - score
  - timestamp

- compare with previous result
- determine trend:
  - improving
  - declining
  - stable

---

# 🚨 ENFORCEMENT RULE

IF QA FAILS:
→ system MUST trigger feedback loop
→ dev agent MUST retry (max_retry from config)
→ IF still FAIL → escalate (manual review)

---

# 🎯 OUTPUT REQUIREMENT

QA output MUST include:

- score (0–100)
- status (pass/fail)
- category_scores
- failed_checks
- suggestions
- trend (optional if history exists)


---

# === ENTERPRISE ENHANCEMENT ===

---

# 🗄️ DATABASE VALIDATION CHECKLIST

QA MUST validate database compliance:

## Migration Compliance
- migration naming follows format YYYYMMDDHHMMSS_description
- up function exists and is valid
- down function exists and is valid (reversible)
- migration is idempotent

## Schema Standards
- primary key exists on every table
- created_at, updated_at fields present
- deleted_at field present (soft delete)
- proper constraints (FK, unique, check)

## Query Optimization
- no SELECT *
- pagination implemented for list queries
- no N+1 queries
- EXPLAIN used for complex queries

## Connection Pooling
- connection pool configured
- max_open, max_idle, max_lifetime set per config.yaml

---

# 🏗️ INFRASTRUCTURE VALIDATION CHECKLIST

QA MUST validate infrastructure compliance:

## Dockerfile Compliance
- multi-stage build used
- non-root user configured
- image scanning configured
- minimal image size (alpine base preferred)

## Kubernetes Manifest
- resource limits defined (CPU, memory)
- liveness probe exists
- readiness probe exists
- startup probe exists
- pod disruption budget defined

## CI/CD Pipeline
- all required stages present (lint → test → build → security scan → deploy-staging → integration-test → deploy-production)
- each stage has exit criteria

## Secret Management
- no hardcoded secrets in repository
- Secret_Manager used for all credentials
- no secrets in environment variable plaintext

---

# 🛡️ SECURITY AUDIT VALIDATION CHECKLIST

QA MUST validate security audit compliance:

## Vulnerability Scan
- vulnerability scan executed
- no critical CVEs in dependencies

## Dependency Check
- dependency check passed
- no outdated dependencies with known vulnerabilities

## Credential Exposure
- no credentials exposed in repository
- no API keys or tokens in source code
- no sensitive data in logs or client-side code

---

# 🚫 ADDITIONAL FAIL CONDITIONS (ENTERPRISE)

QA MUST FAIL if:

- migration does not have down function (reversible)
- Dockerfile uses root user
- hardcoded secrets or credentials found in repository
- Kubernetes manifest missing resource limits
