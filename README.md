# AI-Driven Starter Blueprint — Enterprise-Grade (Kiro)

An enterprise-grade rulebook for AI engineering systems. This blueprint provides a comprehensive, declarative configuration that orchestrates AI agents through structured flows — covering backend, frontend, database, infrastructure, security, QA testing, and deployment.

No application code is generated. The blueprint acts as a "rulebook" consumed by AI agents to enforce standards, validate quality, and automate workflows across the entire software lifecycle.

## Project Structure

```
project-root/
├── .kiro/
│   ├── steering/blueprint.md           # Auto-loaded by Kiro — references all rules/agents/flows
│   ├── hooks/                          # 11 automation hooks
│   ├── specs/                          # Spec documents
│   └── settings/mcp.json              # MCP server configs (Playwright, PostgreSQL, Fetch)
├── kiro/
│   ├── agents/                         # 14 agent definitions
│   ├── flows/                          # 7 flow definitions
│   ├── rules/                          # 7 rule files
│   ├── memory/                         # Contracts & history
│   └── config.yaml                     # Central configuration
├── templates/
│   ├── project_intake.yaml             # Project intake template
│   └── GUIDE.md                        # Usage guide
├── reports/                            # Auto-generated (gitignored)
│   ├── backend/                        # Coverage, test results, benchmarks
│   ├── frontend/                       # Frontend test results
│   ├── testcases/{YYYYMMDD}/           # Testcase documents (MD + HTML)
│   └── e2e/{YYYYMMDD}/                # Execution reports, screenshots, captures
├── backend/                            # Backend code (Go)
├── frontend/                           # Frontend code (React/Vue/Next)
├── .gitignore
├── README.md
├── FEATURE_STATUS.md
└── docker-compose.yml
```

## How to Use

1. Copy this template to a new project folder
2. Fill in `project_intake.yaml` with your business requirements
3. Open in Kiro — steering auto-loads all rules and agents
4. Ask Kiro: "Baca project_intake.yaml dan setup project ini"
5. Kiro designs architecture, builds features, runs QA, generates reports

## Agents (14)

| Agent | Role | Description |
|-------|------|-------------|
| `backend_dev_agent` | Backend Engineer | Go: handler, service, repository, migration, swagger. TransactionManager interface. |
| `frontend_dev_agent` | Frontend Engineer | React/Vue/Next: pages, components, services, types. Skipped if frontend disabled. |
| `qa_agent` | QA Engineer | Scores quality across 11 categories (total 100). Integrates specialist agent results. |
| `qa_planning_agent` | QA Planning Engineer | Creates testcase documents per feature (API, UI, integration, performance, security, DB). |
| `e2e_test_agent` | E2E Test Engineer | Executes testcases step-by-step with captures. Uses Playwright MCP for UI tests. |
| `reviewer_agent` | Code Reviewer | Reviews against all rules. Supports early_review and final_review modes. |
| `pm_agent` | Product Manager | Defines feature specs with database, infra, frontend, security requirements. |
| `planner_agent` | System Architect | Plans architecture and task breakdown including database and infrastructure tasks. |
| `optimizer_agent` | Performance Optimizer | Analyzes performance across backend, frontend, database, infra, caching. |
| `orchestrator_agent` | Orchestrator | Controls flow execution, manages retry, logs to execution_history. |
| `dba_agent` | Database Administrator | Validates migrations, schema, indexing, connection pooling. Uses PostgreSQL MCP. |
| `devops_agent` | DevOps Engineer | Validates Dockerfiles, K8s manifests, CI/CD, IaC, secrets. |
| `security_agent` | Security Auditor | Audits backend, frontend, database, infra security. Dependency scanning. |
| `techwriter_agent` | Technical Writer | Updates README, FEATURE_STATUS, swagger docs. Validates documentation. |

## Flows (7)

