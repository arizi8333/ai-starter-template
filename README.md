# AI-Driven Starter Blueprint — Enterprise-Grade (Kiro)

An enterprise-grade rulebook for AI engineering systems. This blueprint provides a comprehensive, declarative configuration that orchestrates AI agents through structured flows — covering backend, frontend, database, infrastructure, security, and deployment.

No application code is generated. The blueprint acts as a "rulebook" consumed by AI agents to enforce standards, validate quality, and automate workflows across the entire software lifecycle.

## Project Structure

```
ai-starter-template/
├── README.md
├── FEATURE_STATUS.md
├── ai_driven_starter_monorepo_blueprint_go_fe_kiro.md
└── kiro/
    ├── config.yaml
    ├── agents/
    │   ├── backend_dev_agent.yaml
    │   ├── frontend_dev_agent.yaml
    │   ├── qa_agent.yaml
    │   ├── reviewer_agent.yaml
    │   ├── pm_agent.yaml
    │   ├── planner_agent.yaml
    │   ├── optimizer_agent.yaml
    │   ├── orchestrator_agent.yaml
    │   ├── dba_agent.yaml
    │   ├── devops_agent.yaml
    │   ├── security_agent.yaml
    │   └── techwriter_agent.yaml
    ├── flows/
    │   ├── feature_flow.yaml
    │   ├── hotfix_flow.yaml
    │   ├── refactor_flow.yaml
    │   ├── migration_flow.yaml
    │   ├── deployment_flow.yaml
    │   └── security_audit_flow.yaml
    ├── memory/
    │   ├── api_contract.yaml
    │   ├── infra_contract.yaml
    │   ├── execution_history.json
    │   └── quality_history.json
    └── rules/
        ├── global_rules.md
        ├── backend_rules.md
        ├── frontend_rules.md
        ├── qa_rules.md
        ├── database_rules.md
        └── infra_rules.md
```

## How to Use

1. **Choose a flow** — Pick the flow that matches your task (feature, hotfix, refactor, migration, deployment, or security audit).
2. **System runs agents in order** — The flow executes agents sequentially, each performing its specialized role.
3. **QA scores quality** — The QA agent scores the output across 11 categories (total 100 points, threshold 80).
4. **Feedback loop on failure** — If QA or a specialist agent fails, the system retries automatically (up to `max_retry` from config) by looping back to fix only the failed checks.

## Agents

The blueprint includes 12 specialized agents:

| Agent | Role | Description |
|-------|------|-------------|
| `backend_dev_agent` | Backend Engineer | Implements backend features (Go: handler, service, repository, migration, swagger). Supports `feature`, `refactor`, `hotfix`, `database`, `infrastructure` modes. |
| `frontend_dev_agent` | Frontend Engineer | Implements frontend features (React/Vue/Next: pages, components, services, types). Supports `feature`, `refactor`, `hotfix` modes. Skipped if frontend not enabled. |
| `qa_agent` | QA Engineer | Validates quality across 11 scoring categories. Integrates results from specialist agents. Fails on critical violations. |
| `reviewer_agent` | Code Reviewer | Reviews code against all rules (global, backend, frontend, database, infra) and git hygiene. Supports `early_review` and `final_review` modes. |
| `pm_agent` | Product Manager | Defines feature specs including database, infrastructure, frontend, and security requirements. |
| `planner_agent` | System Architect | Analyzes requirements and plans implementation approach, including database and infrastructure tasks. |
| `optimizer_agent` | Performance Optimizer | Analyzes performance across backend, frontend, database, infrastructure, and caching layers. |
| `orchestrator_agent` | Orchestrator | Controls flow execution, evaluates specialist agent results, manages retry and logging. |
| `dba_agent` | Database Administrator | Validates migrations, schema design, indexing, connection pooling, and query optimization against `database_rules.md`. |
| `devops_agent` | DevOps Engineer | Validates Dockerfiles, Kubernetes manifests, CI/CD pipelines, IaC, and secret management against `infra_rules.md`. |
| `security_agent` | Security Auditor | Audits backend, frontend, database, and infrastructure security. Performs dependency vulnerability scanning. |
| `techwriter_agent` | Technical Writer | Generates and updates README, FEATURE_STATUS, swagger docs, and API documentation. Integrated into all flows. |

## Flows

6 flows orchestrate agents for different scenarios:

| Flow | Steps | Use Case |
|------|-------|----------|
| `feature_flow` | pm → planner → reviewer → backend_dev → swagger → frontend_dev → qa → optimizer → techwriter → orchestrator | New feature development |
| `hotfix_flow` | reviewer → backend_dev → frontend_dev → qa → techwriter → orchestrator | Urgent bug fixes |
| `refactor_flow` | planner → backend_dev → frontend_dev → qa → reviewer → optimizer → techwriter → orchestrator | Code refactoring |
| `migration_flow` | planner → backend_dev → dba → reviewer → qa → (retry loop) → techwriter → orchestrator | Database schema changes |
| `deployment_flow` | devops → security → qa → (retry loop) → techwriter → orchestrator | Infrastructure deployment |
| `security_audit_flow` | security → dba → devops → reviewer → techwriter → orchestrator | Comprehensive security audit |

Flows with retry loops automatically re-run failed agents up to `max_retry` (default: 3) from `config.yaml`.

## Rules

6 rule files define enterprise-grade standards:

