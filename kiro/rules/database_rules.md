# 🗄️ DATABASE RULES (STRICT - NON-NEGOTIABLE)

---

# 🔄 MIGRATION MANAGEMENT

- every schema change MUST use a migration file
- migration file naming format: `YYYYMMDDHHMMSS_description`
- every migration MUST have `up` and `down` functions
- `down` function MUST be reversible (undo the `up` changes)
- migration MUST be idempotent
- migration MUST NOT contain business logic
- migration MUST be tested before deployment

---

# 📐 SCHEMA DESIGN STANDARDS

- every table MUST have a primary key
- every table MUST have `created_at` timestamp column
- every table MUST have `updated_at` timestamp column
- every table MUST have `deleted_at` nullable timestamp column (soft delete)
- naming convention: snake_case for tables and columns
- MUST use appropriate data types (no generic text for structured data)
- MUST define column constraints (NOT NULL, DEFAULT) explicitly

---

# 📊 INDEXING STRATEGY

- MUST create index on every foreign key column
- MUST create index on columns frequently used in WHERE clauses
- MUST create index on columns used in ORDER BY
- MUST use composite index for multi-column queries
- MUST NOT create unnecessary indexes (impacts write performance)
- index naming convention: `idx_{table}_{column(s)}`

---

# 🔗 CONNECTION POOLING

- MUST use connection pool for all database connections
- connection pool MUST be configured with `max_open`, `max_idle`, `max_lifetime`
- pool configuration values MUST come from `config.yaml`
- MUST NOT create ad-hoc connections outside the pool
- MUST handle connection exhaustion gracefully

---

# 💾 BACKUP & RECOVERY

- MUST have scheduled backup strategy (schedule defined in config.yaml)
- backup retention period MUST be defined
- recovery procedure MUST be documented
- backup MUST be verified periodically
- MUST support point-in-time recovery for production

---

# 🔒 DATA INTEGRITY

- foreign key constraints MUST be defined for all relationships
- unique constraints MUST be applied where domain requires uniqueness
- check constraints MUST be used for data validation at DB level
- MUST NOT rely solely on application-level validation
- cascading deletes MUST be explicitly defined and justified

---

# ⚡ QUERY OPTIMIZATION

- MUST NOT use SELECT *
- MUST use pagination for all list queries
- MUST NOT allow N+1 queries
- MUST run EXPLAIN for complex queries before deployment
- MUST use prepared statements for repeated queries
- MUST avoid full table scans on large tables

---

# 🔐 DATABASE SECURITY

- encryption at rest MUST be enabled
- encryption in transit MUST be enabled (TLS)
- access control MUST be role-based
- MUST NOT use superuser credentials for application connections
- MUST audit database access for production environments

---

# 🌱 SEED DATA MANAGEMENT

- seed data MUST be separated per environment (development, staging, production)
- seed data MUST NOT contain production data
- seed data MUST be idempotent (safe to run multiple times)
- seed data MUST be version controlled

---

# 🌐 MULTI-ENVIRONMENT CONFIGURATION

- database configuration MUST be separated per environment
- MUST NOT hardcode credentials in configuration files
- credentials MUST be sourced from Secret_Manager
- connection strings MUST be environment-specific
- MUST support configuration override per environment

---

# 🚨 ENFORCEMENT RULE

IF ANY RULE IS VIOLATED:
→ DBA_Agent MUST FAIL
→ QA MUST FAIL
→ MUST RETURN TO DEV FOR FIX