| Flow | Steps | Use Case |
|------|-------|----------|
| `feature_flow` | pm → planner → reviewer → backend_dev → swagger → frontend_dev → qa → optimizer → techwriter → orchestrator | New feature |
| `hotfix_flow` | reviewer → backend_dev → frontend_dev → qa → techwriter → orchestrator | Bug fix |
| `refactor_flow` | planner → backend_dev → frontend_dev → qa → reviewer → optimizer → techwriter → orchestrator | Refactoring |
| `migration_flow` | planner → backend_dev → dba → reviewer → qa → (retry) → techwriter → orchestrator | DB changes |
| `deployment_flow` | devops → security → qa → (retry) → techwriter → orchestrator | Deploy |
| `security_audit_flow` | security → dba → devops → reviewer → techwriter → orchestrator | Security audit |
| `e2e_test_flow` | pm → backend_dev → frontend_dev → security → dba → e2e_test → (retry) → qa → techwriter → orchestrator | E2E testing |

## Rules (7)

| File | Scope |
|------|-------|
| `global_rules.md` | Architecture, git hygiene, PR automation, code quality |
| `backend_rules.md` | Clean architecture, TransactionManager, resilience patterns, testing standards, logging, swagger, background workers |
| `frontend_rules.md` | Accessibility, design system, i18n, SSR/SSG, bundle optimization, error boundaries, feature flags |
| `database_rules.md` | Migration (SQL + Go), schema design, indexing, connection pooling, backup, query optimization |
| `infra_rules.md` | CI/CD, Docker, Kubernetes, secrets, IaC, monitoring, disaster recovery, deployment strategies |
| `qa_rules.md` | 11-category scoring (total 100), fail conditions, test execution & documentation standards |
| `qa_planning_rules.md` | Testcase design, naming convention (TC-ID), coverage rules, step-by-step capture, report format (MD + HTML + JSON) |

## Hooks (11)

| Hook | Trigger | Action |
|------|---------|--------|
| `qa-validation-post-task` | After task | QA scoring + save to quality_history.json |
| `code-review-on-stop` | Agent stop | Review against all rules |
| `performance-check-on-edit` | Go file edited | Check N+1, SELECT *, pagination |
| `security-check-pre-write` | Before file write | Check hardcoded secrets |
| `save-execution-history` | Agent stop | Save to execution_history.json |
| `swagger-auto-generate` | Handler edited | Run swag init |
| `auto-run-backend-tests` | Test file edited | Run go test + coverage to reports/backend/ |
| `auto-run-benchmark-tests` | Test file edited | Run go bench to reports/backend/ |
| `generate-testcases` | After task | QA Planning Agent creates testcase document |
| `e2e-test-post-feature` | Testcase created | Full-stack E2E: backend API via Fetch MCP + frontend UI via Playwright MCP + integration cross-checks, per-step screenshots, MD + HTML + JSON reports |
| `techwriter-update-docs` | Agent stop | Update README, FEATURE_STATUS, swagger |

## MCP Servers (3)

| Server | Purpose | Used By |
|--------|---------|---------|
| Playwright | Browser-based UI testing | e2e_test_agent |
| PostgreSQL | Direct DB queries for verification | dba_agent, e2e_test_agent |
| Fetch | HTTP API testing | e2e_test_agent, security_agent |

Config: `.kiro/settings/mcp.json`. PostgreSQL disabled by default — enable and set connection string per project.

## QA Scoring (11 categories, total 100)

| Category | Weight |
|----------|--------|
| Architecture | 15 |
| Transaction | 10 |
| Testing | 15 |
| Observability | 10 |
| APM | 5 |
| Security | 10 |
| Performance | 10 |
| Code Quality | 5 |
| Database | 10 |
| Infrastructure | 5 |
| Security Audit | 5 |

Pass threshold: 80. Auto-fail on: migration without down, Dockerfile root user, hardcoded secrets, K8s without resource limits, tests not passing, coverage below 80%.

## Report Structure

```
reports/
├── backend/                            # Go test + coverage + benchmark
├── testcases/{YYYYMMDD}/              # Testcase docs (MD + HTML) per date
└── e2e/{YYYYMMDD}/                    # Execution reports per date
    ├── {feature}-report.md
    ├── {feature}-report.html
    ├── {feature}-results.json
    ├── screenshots/{feature}/          # UI captures per step
    └── captures/{feature}/             # API/DB/perf captures per step
```

## Getting Started

See `templates/GUIDE.md` for step-by-step instructions.
