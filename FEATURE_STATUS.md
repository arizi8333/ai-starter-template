# Feature Status — Enterprise Blueprint

## Phase 1: Rules

- [x] `kiro/rules/global_rules.md` — Global rules + git hygiene
- [x] `kiro/rules/backend_rules.md` — Backend rules + resilience patterns + background workers + testing standards
- [x] `kiro/rules/frontend_rules.md` — Frontend rules + enterprise enhancements
- [x] `kiro/rules/database_rules.md` — Database rules (SQL + Go migrations)
- [x] `kiro/rules/infra_rules.md` — Infrastructure & DevOps rules
- [x] `kiro/rules/qa_rules.md` — QA scoring + test execution standards
- [x] `kiro/rules/qa_planning_rules.md` — QA planning + testcase design + report format

## Phase 2: Agents

- [x] `kiro/agents/backend_dev_agent.yaml` — Senior Backend Engineer
- [x] `kiro/agents/frontend_dev_agent.yaml` — Senior Frontend Engineer
- [x] `kiro/agents/qa_agent.yaml` — QA Engineer (11-category scoring)
- [x] `kiro/agents/qa_planning_agent.yaml` — QA Planning Engineer (testcase design)
- [x] `kiro/agents/e2e_test_agent.yaml` — E2E Test Engineer (step-by-step + Playwright MCP)
- [x] `kiro/agents/reviewer_agent.yaml` — Code Reviewer
- [x] `kiro/agents/pm_agent.yaml` — Product Manager
- [x] `kiro/agents/planner_agent.yaml` — System Architect
- [x] `kiro/agents/optimizer_agent.yaml` — Performance Optimizer
- [x] `kiro/agents/orchestrator_agent.yaml` — Orchestrator
- [x] `kiro/agents/dba_agent.yaml` — Senior Database Administrator
- [x] `kiro/agents/devops_agent.yaml` — Senior DevOps Engineer
- [x] `kiro/agents/security_agent.yaml` — Senior Security Auditor
- [x] `kiro/agents/techwriter_agent.yaml` — Senior Technical Writer

## Phase 3: Flows

- [x] `kiro/flows/feature_flow.yaml` — New feature development
- [x] `kiro/flows/hotfix_flow.yaml` — Bug fixes
- [x] `kiro/flows/refactor_flow.yaml` — Code refactoring
- [x] `kiro/flows/migration_flow.yaml` — Database migrations
- [x] `kiro/flows/deployment_flow.yaml` — Deployment
- [x] `kiro/flows/security_audit_flow.yaml` — Security audit
- [x] `kiro/flows/e2e_test_flow.yaml` — End-to-end testing

## Phase 4: Config & Memory

- [x] `kiro/config.yaml` — Central config (scoring, retry, database, infra, security, deployment)
- [x] `kiro/memory/api_contract.yaml` — API contract + database schemas
- [x] `kiro/memory/infra_contract.yaml` — Infrastructure contract
- [x] `kiro/memory/execution_history.json` — Execution tracking
- [x] `kiro/memory/quality_history.json` — QA score tracking

## Phase 5: Hooks

- [x] `qa-validation-post-task` — QA scoring + save quality_history
- [x] `code-review-on-stop` — Auto code review
- [x] `performance-check-on-edit` — Performance check on Go files
- [x] `security-check-pre-write` — Secret detection before write
- [x] `save-execution-history` — Save execution_history
- [x] `swagger-auto-generate` — Auto swag init on handler edit
- [x] `auto-run-backend-tests` — Auto test + coverage to reports/backend/
- [x] `auto-run-benchmark-tests` — Auto benchmark to reports/backend/
- [x] `generate-testcases` — QA Planning Agent creates testcase docs
- [x] `e2e-test-post-feature` — E2E Test Agent runs testcases step-by-step
- [x] `techwriter-update-docs` — TechWriter updates README/FEATURE_STATUS/swagger

## Phase 6: MCP & Tooling

- [x] `.kiro/settings/mcp.json` — Playwright (UI testing), PostgreSQL (DB verification), Fetch (API testing)
- [x] `.kiro/steering/blueprint.md` — Auto-load steering with all references
- [x] `.gitignore` — Backend, frontend, env, IDE, reports, swagger, coverage
- [x] `templates/project_intake.yaml` — Project intake template
- [x] `templates/GUIDE.md` — Usage guide

## Phase 7: Documentation

- [x] `README.md` — Full project documentation
- [x] `FEATURE_STATUS.md` — This file

---

## Changelog

| Date | Description |
|------|-------------|
| 2026-04-08 | Initial enterprise blueprint — rules, agents, flows, config, memory |
| 2026-04-08 | Split dev_agent into backend_dev_agent + frontend_dev_agent |
| 2026-04-08 | Added resilience patterns, background workers, testing standards to backend_rules |
| 2026-04-08 | Added git hygiene to global_rules, reviewer_agent |
| 2026-04-09 | Added project intake template, steering, hooks |
| 2026-04-09 | Added e2e_test_agent, qa_planning_agent, qa_planning_rules |
| 2026-04-09 | Added e2e_test_flow, generate-testcases hook, e2e-test-post-feature hook |
| 2026-04-09 | Added MCP servers: Playwright, PostgreSQL, Fetch |
| 2026-04-14 | Added techwriter-update-docs hook, auto-run-benchmark-tests hook |
| 2026-04-14 | Updated all reports to date-based folders (reports/{YYYYMMDD}/) |
| 2026-04-14 | Added HTML format for all reports (MD + HTML + JSON) |
| 2026-04-15 | Updated e2e-test-post-feature hook v2: MCP Fetch + Playwright integration, per-step screenshots, HTML report with collapsible sections, unit test referencing |
| 2026-04-15 | Updated e2e-test-post-feature hook v3: real Playwright browser execution — direct mcp_playwright_browser_* tool calls, per-field screenshots, concrete examples (Login/Create forms), error path testing |
| 2026-04-15 | Updated e2e-test-post-feature hook v4: full-stack testing — backend API via Fetch MCP + frontend UI via Playwright MCP + integration cross-checks, CRUD examples (Create/Edit/Delete), health check |
