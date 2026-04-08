# 🧠 GLOBAL RULES (NON-NEGOTIABLE)

- MUST NOT break existing API contract
- MUST NOT modify unrelated files
- MUST follow naming conventions
- MUST NOT duplicate logic (reuse existing code)
- MUST analyze existing code before making changes

---

# 🔄 CHANGE MANAGEMENT

- MUST maintain backward compatibility
- MUST update related components when changing contract
- MUST ensure consistency across layers

---

# 🧾 CODE QUALITY

- MUST follow clean architecture principles
- MUST keep functions small and readable
- MUST follow single responsibility principle

---

# 🔐 SAFETY RULES

AI MUST NOT:
- bypass architecture layers
- introduce breaking changes silently
- remove existing functionality

AI MUST:
- respect all defined rules
- follow flow strictly
- ensure generated code is maintainable

---

# 🛠️ PR AUTOMATION

## Commit Rules

- MUST follow conventional commit format:
  - feat(scope): description
  - fix(scope): description
  - refactor(scope): description

---

## PR Rules

PR MUST include:
- feature summary
- list of changes
- QA score
- checklist (test, swagger, logging, APM)

---

## PR Restrictions

- PR MUST NOT be created if QA status = FAIL
- PR MUST NOT be created if score < threshold
- PR MUST include only relevant changes

---

# 🗂️ GIT HYGIENE RULES

## .gitignore
- .gitignore MUST exist in project root
- MUST ignore: build artifacts, node_modules, vendor/, .env files, IDE configs (.idea/, .vscode/), compiled binaries, OS files (.DS_Store, Thumbs.db)
- MUST NOT commit generated files (swagger output, compiled assets) unless explicitly required

## Commit Hygiene
- MUST NOT commit large binary files directly to repository
- MUST NOT commit media files (images, videos) without LFS or external storage
- MUST NOT commit credentials, tokens, or secrets (enforced by security_agent)
- commit history MUST be clean — no WIP or fixup commits in main/production branches

## Branch Naming
- MUST follow convention: feature/, bugfix/, hotfix/, release/, refactor/
- branch name MUST be descriptive (e.g., feature/user-authentication, bugfix/fix-login-error)

---

# 📊 QUALITY ENFORCEMENT

- All features MUST pass QA validation
- All features MUST meet minimum quality score
- Low-quality output MUST be rejected and reprocessed

---

# 🚨 FINAL RULE

IF ANY RULE IS VIOLATED:
→ FEATURE MUST BE REJECTED
→ MUST RETURN TO DEV FOR FIX