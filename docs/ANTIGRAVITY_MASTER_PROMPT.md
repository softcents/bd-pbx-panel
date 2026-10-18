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


## Billing modes: Prepaid, Postpaid and Hybrid (all mandatory)

Implement all three billing modes as first-class, configurable options. Mode may be set at tenant/customer level and, for Hybrid, independently per charge category/service where the billing architecture supports it. Every change must be permission-controlled, effective-dated, audited and must not retroactively alter already posted transactions.

### Prepaid
- Customer/reseller funds a wallet before consuming billable services.
- Rate actual eligible usage against the applicable tariff and debit the wallet exactly once using idempotent ledger transactions.
- Show current balance, reserved/pending usage if supported, transaction history, low-balance thresholds and alerts.
- Configure low-balance warning, top-up options, optional auto-top-up only with explicit customer authorization, usage caps and a defined insufficient-balance policy.
- Do not allow negative balances unless an explicitly authorized policy permits a bounded grace balance.

### Postpaid
- Record billable usage and rate it against the effective tariff; aggregate into the selected billing cycle (monthly by default, with other cycles only if implemented).
- Generate itemized invoices with service period, due date, tax/fees where configured, payments, credits, balance due and source usage references.
- Support credit limit, soft warnings, grace period, overdue reminders and configurable restricted/suspended-service actions with explicit policy and audit history.
- Distinguish usage incurred, invoiced amount, collected cash and outstanding receivable. A generated invoice must not create a second revenue entry for the same usage.

### Hybrid
- Allow a customer to have recurring fixed charges (e.g., extension subscription, DID rental, feature add-on) and independently select prepaid wallet or postpaid invoicing for eligible variable usage (e.g., call charges; SMS/Voice SMS only if those modules are integrated).
- Define billing mode by charge category/service, with a clear summary of what is charged to wallet and what appears on the invoice.
- Prevent double charging when usage events are rated or retried. Monthly recurring fees must follow their own billing schedule and must not be charged again on every usage event.
- Support proration for plan changes only if explicitly implemented, with a visible and tested calculation policy.

### Common billing rules and acceptance tests
- Persist mode, effective date, tariff version, currency, rounding policy, invoice cycle, credit limit, low-balance threshold and service-action policy where applicable.
- Use decimal-safe money arithmetic, atomic ledger writes, idempotency keys and immutable references. Posted transactions are corrected by reversals/credit notes, not silent edits.
- Keep wallet top-ups distinct from earned service revenue and distinguish estimates from reconciled provider costs.
- Enforce tenant and role authorization on all screens, API requests, jobs and exports.
- Test prepaid debit/refund and insufficient balance; postpaid rating, invoice, partial payment and credit limit; hybrid recurring fees plus usage; plan/rate changes; duplicate CDR/event processing; rounding; reversals; and cross-tenant access denial.
- UI must let an authorized admin select Prepaid, Postpaid or Hybrid, explain each mode in plain language, configure per-service rules, preview example charges and show a confirmation summary before applying changes. Never show a mode as working until the backend and tests support it.


## Enterprise Billing v2 — advanced revenue management requirements

Treat billing as a modular telecom revenue-management subsystem, not a few wallet and invoice screens. Audit current source code and data models first. Integrate with existing behavior, preserve API compatibility, and label every requirement as implemented/partial/missing before building. No demo data or false feature claims.

### A. Product/service catalog and commercial plans
- Catalog of recurring and usage-based products: extensions, DID/number rental, PBX plans, feature add-ons, call usage and SMS/Voice SMS only when those modules and usage sources are integrated.
- Plan templates, custom plans, bundles/allowances, included minutes/messages, overage rates, one-time setup fees, recurring fees, discounts, promo codes and contract/effective dates where implemented.
- Versioned tariff tables by tenant, reseller, customer, destination/prefix, service, time band and provider route where relevant. Define rating precedence, tie-breaks and effective dates explicitly; never change historical usage pricing when a tariff is edited.
- Price preview/simulator using sample usage and explainable line-item breakdown before publishing a rate plan.

### B. Rating and mediation pipeline
- Normalize CDR and other usage events into a canonical event format with unique source ID, tenant/customer, service, start/end, billable duration/units, route/provider, destination and source status where available.
- Validate, deduplicate, quarantine malformed/late events, retry safely and expose an unbilled/failed-rating queue.
- Configurable rounding and billing increments, minimum charge, connection fee, free seconds/allowances and destination classification where supported. Keep raw usage immutable and retain rating version, tariff version, calculation trace and rated line item.
- Support rerating only through controlled adjustment/reversal workflows with before/after comparison and audit; never silently rewrite posted invoices or ledger entries.
- Separate customer sell rate, estimated provider cost, confirmed provider invoice cost, and realized margin. Mark cost/margin as estimated until reconciliation completes.

