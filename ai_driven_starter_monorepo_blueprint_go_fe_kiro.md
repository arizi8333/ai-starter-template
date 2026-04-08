# 🚀 AI-Driven Starter Blueprint — ENTERPRISE-GRADE RULES (Kiro)

This is a **super-strict, enterprise-grade rulebook** to ensure AI consistently produces **secure, scalable, observable, and maintainable systems**.

---

# 🧠 CORE PRINCIPLE

System > Code  
Consistency > Speed  
Safety > Flexibility  

---

# 📜 GLOBAL RULES (NON-NEGOTIABLE)

- MUST NOT place business logic in handler
- MUST use service layer
- MUST use repository for DB
- MUST include swagger
- MUST include tests
- MUST NOT break existing API
- MUST follow naming conventions
- MUST analyze existing code before changes
- MUST follow api_contract.yaml

---

# 🔗 CONTRACT-DRIVEN DEVELOPMENT

- API MUST follow api_contract.yaml
- MUST NOT modify existing contract without versioning
- request/response MUST match contract
- QA MUST validate contract compliance

---

# 🧩 DEPENDENCY RULES

- handler MUST depend only on service
- service MUST depend only on repository
- repository MUST NOT depend on service
- no circular dependency allowed
- shared utilities MUST be placed in pkg/

---

# 🔄 TRANSACTION RULES (STRICT)

- ALL DB write operations MUST use transaction manager
- transaction MUST exist ONLY in service layer
- transaction MUST NOT exist in handler
- transaction MUST NOT exist in repository
- repository MUST read transaction from context
- nested transaction MUST be avoided unless explicitly required

---

# 🧠 CONTEXT RULES

- context MUST flow from handler → service → repository
- MUST NOT create new context arbitrarily
- MUST propagate:
  - request_id
  - tracing context
- context cancellation MUST be respected

---

# ❗ ERROR HANDLING RULES

- errors MUST be wrapped with context
- MUST NOT expose internal error directly
- service MUST return domain-specific errors
- handler MUST map errors to HTTP response
- error MUST include traceable metadata

---

# 🔐 SECURITY RULES

- all input MUST be validated
- MUST NOT use raw SQL concatenation
- MUST use safe query methods
- sensitive data MUST NOT be logged
- auth middleware REQUIRED where applicable
- rate limiting SHOULD be applied

---

# 📦 DTO & MAPPING RULES

- handler MUST use DTO
- MUST NOT expose domain model
- mapping MUST be explicit
- MUST NOT leak internal structure

---

# 🧪 TESTING RULES (STRICT)

- every service MUST have unit tests
- MUST test success & failure cases
- integration test REQUIRED
- edge cases MUST be tested
- test MUST pass before completion
- feature MUST FAIL if no test

---

# 🔁 IDEMPOTENCY RULES

- critical write endpoints MUST be idempotent
- duplicate requests MUST NOT create duplicate data
- idempotency key SHOULD be supported

---

# 📊 OBSERVABILITY RULES

- every request MUST have request_id
- structured logging REQUIRED
- logs MUST include identifiers
- all errors MUST be logged
- critical endpoints SHOULD expose metrics

---

# ⚡ APM & TRACING RULES

- APM MUST start at handler
- tracing MUST propagate across layers
- every request MUST be traceable
- slow transactions MUST be identifiable

---

# 📈 PERFORMANCE RULES

- MUST NOT use SELECT *
- MUST avoid N+1 queries
- pagination REQUIRED for list endpoints
- SHOULD suggest index for heavy queries
- MUST consider response time

---

# ⚙️ CONFIGURATION RULES

- MUST NOT hardcode configuration
- MUST use env/config file
- MUST NOT store secrets in code

---

# 🔄 CHANGE AWARENESS RULES

- MUST analyze existing system before change
- MUST maintain backward compatibility
- MUST update related components together
- MUST NOT introduce breaking change

---

# 🧠 CODE QUALITY RULES

- MUST NOT duplicate logic
- MUST reuse existing components
- MUST follow single responsibility principle
- MUST keep functions small and readable

---

# 🔐 SAFETY GUARDRAILS

AI MUST NOT:
- modify unrelated files
- bypass architecture layers
- remove existing functionality

AI MUST:
- follow all rules
- generate test
- update swagger
- implement observability
- respect API contract

---

# 🧾 DEFINITION OF DONE (STRICT)

Feature is COMPLETE ONLY IF:

