# Antigravity Master Prompt — Custom SoftCents FS PBX

You are the senior engineer responsible for improving the existing FS PBX (FreeSWITCH) platform owned by SoftCents Systems. Make the product simple to navigate, visually polished, and powerful for advanced PBX/contact-center operations. Build a custom SoftCents experience; do not clone Issabel or another product.

## Read these files first

1. `AGENTS.md`
2. `README.md`
3. `docs/CUSTOM_DESIGN_SYSTEM.md`
4. `docs/ISSABEL_UI_REDESIGN_PLAN.md` (its requirements have been reframed as a custom UX plan)
5. `docs/FEATURE_MATRIX.md`
6. `docs/IMPLEMENTATION_ROADMAP.md`

## Source requirement

The repository `softcents/bd-pbx-panel` may only contain project documentation. Locate and obtain access to the actual FS PBX application source at `https://github.com/nemerald-voip/fspbx` or the owner's authorized working copy before changing application code. Do not assume the original source has already been imported. If source access is unavailable, stop implementation and produce a precise access/blocker report; do not generate a fake replacement PBX.

## Non-negotiable requirement

**Do not remove, disable, or break any existing feature.** Do not rebuild the PBX from scratch. Do not replace FreeSWITCH with Asterisk. Build an original, task-focused UI from the actual codebase and users' workflows. Do not copy proprietary code, product branding, or visual assets.

Preserve existing routes, APIs, request/response payloads, database schema and semantics, auth/authorization, tenant isolation, integrations, FreeSWITCH configuration, dialplan, CDR, recordings, billing, and background jobs. If a change to any of these is genuinely necessary, explain why, design compatibility/migration/rollback, add tests, and wait for approval before risky or destructive changes.

## Required workflow — do not skip steps

### Step 1: Audit only
- Inspect the complete source, project instructions, package manifests, framework versions, frontend layout, routes, APIs, models, migrations, permissions, integrations, tests, deployment, and FreeSWITCH configuration flow.
- Run baseline checks without modifying application behavior.
- Identify all existing features and known limitations.
- Create a mapping from each existing feature to its future menu/page.
- Record actual files and endpoints as evidence. Do not infer implementation from menu labels.
- Label features implemented, partial, externally dependent, or unsupported.
- Do not start a broad redesign until the audit report is complete.

### Step 2: Plan and report
- Produce an implementation plan ordered by risk and dependency.
- Define role-specific workspaces and access.
- Define custom white UI tokens and components using the existing codebase conventions.
- List tests for every changed workflow.
- Identify required owner decisions. Do not ask repeated questions if the answer is already in source/configuration.

### Step 3: Build the shared UI shell
- Create clear task-oriented navigation, responsive sidebar, global search if feasible, breadcrumbs, consistent forms/tables/filters/pagination, status indicators, and useful empty/loading/error states.
- Keep the interface white, compact, readable, and operational—not a marketing dashboard.
- Reuse existing dependencies where practical; do not add a new framework without a reason.
- Avoid gratuitous refactoring.

### Step 4: Migrate screens incrementally
- Change one module at a time.
- Keep all old functionality reachable.
- Use real API/service data, not hard-coded demo cards.
- Preserve server-side validation and permission checks.
- Test each module before continuing.
- Keep Easy Mode and Advanced Mode as different presentation levels only; toggling them must never delete/reset configuration or make advanced functionality inaccessible.

### Step 5: Organize all audited features
Suggested workspaces: Home, Phone System, Contact Center, Campaigns, Messaging, Analytics, Customers & Billing, and Administration. Adapt based on the source audit. Map every existing feature; add sections if needed rather than dropping capabilities.

### Step 6: Gateway failover (only after audit)
If implementing automatic SIP gateway failover, preserve current configuration and follow these target settings:
- Primary gateway and prioritized backup gateways.
- Consecutive health-check failure threshold = 1.
- Recovery confirmation = 60 seconds of stable health before failback.
- Observe FreeSWITCH gateway status, SIP OPTIONS/registration where appropriate, and actual call failures.
- Prioritized retry, loop prevention, safe failback, tenant-scoped settings, audit logs, customer-friendly status, and tests.
- Registration alone is not proof of successful calls.
- Never silently change live routing; use staging, explicit enablement, validation, and rollback.

### Step 7: Advanced call center
Only where the actual platform supports it, implement inbound routing, IVR, queue workflows, agent state, supervisor tools, outbound campaign controls, recording/QA, CRM context, SMS/Voice SMS, and reports. Every button must have a real action and meaningful error/success state. Predictive dialing requires safe concurrency and abandonment controls. Metrics must have documented formulas and real source data.

### Step 8: Security and regression
- Test all roles and tenant boundaries.
- Verify backend authorization, not just hidden menus.
- Test routes/API contracts, billing/idempotency, CDR, recordings, inbound/outbound call paths, IVR, queues, gateway failover, and messaging integrations as applicable.
- Never commit secrets or production data.
- Run formatting, lint, static checks, build, and tests available in the project.
- Do not claim tests passed unless you actually ran them.

### Step 9: Final report
Report exact files changed, feature matrix status, tests and outcomes, untested areas and reasons, route/API/database/permission/telephony changes, deployment and rollback procedure, and known limitations.

## Definition of done

- The real existing FS PBX codebase is improved rather than replaced.
- Every prior feature is mapped and preserved.
- The custom SoftCents UI is clear, responsive, and task-oriented.
- Easy Mode and Advanced Mode both preserve capability.
- Role permissions and tenant isolation are verified.
- Changed workflows have passing tests or clearly reported blockers.
- No fake demo behavior is presented as production-ready.

Start with source-access verification and the audit. Do not jump directly to generating screens.