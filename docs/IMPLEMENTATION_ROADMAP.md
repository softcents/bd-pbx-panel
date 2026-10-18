# Custom SoftCents UX — Implementation Roadmap

## Phase 0 — Source access and baseline

**Gate:** Original FS PBX source is accessible.

- Confirm repository, branch, framework versions, deployment target, and test commands.
- Read project-specific instructions and existing design guide.
- Run baseline build, lint, and tests without modifying production code.
- Map routes, APIs, models, permissions, tenant scoping, FreeSWITCH integration, and existing UI.
- Save a source-backed feature inventory and identify critical flows.
- Stop and report blockers if code access or a safe test environment is missing.

**Deliverable:** Audit report, baseline results, feature matrix, risk register.

## Phase 1 — Custom UX architecture

- Design a task-oriented SoftCents experience rather than copying another PBX interface.
- Map every current feature to a clear destination.
- Design role-specific workspaces and navigation.
- Define Easy Mode vs Advanced Mode behavior.
- Define white operational UI tokens and reusable components.
- Review the UX plan before broad implementation.

**Gate:** No feature is orphaned; roles and permissions are mapped.

## Phase 2 — Shared application shell

- Implement shared page shell, task-oriented navigation, global search where supported, breadcrumbs, page titles, alerts, forms, tables, pagination, filters, status indicators, and loading/empty/error states.
- Keep existing route names and business logic intact.
- Make navigation responsive and keyboard accessible.
- Use real data and existing services; do not substitute hard-coded demo metrics.

**Gate:** Existing pages still load and authorization remains effective.

## Phase 3 — Migrate existing screens incrementally

- Organize existing screens into Phone System, Contact Center, Campaigns, Messaging, Analytics, Customers & Billing, and Administration, adapting after audit.
- Migrate one coherent module at a time.
- Preserve API contracts and validation behavior.
- Verify each module's critical workflows before moving to the next.
- Keep a compatibility route/redirect if necessary and safe.

**Gate:** Per-module regression checklist passes.

## Phase 4 — Advanced contact-center improvements

Only implement features supported by actual code and telephony APIs:
- Agent/team/skill workflows.
- Supervisor wallboard and authorized monitoring.
- Campaign workflows and import validation.
- Safe retries, DNC, concurrency, and abandonment controls.
- Recording/QA, CRM context, messaging integrations, and reports where data/API support exists.

**Gate:** No fake actions; each feature has a verified integration and tests.

## Phase 5 — SIP gateway resilience

- Audit existing gateway and routing configuration.
- Add health checks and observability before automated switching.
- Configure failure threshold 1 and stable recovery confirmation of 60 seconds as requested.
- Include prioritized retries, safe failback, per-tenant controls, audit events, and operational status.
- Test using a staging FreeSWITCH environment before production rollout.

**Gate:** Failure/recovery/failback tests pass; production behavior is not silently changed.

## Phase 6 — Security, accessibility, and performance

- Verify authorization at backend boundaries.
- Test cross-tenant access denial.
- Check secrets handling, webhook verification, input validation, CSRF/XSS protections, and audit redaction.
- Test keyboard navigation, focus, contrast, mobile layout, and large tables.
- Review query performance, pagination, caching, and real-time update load.

**Gate:** Security and performance issues are documented and blocking defects resolved.

## Phase 7 — Release readiness

- Run full automated tests and manual telephony regression suite.
- Verify migrations and backup/rollback steps.
- Prepare staging deployment, smoke tests, monitoring, and rollback plan.
- Produce a change log, test report, known limitations, and operator notes.
- Deploy only with explicit approval.

## Required status reporting

After each phase, report:
1. Completed work and exact files changed.
2. Tests run and results.
3. What remains and why.
4. Risks or owner decisions required.
5. Whether any feature/API/schema/telephony behavior changed.
6. Rollback steps for risky changes.

## Current blocker

The new `softcents/bd-pbx-panel` repository is a specification scaffold. The actual FS PBX application source must be made available before Phases 0–3 can be completed safely.