- code implemented
- unit test exists & passing
- integration test exists
- swagger updated
- logging implemented
- APM integrated
- API contract satisfied
- performance considered
- database compliance validated (migration, schema, indexing)
- infrastructure validation passed (Dockerfile, K8s, CI/CD)
- security audit passed (no critical/high issues)
- deployment readiness confirmed (environment config, rollback strategy)

---

# 🔄 FLOW AWARENESS (CRITICAL)

System MUST support modes:

## feature
- full implementation allowed
- MUST follow all rules

## refactor
- MUST improve code quality
- MUST NOT change behavior
- MUST NOT break API

## hotfix
- MUST apply minimal change
- MUST NOT refactor unrelated code
- MUST prioritize stability

---

# 🧪 QA AUTOMATION SYSTEM (STRICT)

## ✅ VALIDATION CHECKLIST

QA MUST validate:

- architecture compliance
- transaction usage
- context propagation
- API & swagger
- testing coverage
- observability
- APM tracing
- security
- performance

---

## 📊 SCORING SYSTEM (0–100)

| Category         | Weight |
|-----------------|--------|
| Architecture     | 15     |
| Transaction      | 10     |
| Testing          | 15     |
| Observability    | 10     |
| APM              | 5      |
| Security         | 10     |
| Performance      | 10     |
| Code Quality     | 5      |
| Database         | 10     |
| Infrastructure   | 5      |
| Security Audit   | 5      |

---

## 🚫 AUTO REJECT RULE

QA MUST FAIL if:

- score < threshold (config.yaml)
- missing transaction
- missing test
- missing swagger
- breaking API contract

---

# 🔁 AUTO FEEDBACK LOOP

Flow:

QA FAIL → Dev → QA (loop)

Rules:
- Dev MUST fix ONLY failed checks
- MUST respect max_retry (config.yaml)
- MUST stop if retry exceeded

---

# 📊 QUALITY TREND TRACKING

- QA result MUST be stored
- MUST compare previous score
- MUST determine trend:
  - improving
  - declining
  - stable

---

# 🛠️ PR AUTOMATION

PR MUST include:
- summary
- changes
- QA score
- checklist

PR MUST NOT be created if QA FAIL

Commit MUST follow:

feat(scope): description

---

# ⚙️ ORCHESTRATION RULES

- system MUST follow defined flow
- MUST NOT skip steps
- MUST validate each agent output
- MUST handle retry logic
- MUST stop on critical failure
- MUST log execution result

---

# 🚀 FINAL SYSTEM BEHAVIOR

System WILL:

- build feature
- validate strictly
- score quality
- reject low quality
- auto-fix via feedback loop
- track quality over time
- auto-create PR

---

# 🔥 FINAL LEVEL

This is NOT just AI coding.

This is:

👉 AUTONOMOUS AI ENGINEERING SYSTEM

Capable of:
- designing systems
- building features
- validating quality
- fixing itself
- tracking improvement
- shipping production-ready code


---

# === ENTERPRISE-GRADE ENHANCEMENT ===

---

# 🗄️ DATABASE RULES (Summary)

Rules file: `kiro/rules/database_rules.md`

## Migration Management
- every schema change MUST use a versioned migration file
- naming format: `YYYYMMDDHHMMSS_description`
- every migration MUST have `up` and `down` functions (reversible)
- migration MUST be idempotent and MUST NOT contain business logic

## Schema Design Standards
- every table MUST have: primary key, `created_at`, `updated_at`, `deleted_at` (soft delete)
- naming convention: snake_case for tables and columns
- MUST define column constraints (NOT NULL, DEFAULT) explicitly

## Indexing Strategy
- MUST create index on every foreign key column
- MUST create index on columns used in WHERE and ORDER BY
- MUST use composite index for multi-column queries
- index naming: `idx_{table}_{column(s)}`

## Connection Pooling
- MUST use connection pool with `max_open`, `max_idle`, `max_lifetime` from config.yaml
- MUST NOT create ad-hoc connections outside the pool

## Backup & Recovery
- scheduled backup strategy (from config.yaml)
- recovery procedure MUST be documented
- MUST support point-in-time recovery for production

## Data Integrity
- foreign key constraints MUST be defined for all relationships
- unique and check constraints MUST be applied where domain requires
- MUST NOT rely solely on application-level validation

## Query Optimization
- MUST NOT use SELECT *
- MUST use pagination for all list queries
- MUST NOT allow N+1 queries
- MUST run EXPLAIN for complex queries

