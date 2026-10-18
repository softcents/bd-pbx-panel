# Antigravity Master Prompt — Issabel-Inspired SoftCents FS PBX

You are the senior engineer responsible for improving the existing FS PBX (FreeSWITCH) platform owned by SoftCents Systems. The owner strongly prefers Issabel PBX's interface and navigation. Recreate the familiar Issabel-style information architecture and operational feel while preserving the existing FS PBX application and all working features.

## Read these files first

1. `AGENTS.md`
2. `README.md`
3. `docs/CUSTOM_DESIGN_SYSTEM.md`
4. `docs/ISSABEL_UI_REDESIGN_PLAN.md`
5. `docs/FEATURE_MATRIX.md`
6. `docs/IMPLEMENTATION_ROADMAP.md`

## Source requirement

The repository `softcents/bd-pbx-panel` may only contain project documentation. Locate and obtain access to the actual FS PBX application source at `https://github.com/nemerald-voip/fspbx` or the owner's authorized working copy before changing application code. Do not assume the original source has already been imported. If source access is unavailable, stop implementation and produce a precise access/blocker report; do not generate a fake replacement PBX.

## Non-negotiable requirement

**Do not remove, disable, or break any existing feature.** Do not rebuild the PBX from scratch. Do not replace FreeSWITCH with Asterisk. Use Issabel as a reference for familiar menu hierarchy, labels, and workflows, while keeping implementation compatible with FreeSWITCH. Do not copy proprietary code, branding, or visual assets.

Preserve existing routes, APIs, request/response payloads, database schema and semantics, auth/authorization, tenant isolation, integrations, FreeSWITCH configuration, dialplan, CDR, recordings, billing, and background jobs. If a change is genuinely necessary, explain why, design compatibility/migration/rollback, add tests, and obtain approval before risky or destructive changes.

## Required workflow

### Step 1: Audit only
Inspect the complete source, project instructions, package manifests, framework versions, frontend layout, routes, APIs, models, migrations, permissions, integrations, tests, deployment, and FreeSWITCH configuration flow. Run baseline checks without changing behavior. Inventory every existing feature, cite actual files/endpoints, mark support status honestly, and map each feature to its future menu. Do not infer implementation from labels.

### Step 2: Plan and report
Produce a risk-ordered plan, role-specific navigation, UI tokens, and tests for each changed workflow. If source access is missing, stop and report the blocker.

### Step 3: Build the Issabel-inspired shell
Create familiar top-level navigation and nested submenus adapted to the actual FS PBX feature set. Suggested sections: Dashboard, PBX, Applications, Contact Center, Campaigns, Messaging, Reports, Reseller/Billing, System. Keep a clean white background, compact tables/forms, responsive navigation, breadcrumbs, clear status/error/loading states, and practical forms. Reuse existing dependencies; avoid gratuitous refactoring.

### Step 4: Migrate screens incrementally
Change one module at a time. Keep all existing functionality reachable. Use real API/service data, preserve server-side validation and permissions, and test each module before continuing. Easy Mode and Advanced Mode change presentation only; neither may delete/reset configuration or make advanced capabilities inaccessible.

### Step 5: Map all features
Map every audited feature into the new menu. Add sections if needed instead of dropping functionality. Do not show unsupported capabilities as working. Use Issabel's information architecture as inspiration, not as a command to replace FreeSWITCH-specific logic.

### Step 6: Gateway failover (only after audit)
If implementing automatic SIP gateway failover, preserve current configuration:
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
Test all roles and tenant boundaries, backend authorization, routes/API contracts, billing/idempotency, CDR, recordings, inbound/outbound call paths, IVR, queues, gateway failover, and messaging as applicable. Never commit secrets. Run available formatting, lint, static checks, build, and tests. Do not claim tests passed unless actually run.

### Step 9: Final report
Report exact files changed, feature matrix status, tests and outcomes, untested areas and reasons, route/API/database/permission/telephony changes, deployment and rollback procedure, and known limitations.

## Definition of done

- The real existing FS PBX codebase is improved rather than replaced.
- Every prior feature is mapped and preserved.
- The interface has familiar Issabel-inspired navigation while remaining FreeSWITCH-compatible.
- Easy Mode and Advanced Mode both preserve capability.
- Role permissions and tenant isolation are verified.
- Changed workflows have passing tests or clearly reported blockers.
- No fake demo behavior is presented as production-ready.

Start with source-access verification and the audit. Do not jump directly to generating screens.