| File | Scope |
|------|-------|
| `global_rules.md` | Cross-cutting rules applicable to all agents and domains, including git hygiene (gitignore, commit format, branch naming) |
| `backend_rules.md` | Backend development standards (API design, error handling, testing, observability) |
| `frontend_rules.md` | Frontend standards including enterprise enhancements (micro-frontend, design system, accessibility, i18n, SSR/SSG, bundle optimization, error boundaries, feature flags, E2E and visual regression testing) |
| `qa_rules.md` | QA validation checklists and scoring rules for all 11 categories including database, infrastructure, and security audit |
| `database_rules.md` | Database standards (migration management, schema design, indexing, connection pooling, backup/recovery, data integrity, query optimization, security, seed data, multi-environment config) |
| `infra_rules.md` | Infrastructure and DevOps standards (CI/CD, Docker, Kubernetes, environment management, secrets, IaC, monitoring, disaster recovery, network security, load balancing, deployment strategies) |

All rules are strictly enforced. Violations cause the responsible agent to fail, which triggers the QA feedback loop.

## Config (`config.yaml`)

The central configuration file controls the entire system:

| Section | Description |
|---------|-------------|
| `project_name` | Project identifier |
| `default_flow` | Default flow to execute (e.g., `feature_flow`) |
| `quality` | QA threshold (80), scoring toggle, auto-reject, and scoring weights for all 11 categories |
| `retry` | Max retry attempts (3), retry strategy (`fix_only_failed_checks`) |
| `features` | Feature toggles (quality tracking, feedback loop, PR automation, observability, security checks) |
| `qa` | QA behavior (strict mode, fail fast, require all categories) |
| `pr` | PR automation settings (auto-create, require pass, include summary/score/checklist) |
| `logging` | Log level and execution history toggle |
| `memory` | Paths to quality and execution history files |
| `environment` | Current mode (development / staging / sandbox / production) |
| `database` | Connection pool config (max_open, max_idle, max_lifetime), migration tool, backup schedule and retention |
| `infrastructure` | Deployment strategy, container registry, Kubernetes namespaces per environment, IaC tool |
| `security` | Audit schedule, vulnerability scan toggle, severity threshold for auto-reject |
| `deployment` | Environment list with approval and rollback flags per environment |

## Memory System

The memory layer stores contracts and history for cross-agent coordination:

| File | Purpose |
|------|---------|
| `api_contract.yaml` | Defines API service contracts and database schemas (tables, columns, indexes, relations). Consumed by dev, PM, and QA agents. |
| `infra_contract.yaml` | Defines infrastructure contracts — environment resources (CPU, memory, replicas), deployment targets (registry, cluster, namespaces), and monitoring/alerting config. |
| `execution_history.json` | Tracks execution history of agent runs for audit and debugging. |
| `quality_history.json` | Tracks QA scoring history over time for trend analysis. |

## QA Scoring System

Quality is scored across 11 categories totaling 100 points. The pass threshold is **80**.

| Category | Weight | What It Validates |
|----------|--------|-------------------|
| Architecture | 15 | System design, separation of concerns, patterns |
| Transaction | 10 | Transaction management, data consistency |
| Testing | 15 | Unit tests, integration tests, coverage |
| Observability | 10 | Logging, tracing, metrics |
| APM | 5 | Application performance monitoring |
| Security | 10 | Auth, input validation, encryption |
| Performance | 10 | Response times, resource usage, optimization |
| Code Quality | 5 | Clean code, naming, documentation |
| Database | 10 | Migration compliance, schema standards, query optimization, connection pooling |
| Infrastructure | 5 | Dockerfile compliance, K8s manifests, CI/CD pipeline, secret management |
| Security Audit | 5 | Vulnerability scan, dependency check, credential exposure |

**Fail conditions** (automatic QA failure):
- Migration without a reversible `down` function
- Dockerfile running as root user
- Hardcoded secrets or credentials in repository
- Kubernetes manifest missing resource limits

## Deployment Strategies

The blueprint supports three deployment strategies, configurable in `config.yaml` under `infrastructure.deployment_strategy`:

| Strategy | Description |
|----------|-------------|
| **Blue-Green** | Two identical environments. Traffic switches from blue (current) to green (new) for zero-downtime releases. Instant rollback by switching back. |
| **Canary** | Gradual rollout to a small subset of users first. Monitor metrics, then expand to full rollout. Minimizes blast radius. |
| **Rolling** | Incrementally replaces old instances with new ones. No extra environment needed. Automatic rollback on failure. |

All strategies require:
- Rollback enabled per environment (configurable in `deployment.environments`)
- Production deployments require approval (`approval_required: true`)
- Health checks (liveness, readiness, startup probes) must pass before traffic routing

## Contributing

To extend this blueprint:

### Adding a New Rule File
1. Create a Markdown file in `kiro/rules/` following the format of existing rule files.
2. Reference the new rule file in relevant agent `instructions`.
3. Update `qa_rules.md` to include a validation checklist for the new domain.

### Adding a New Agent
1. Create a YAML file in `kiro/agents/` with: `role`, `instructions`, `checks`, `rules`, and `output_schema`.
2. Reference the relevant rule files in the agent's instructions.
3. Add the agent to the appropriate flow(s) in `kiro/flows/`.
4. Update this README to document the new agent.

### Adding a New Flow
1. Create a YAML file in `kiro/flows/` defining the `steps` (agent sequence).
2. Include retry loops where quality gates are needed.
3. Always end with `techwriter_agent` → `orchestrator_agent`.
4. Update this README to document the new flow.

### Updating Scoring
1. Update weights in `config.yaml` under `quality.scoring` — total must equal 100.
2. Update `qa_rules.md` scoring section to match.
3. Update `qa_agent.yaml` scoring section to match.
4. Update the blueprint main document scoring table to match.