## Database Security
- encryption at rest and in transit (TLS) MUST be enabled
- access control MUST be role-based
- MUST NOT use superuser credentials for application connections

## Seed Data Management
- seed data separated per environment
- seed data MUST be idempotent and version controlled

## Multi-Environment Configuration
- database configuration separated per environment
- credentials MUST be sourced from Secret_Manager

## Enforcement
IF ANY RULE IS VIOLATED → DBA_Agent MUST FAIL → QA MUST FAIL → MUST RETURN TO DEV FOR FIX

---

# 🏗️ INFRASTRUCTURE / DEVOPS RULES (Summary)

Rules file: `kiro/rules/infra_rules.md`

## CI/CD Pipeline
- stages MUST follow: lint → test → build → security scan → deploy-staging → integration-test → deploy-production
- every stage MUST have defined exit criteria
- pipeline MUST fail fast on critical errors

## Container / Docker
- MUST use multi-stage build
- MUST run as non-root user
- MUST enable image scanning
- image size MUST be minimal (alpine base preferred)
- MUST define HEALTHCHECK in Dockerfile

## Kubernetes / Orchestration
- MUST define resource limits (CPU, memory) for every container
- MUST define liveness, readiness, and startup probes
- MUST define pod disruption budget
- MUST use namespaces for environment isolation

## Environment Management
- environments separated: development, staging, production
- configuration isolated per environment
- MUST NOT share credentials across environments

## Secret Management
- MUST use Secret_Manager for all secrets
- MUST NOT store secrets in repository or plaintext env vars
- MUST rotate secrets periodically

## Infrastructure as Code
- all infrastructure MUST be defined in code (Terraform/Pulumi)
- state management and versioning MUST be configured
- MUST NOT make manual infrastructure changes outside IaC

## Monitoring & Alerting
- metrics collection and log aggregation MUST be enabled
- alerting rules and dashboards MUST exist for every service
- MUST define SLIs and SLOs for critical services

## Disaster Recovery
- RTO and RPO MUST be defined
- backup strategy and failover procedure MUST be documented and tested

## Network Security
- network segmentation and firewall rules MUST be implemented
- TLS termination and DDoS protection MUST be configured
- MUST NOT expose internal services to public network

## Load Balancing & Scaling
- HPA (Horizontal Pod Autoscaler) MUST be configured
- scaling policy with min/max replicas MUST be documented

## Deployment Strategy
- MUST support Blue_Green_Deployment, Canary_Deployment, and rolling update
- rollback MUST be automatic on failure
- deployment MUST be zero-downtime

## Enforcement
IF ANY RULE IS VIOLATED → DevOps_Agent MUST FAIL → QA MUST FAIL → MUST RETURN TO DEV FOR FIX

---

# 🎨 FRONTEND RULES (Enterprise Enhancement)

Rules file: `kiro/rules/frontend_rules.md` (enhanced)

In addition to existing frontend rules, the following enterprise-grade standards apply:

- **Micro-Frontend Architecture**: MUST use module federation or similar; each module deployable independently
- **Design System**: MUST use design tokens, centralized component library, and component documentation (Storybook or equivalent)
- **Accessibility (Enhanced)**: MUST use semantic HTML, ARIA attributes, keyboard navigation; color contrast minimum 4.5:1
- **Internationalization (i18n)**: MUST use i18n library; translation strings separated from code; RTL layout support
- **SSR/SSG**: MUST consider SSR/SSG for SEO-critical and performance-sensitive pages
- **Bundle Optimization (Enhanced)**: MUST implement tree shaking, code splitting, lazy loading; max bundle size configured
- **Error Boundary**: every route and critical module MUST have error boundary with fallback UI and error reporting
- **Feature Flag Integration**: MUST use feature flag system for new features; toggle without deployment
- **E2E Testing**: MUST use Cypress or Playwright; minimum coverage: happy path + error path
- **Visual Regression Testing**: MUST use screenshot comparison for critical components and main pages

---

# 🤖 NEW AGENTS

## DBA_Agent (`kiro/agents/dba_agent.yaml`)
- **Role**: Database Administrator
- **Responsibility**: Validates migration files, schema design, indexing strategy, connection pooling, and query optimization against `database_rules.md`
- **Checks**: migration (naming, up/down, idempotent), schema (PK, timestamps, constraints), indexing (FK, WHERE, ORDER BY), connection pool, query optimization
- **Severity**: HIGH (missing down function, missing PK, no connection pool, SELECT *), MEDIUM (missing FK index, missing timestamps), LOW (naming convention)
- **Status**: FAIL if any severity HIGH issue found