### C. Credit, risk and service policy
- Prepaid balance, optional reservations for in-progress usage only if technically reliable, low-balance thresholds, customer-authorized auto-top-up, configurable overdraft/grace amount, and safe insufficient-funds behavior.
- Postpaid credit limit, real-time exposure where usage events support it, warning thresholds, payment terms, aging buckets, overdue policy and authorized grace/suspension workflow.
- Risk controls: unusual spend/minute spike alerts, repeated failed payment alerting, duplicate CDR/payment detection, per-tenant spending caps, destination/category restrictions if integrated, and manual review queues. No opaque automatic fraud accusations or unreviewed destructive action.
- Service restriction must be explicit, reversible, tenant-aware, auditable, and safe for active calls/emergency or legally protected service paths where applicable. Show impact and require authorization before applying.

### D. Invoicing, payments and collections
- Invoice lifecycle: draft → reviewed/posted → partially paid/paid/overdue/voided via approved reversal; itemized usage and recurring charges, service period, due date, taxes/fees only under configured rules, credit notes, receipts and statements.
- Support partial payments, overpayments/credits, refunds, disputes, write-offs only with authorized policy, payment allocation and reconciliation, and downloadable customer statements.
- Payment integrations must verify provider signatures/server-side status, use idempotency, store provider references, reconcile asynchronous callbacks, and never trust browser redirects as proof of payment.
- Support payment collection reports, aging and reminder rules; prevent repeated reminder storms and log delivery/result where supported.

### E. Reseller and multi-tenant commercial hierarchy
- Platform → reseller → customer hierarchy with tenant-scoped visibility and explicit financial ownership.
- Separate customer sell tariff from reseller buy/transfer price; support margin/commission/discount policies and effective dates. Do not assume reseller collections automatically become platform revenue.
- Track reseller wallet/credit, transfers, customer collections if represented in this system, commissions, receivables/payables and settlement statements. Define who is merchant of record and who owes each balance.
- Tenant-specific invoice branding and numbering only where supported, with no cross-tenant data leakage. Enforce RBAC on API, jobs, exports and reports, not only UI.

### F. General ledger, costs and accounting integrity
- Double-entry or equivalent balanced accounting ledger with chart of accounts and mapping rules for service revenue, deferred/unearned revenue where relevant, wallet/customer liabilities, receivables, provider cost payable, cash/payment clearing, tax payable and operating expenses.
- Immutable posted journal entries; corrections via reversal/credit/debit adjustment with actor, reason, timestamp and reference. Period close/lock, reopening only with privileged approval and audit.
- Distinguish cash collected, invoice revenue, recognized service revenue, wallet funding/liability, provider usage accrual, confirmed provider bill and operating expense. Avoid double counting top-ups, invoices, payment receipts and usage.
- Multi-currency only if exchange rates, base currency, conversion timestamp, rounding and realized FX gain/loss are fully modeled; otherwise explicitly enforce a single configured currency.
- Tax reports only when tax jurisdiction/rules are configured; never invent tax rates or claim tax compliance without validation.

### G. Provider cost reconciliation and margin
- Import or enter provider bills with invoice reference, period, service, amount and currency; match to rated usage/CDRs where data is available.
- Report expected cost vs billed cost, unmatched usage, unmatched bill lines, duplicate charges, rate variance and margin by provider/route/destination/customer/reseller/service.
- Track carrier deposits, number rental and recurring provider commitments as separate costs when configured.

### H. Finance dashboards and reports
- P&L by period, tenant, reseller, customer, service and provider: recognized revenue, direct cost, gross profit, operating expenses, net profit/loss and margin.
- Cash flow/collections, accounts receivable aging, accounts payable, invoice register, wallet liabilities, ledger/journal, payment reconciliation, expense register, tax summaries (configured only), reseller settlement and usage/rating exceptions.
- Drill down every aggregate to source invoices, payments, usage records, provider bills and journal entries. Exports must honor same filters and authorization as the screen. Display timezone, currency, filters, generated-at time and provisional/final status.
- Forecasting is explicitly a forecast based on stated assumptions, separate from actuals. Never mix estimated future values into historical actual P&L.

### I. Operational controls and audit
- Fine-grained permissions for rate editing, invoice posting, refunding, wallet adjustment, expense approval, period close and report export; optional dual approval for high-value refunds/adjustments.
- Audit rate changes, billing-mode changes, manual charges, usage rerating, payment reconciliation, journal postings, refunds, expense edits and service restrictions.
- Background jobs must be idempotent, observable, retryable and alert on stuck rating, invoice runs or reconciliation. Provide dry-run/preview for bulk billing and a resumable run report.
- Data retention, backup/restore and export strategy for financial ledgers and invoice artifacts.

### J. Required automated tests and rollout
- Golden fixtures for rating and P&L; invoice-to-ledger and payment-to-ledger reconciliation; prepaid debit exactly once; postpaid invoice exactly once; hybrid recurring fee plus usage without duplicate charge.
- Duplicate/late CDRs, malformed records, rerating, partial payment, refund/reversal, wallet top-up treatment, rate versioning, timezone/day-boundary, rounding, credit limit, provider invoice variance and cross-tenant authorization tests.
- Load tests for monthly rating/invoice runs, migration/backfill dry run, reconciliation report, feature flags and rollback plan. Never mutate live billing or routing in production without explicit reviewed migration and backup.
