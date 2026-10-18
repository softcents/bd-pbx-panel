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

## Mandatory end-to-end workflow requirement

Do not stop at an Issabel-like theme/sidebar. Recreate the familiar operational sequence across the actual FS PBX capabilities:

1. Add a SIP trunk/gateway with provider details, safe credential handling, registration/health status and useful diagnostics.
2. Add the provider's phone numbers/DIDs as separate objects where supported; clearly distinguish numbers from gateways.
3. Create an extension, generate/configure SIP credentials using existing backend logic, optionally associate a device, and configure voicemail where supported.
4. Connect inbound DID/number to an inbound route and a valid tenant-owned destination.
5. Configure outbound route/pattern, route priority, selected gateway/trunk and caller-ID/permission policy.
6. Build supported call flows such as ring groups, queues, IVR, time conditions and voicemail.
7. Verify setup with truthful status, call-history/diagnostics and a checklist; never claim a test call succeeded without evidence.

Use Issabel as a UX/workflow reference only. Keep FreeSWITCH as the call engine and preserve the current app's backend, APIs, DB, auth, roles, tenant isolation, dialplan, CDRs, recordings, jobs and integrations. Audit the real source before implementing. Use only existing working capabilities; no fake controls, dead pages or invented API fields. Separate DID/phone-number records from SIP gateway/trunk records. Show confirmation and safe previews before disruptive live changes. Easy Mode can guide users through the same sequence but must not remove any Advanced Mode functionality.

Acceptance test: a tenant administrator should be able to add a gateway, add/associate a DID, create an extension, assign inbound routing, configure outbound routing and verify the setup using a discoverable Issabel-like workflow, without crossing tenant boundaries.


## Product target: 3CX-level simplicity with enterprise-grade breadth

Use 3CX as the usability benchmark and Issabel as a reference for familiar PBX administration flows. Build an original FS PBX experience, not a clone. FreeSWITCH remains the telephony engine.

### Two modes, one complete product
- Quick Setup / Easy Mode: guided workflows, plain-language labels, validated defaults, contextual help, progress checklist, global search and a clear next step.
- Advanced Mode: every supported configuration option, detailed routing, granular permissions, diagnostics and operational controls.
- Both modes share the same records and permissions. Switching modes must not lose work. Easy Mode may simplify presentation but must never delete or permanently hide features.

### Make the common workflow effortless
Gateway/trunk → phone number/DID → extension and device → inbound route → outbound route → IVR/ring group/queue → verify status and call results. Explain unfamiliar terms, validate inputs, show a clear next step and provide useful recovery guidance. A wizard must not be the only way to configure anything.

### Advanced capability ambition
Where supported by the source and implemented with tests, target multi-tenant/reseller hierarchy, granular role/feature permissions, advanced contact center and supervisor tools, progressive/IVR campaigns, gateway health checks and safe prioritized failover/failback, billing/tariffs/wallets, audit logs, APIs/webhooks, monitoring and integrations. Audit the code first and classify each feature as implemented, partial or missing. Never present a target as a working feature before the backend, permissions, UI and tests are complete.

Preserve current APIs, database behavior, authentication, tenant isolation, dialplan, CDR, recordings, background jobs and integrations. Do not copy proprietary code or branding, and do not introduce Asterisk-only behavior. Preview, confirm, audit and provide a recovery path for disruptive live-routing changes.


## Advanced Billing, Finance & Profit/Loss Panel — mandatory product scope

Build the FS PBX as a business operations panel, not only a telephony settings UI. Use a clean white, 3CX-easy operational experience with Issabel-familiar PBX navigation and a dedicated finance area. Audit existing billing/payment code first; integrate with existing APIs and data models wherever possible. Do not replace working logic blindly or show fake financial figures.

