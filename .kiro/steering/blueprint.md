# Enterprise Blueprint Steering

Kamu bekerja di project yang menggunakan AI-Driven Starter Blueprint enterprise-grade.
Seluruh pekerjaan HARUS mengikuti aturan, konvensi, dan standar yang didefinisikan di blueprint ini.

## Rules yang HARUS Diikuti

Baca dan ikuti SEMUA rules berikut sebelum menulis atau memodifikasi kode:

- #[[file:kiro/rules/global_rules.md]] — aturan global (arsitektur, git hygiene, PR, code quality)
- #[[file:kiro/rules/backend_rules.md]] — aturan backend (arsitektur, transaction, testing, logging, security, performance)
- #[[file:kiro/rules/frontend_rules.md]] — aturan frontend (accessibility, design system, i18n, bundle optimization)
- #[[file:kiro/rules/database_rules.md]] — aturan database (migration, schema, indexing, connection pooling, query optimization)
- #[[file:kiro/rules/infra_rules.md]] — aturan infrastruktur (Docker, Kubernetes, CI/CD, secrets, monitoring, deployment)
- #[[file:kiro/rules/qa_rules.md]] — aturan QA (scoring 11 kategori, fail conditions, feedback loop)

## Agents yang Tersedia

Pahami peran setiap agent dan gunakan sesuai konteks:

- #[[file:kiro/agents/dev_agent.yaml]] — implementasi fitur (backend, database, infrastructure modes)
- #[[file:kiro/agents/qa_agent.yaml]] — validasi kualitas (scoring 11 kategori, total 100)
- #[[file:kiro/agents/reviewer_agent.yaml]] — code review (early_review, final_review)
- #[[file:kiro/agents/pm_agent.yaml]] — definisi fitur dan requirements
- #[[file:kiro/agents/planner_agent.yaml]] — perencanaan arsitektur dan task breakdown
- #[[file:kiro/agents/optimizer_agent.yaml]] — analisis performa (database, frontend, infra, caching)
- #[[file:kiro/agents/orchestrator_agent.yaml]] — koordinasi flow dan finalisasi
- #[[file:kiro/agents/dba_agent.yaml]] — validasi database (migration, schema, indexing)
- #[[file:kiro/agents/devops_agent.yaml]] — validasi infrastruktur (Docker, K8s, CI/CD, secrets)
- #[[file:kiro/agents/security_agent.yaml]] — audit keamanan (backend, frontend, database, infra)
- #[[file:kiro/agents/techwriter_agent.yaml]] — dokumentasi (README, FEATURE_STATUS, swagger)

## Flows yang Tersedia

Gunakan flow yang sesuai dengan jenis pekerjaan:

- #[[file:kiro/flows/feature_flow.yaml]] — untuk fitur baru
- #[[file:kiro/flows/hotfix_flow.yaml]] — untuk perbaikan urgent
- #[[file:kiro/flows/refactor_flow.yaml]] — untuk refactoring
- #[[file:kiro/flows/migration_flow.yaml]] — untuk perubahan database schema
- #[[file:kiro/flows/deployment_flow.yaml]] — untuk deployment ke environment
- #[[file:kiro/flows/security_audit_flow.yaml]] — untuk audit keamanan menyeluruh

## Contracts dan Memory

Referensikan contracts untuk memastikan konsistensi:

- #[[file:kiro/memory/api_contract.yaml]] — API contract dan database schemas
- #[[file:kiro/memory/infra_contract.yaml]] — infrastructure contract (environments, deployment targets, monitoring)
- #[[file:kiro/config.yaml]] — konfigurasi project (quality threshold, scoring weights, retry, database, infra, security, deployment)

## Project Intake

Jika ada file `project_intake.yaml` di root project, baca file tersebut sebagai definisi project:
- Gunakan section `project` untuk konteks bisnis
- Gunakan section `tech_stack` untuk menentukan arsitektur
- Gunakan section `core_features` untuk menentukan fitur yang perlu dibangun
- Gunakan section `roadmap` untuk menentukan fase pengembangan
- Desain database schema, API endpoints, dan folder structure berdasarkan deskripsi bisnis di intake
- JANGAN minta user definisikan detail teknis — Kiro yang menentukan berdasarkan rules

## Prinsip Kerja

1. SELALU baca rules yang relevan sebelum menulis kode
2. SELALU ikuti konvensi yang sudah ada di project
3. Perubahan pada file existing HARUS additive (tidak menghapus yang sudah ada)
4. Scoring weights HARUS konsisten di 4 file: config.yaml, qa_rules.md, qa_agent.yaml, blueprint.md
5. Setiap fitur HARUS melewati QA validation (score >= threshold)
6. Migration HARUS reversible (up dan down function)
7. TIDAK BOLEH hardcode credentials atau secrets
8. TIDAK BOLEH commit file besar, generated files, atau sensitive data ke repository