## DevOps_Agent (`kiro/agents/devops_agent.yaml`)
- **Role**: DevOps Engineer
- **Responsibility**: Validates Dockerfile, Kubernetes manifests, CI/CD pipeline, IaC files, and secret management against `infra_rules.md`
- **Checks**: docker (multi-stage, non-root, scanning, size), kubernetes (resource limits, probes, PDB), cicd (stages, security scan), iac (state, versioning), secrets (no hardcoded)
- **Severity**: HIGH (root user, missing resource limits, hardcoded secrets, missing health checks), MEDIUM (missing security scan, no multi-stage), LOW (image size)
- **Status**: FAIL if any severity HIGH issue found

## Security_Agent (`kiro/agents/security_agent.yaml`)
- **Role**: Security Auditor
- **Responsibility**: Audits backend, frontend, database, and infrastructure security; performs dependency vulnerability scanning
- **Checks**: backend (input validation, SQL injection, auth), frontend (XSS, CSRF, secure storage), database (encryption, access control), infrastructure (network, TLS, containers), dependencies (CVEs)
- **Severity**: CRITICAL (SQL injection, hardcoded credentials, missing encryption), HIGH (XSS, missing auth, root container), MEDIUM (missing CSRF), LOW (outdated non-critical dependency)
- **Status**: FAIL if any severity CRITICAL or HIGH issue found

## TechWriter_Agent (`kiro/agents/techwriter_agent.yaml`)
- **Role**: Technical Writer
- **Responsibility**: Generates/updates README.md, FEATURE_STATUS.md, swagger docs, API docs; ensures documentation consistency
- **Checks**: readme (completeness), feature_status (up-to-date), swagger (accuracy), api_docs (from contract), consistency (terminology, cross-references)
- **Severity**: HIGH (missing README section, incomplete swagger, broken cross-reference), MEDIUM (outdated status, inconsistent terminology), LOW (formatting)
- **Status**: FAIL if documentation incomplete or inconsistent
- **Integration**: Included as step before orchestrator_agent in ALL flows

---

# 🔄 NEW FLOWS

## Migration Flow (`kiro/flows/migration_flow.yaml`)
- **Use Case**: Database schema changes and migrations
- **Steps**: planner_agent → dev_agent → dba_agent → reviewer_agent → qa_agent → loop (retry if dba/qa fail, max_retry from config) → techwriter_agent → orchestrator_agent
- **Retry**: Automatic feedback loop back to dev_agent if DBA_Agent or qa_agent returns fail

## Deployment Flow (`kiro/flows/deployment_flow.yaml`)
- **Use Case**: Infrastructure deployment and release
- **Steps**: devops_agent → security_agent → qa_agent → loop (retry if fail, max_retry from config) → techwriter_agent → orchestrator_agent
- **Config**: Deployment strategy (blue-green, canary, rolling) sourced from config.yaml

## Security Audit Flow (`kiro/flows/security_audit_flow.yaml`)
- **Use Case**: Comprehensive security audit across all domains
- **Steps**: security_agent → dba_agent → devops_agent → reviewer_agent → techwriter_agent → orchestrator_agent
- **Output**: Consolidated security report covering code, database, and infrastructure

---

# ✅ QA VALIDATION CHECKLIST (Enterprise Enhancement)

In addition to existing QA validation, the following MUST be validated:

## Database Validation
- migration compliance (naming format, up/down functions, idempotent)
- schema standards (primary key, timestamps, soft delete, constraints)
- query optimization (no SELECT *, pagination, no N+1, EXPLAIN)
- connection pooling configured

## Infrastructure Validation
- Dockerfile compliance (multi-stage build, non-root user, minimal image)
- Kubernetes manifest (resource limits, health checks, PDB)
- CI/CD pipeline (all required stages present, security scan)
- secret management (no hardcoded secrets, Secret_Manager used)

## Security Audit Validation
- vulnerability scan executed
- dependency check passed (no critical CVEs)
- no credential exposure in repository

## Additional Auto Reject Rules
- QA MUST FAIL if migration does not have reversible `down` function
- QA MUST FAIL if Dockerfile uses root user
- QA MUST FAIL if secrets are hardcoded in repository
- QA MUST FAIL if Kubernetes manifest has no resource limits