### Finance dashboard
Provide date-range and tenant/reseller/customer/provider filters, with clear totals and drill-down:
- Gross sales/revenue, collected payments, unpaid invoices/accounts receivable, refunds/credits, discounts and adjustments.
- Telephony/provider costs, SMS/voice costs where these services exist, gateway/provider charges, platform/hosting costs and manually entered operating expenses.
- Gross profit, operating expenses, net profit/loss, profit margin, revenue vs cost, and comparison with the previous period.
- Separate cash movement from earned revenue and expenses. Show the calculation definitions and data freshness; never count wallet top-ups as earned revenue twice.

### Billing and accounting functions
- Customer, reseller and tenant-specific tariffs/rate plans; effective dates, per-service rates, currency, taxes/fees where configured, and auditable rate changes.
- Usage rating for CDR/call duration, destinations, provider route and applicable tariffs; support SMS/Voice SMS or other products only where the platform actually has those modules and usage data.
- Invoice generation, invoice items, due dates, partial/full payments, credit notes/refunds, adjustments, receipts, payment reconciliation, overdue tracking and export/print.
- Wallet/prepaid balance and postpaid credit limits, low-balance alerts, usage caps and policy-controlled suspension/warnings. Resellers collect from their own customers according to assigned permissions; platform owners can see consolidated totals without violating tenant boundaries.
- Provider cost records and reconciliation against actual usage/invoices; distinguish estimated cost from confirmed provider cost.
- Record expense categories such as hosting/VPS, number rental, carrier/provider invoices, staff, payment fees and other operating expenses. Allow role-restricted manual entries with evidence/reference, notes and audit trail.
- Payment gateway integration (including PayStation only if existing credentials/API integration is configured) must be idempotent, verify callbacks/signatures, reconcile transactions and never mark a payment paid based solely on a browser redirect.
- Role permissions for Finance, Super Admin, Reseller and Customer Admin; users see only permitted tenants, invoices, balances and reports.

### Financial reports
Include date filters, tenant/reseller/customer/provider filters, drill-down to source transactions, CSV/XLSX/PDF export where supported, and clear generated-at timestamps:
1. Profit & Loss: revenue by product/service, direct costs, gross profit, operating expenses, net profit/loss and margin.
2. Revenue and collection: invoiced vs collected, outstanding, overdue aging, refunds/credits and bad-debt adjustments if supported.
3. Usage and cost: call minutes, destinations, route/provider, billable usage, rated customer revenue, provider cost, margin and unbilled usage.
4. Reseller performance: sales, customer collections, reseller commissions/discounts, payable/receivable and margin.
5. Provider reconciliation: usage, expected cost, invoice cost, variance and unresolved items.
6. Wallet/ledger: opening balance, top-ups, debits, credits, adjustments and closing balance with immutable transaction references.
7. Payments and expenses: gateway status, fees, refunds, expense category, supplier and supporting reference.
8. Tax reports only for configured jurisdictions and tax rules; do not invent tax liability.

### Ledger and calculation correctness
Use a transaction ledger with unique IDs, timestamps, tenant ownership, currency, source type/reference, debit/credit or equivalent balanced entries, actor, and audit history. Do not silently edit/delete posted financial transactions; use reversing entries/credit notes with reasons. Use decimal-safe money arithmetic, explicit rounding rules, currency awareness, idempotency keys and database transactions. Define accounting treatment clearly: wallet funding is a liability/balance movement until consumed according to the selected accounting policy; do not double-count it as service revenue. Separate invoice revenue, cash collection, provider cost accrual, actual provider bills and operating expenses. Report provisional P&L when costs are estimates and finalized P&L only when the selected period's data is reconciled.

### Security, safety and acceptance tests
- Enforce server-side RBAC and tenant scoping on every query, export and mutation; never trust UI filters for authorization.
- Audit rate changes, invoice posting, refunds, balance adjustments, expense entries, payment reconciliation and permission changes.
- Test duplicate payment callbacks, partial payments, refunds, rounding, cross-tenant access denial, wallet top-up vs revenue accounting, invoice/ledger reconciliation, missing provider cost, time-zone/date boundaries and P&L totals against known fixtures.
- No hardcoded/demo totals. If a report lacks data, show zero/unknown/estimated with a clear explanation rather than fabricating figures.
