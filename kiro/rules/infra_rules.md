# 🏗️ INFRASTRUCTURE & DEVOPS RULES (STRICT - NON-NEGOTIABLE)

---

# 🔄 CI/CD PIPELINE

- pipeline stages MUST follow this order: lint → test → build → security scan → deploy-staging → integration-test → deploy-production
- every stage MUST have defined exit criteria
- pipeline MUST fail fast on critical errors
- MUST NOT skip security scan stage
- pipeline configuration MUST be version controlled

---

# 🐳 CONTAINER / DOCKER

- MUST use multi-stage build
- MUST run as non-root user
- MUST enable image scanning
- image size MUST be minimal (alpine base preferred)
- MUST NOT include development dependencies in production image
- MUST use specific image tags (no `latest` in production)
- MUST define HEALTHCHECK in Dockerfile

---

# ☸️ KUBERNETES / ORCHESTRATION

- MUST define resource limits (CPU, memory) for every container
- MUST define liveness probe
- MUST define readiness probe
- MUST define startup probe
- MUST define pod disruption budget
- MUST use namespaces for environment isolation
- MUST NOT use default namespace for workloads

---

# 🌍 ENVIRONMENT MANAGEMENT

- environments MUST be separated: development, staging, production
- configuration MUST be isolated per environment
- MUST NOT share credentials across environments
- environment-specific config MUST be managed via ConfigMaps or equivalent

---

# 🔑 SECRET MANAGEMENT

- MUST use Secret_Manager for all secrets and credentials
- MUST NOT store secrets in repository
- MUST NOT store secrets in environment variables as plaintext
- MUST rotate secrets periodically
- MUST audit secret access

---

# 📝 INFRASTRUCTURE AS CODE

- all infrastructure MUST be defined in code (Terraform/Pulumi)
- state management MUST be configured (remote state)
- infrastructure code MUST be versioned
- MUST use modules for reusable infrastructure components
- MUST NOT make manual infrastructure changes outside IaC

---

# 📊 MONITORING & ALERTING

- metrics collection MUST be enabled for every service
- log aggregation MUST be configured
- alerting rules MUST be defined for critical metrics
- dashboard MUST exist for every service
- MUST define SLIs and SLOs for critical services

---

# 🛡️ DISASTER RECOVERY

- RTO (Recovery Time Objective) MUST be defined
- RPO (Recovery Point Objective) MUST be defined
- backup strategy MUST be documented
- failover procedure MUST be documented and tested
- MUST conduct DR drills periodically

---

# 🌐 NETWORK SECURITY

- network segmentation MUST be implemented
- firewall rules MUST be defined and documented
- TLS termination MUST be configured
- DDoS protection MUST be enabled for public-facing services
- MUST NOT expose internal services to public network

---

# ⚖️ LOAD BALANCING & SCALING

- HPA (Horizontal Pod Autoscaler) MUST be configured
- load balancer configuration MUST be defined
- scaling policy MUST be documented
- MUST define minimum and maximum replica counts
- MUST configure scaling metrics and thresholds

---

# 🚀 DEPLOYMENT STRATEGY

- MUST support Blue_Green_Deployment
- MUST support Canary_Deployment
- MUST support rolling update
- rollback MUST be automatic on failure
- deployment MUST be zero-downtime
- MUST validate health checks after deployment

---

# 🚨 ENFORCEMENT RULE

IF ANY RULE IS VIOLATED:
→ DevOps_Agent MUST FAIL
→ QA MUST FAIL
→ MUST RETURN TO DEV FOR FIX
