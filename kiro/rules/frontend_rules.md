# 🚀 Frontend Rules — Enterprise-Grade (Kiro)

## 🧠 CORE PRINCIPLES
- maintain consistency > speed
- UI/UX accessibility must be respected
- reusability and componentization

## 📦 COMPONENT RULES
- every component MUST be reusable
- props and state MUST be typed (TypeScript)
- no business logic in UI components
- separate presentational vs container components
- follow naming conventions consistently

## 🧩 STATE MANAGEMENT
- use global state management (Redux, Zustand, Pinia) where required
- local state MUST remain local
- avoid prop drilling
- side effects handled via hooks/middleware

## 🔄 API INTEGRATION
- use service layer for API calls
- API response MUST be validated
- errors MUST be handled gracefully
- retries SHOULD be implemented for critical endpoints
- context / request ID MUST propagate if backend provides tracing

## 🧪 TESTING
- every component MUST have unit tests (Jest/Testing Library)
- integration tests required for key workflows
- UI snapshots SHOULD be updated carefully
- edge cases MUST be covered

## 📊 OBSERVABILITY
- logging of errors or events should exist
- any errors in UI must be traceable to backend request_id
- performance metrics should be collected (LCP, FID, etc.)

## 🔐 SECURITY
- input validation for forms
- prevent XSS, CSRF
- sensitive data MUST NOT be logged
- secure local storage / cookies

## 🔁 IDEMPOTENCY / UX
- critical actions MUST be idempotent (no duplicate clicks)
- confirmation for destructive actions
- optimistic UI updates must reconcile with backend

## ⚡ PERFORMANCE
- lazy load components when possible
- avoid unnecessary re-renders
- code-splitting and chunking should be applied
- heavy assets MUST be optimized

## 🧠 CODE QUALITY
- follow single responsibility principle
- avoid duplicate logic
- keep components small and readable
- follow style guide (Prettier, ESLint)

## 🔐 SAFETY GUARDRAILS
- AI MUST NOT break existing UI
- AI MUST NOT overwrite unrelated files
- AI MUST follow all rules above
- AI MUST generate test and ensure observability


---

# === ENTERPRISE-GRADE ENHANCEMENT ===

---

# 🏗️ MICRO-FRONTEND ARCHITECTURE

- MUST use module federation or similar approach for independent deployment
- each module MUST be deployable independently
- shared dependencies MUST be managed centrally
- inter-module communication MUST use defined contracts
- MUST NOT create tight coupling between modules

---

# 🎨 DESIGN SYSTEM

- MUST use design tokens for colors, spacing, typography
- MUST maintain a centralized component library
- MUST document all components (Storybook or equivalent)
- components MUST follow consistent API patterns
- design token changes MUST propagate across all modules

---

# ♿ ACCESSIBILITY (ENHANCED)

- MUST use semantic HTML elements
- MUST include ARIA attributes where semantic HTML is insufficient
- MUST support full keyboard navigation
- color contrast ratio MUST be minimum 4.5:1 for normal text
- MUST support screen readers
- focus management MUST be implemented for dynamic content

---

# 🌐 INTERNATIONALIZATION (i18n)

- MUST use an i18n library for all user-facing text
- translation strings MUST be separated from code
- MUST support RTL (right-to-left) layout
- date, number, and currency formatting MUST be locale-aware
- MUST NOT hardcode user-facing strings in components

---

# 🖥️ SSR / SSG

- MUST consider SSR for SEO-critical pages
- MUST consider SSG for static content pages
- initial load performance MUST be optimized via server rendering where applicable
- hydration MUST be handled correctly to avoid mismatches

---

# 📦 BUNDLE OPTIMIZATION (ENHANCED)

- MUST enable tree shaking
- MUST implement code splitting per route/module
- MUST use lazy loading for non-critical components
- maximum bundle size limit MUST be configured and enforced
- MUST analyze bundle size in CI pipeline

---

# 🛡️ ERROR BOUNDARY

- every route MUST have an error boundary
- every critical module MUST have an error boundary
- error boundary MUST render a fallback UI
- error boundary MUST report errors to monitoring system
- MUST NOT expose stack traces to end users

---

# 🚩 FEATURE FLAG INTEGRATION

- MUST use a feature flag system for new features
- feature flags MUST support toggle without deployment
- MUST clean up stale feature flags periodically
- MUST NOT leave permanent feature flag conditionals in code

---

# 🧪 E2E TESTING

- MUST use Cypress or Playwright for E2E tests
- minimum coverage: happy path and error path for critical user journeys
- E2E tests MUST run in CI pipeline
- MUST NOT depend on external services (use mocks/stubs)

---

# 📸 VISUAL REGRESSION TESTING

- MUST implement screenshot comparison for critical components
- MUST implement screenshot comparison for main pages
- visual regression tests MUST run in CI pipeline
- baseline screenshots MUST be version controlled
