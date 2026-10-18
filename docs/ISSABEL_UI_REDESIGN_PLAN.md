# FS PBX Issabel-Inspired UI Redesign Plan

## 1. Product principle

Use Issabel PBX's familiar navigation and operational patterns to make FS PBX easier to use. This is an Issabel-inspired interface for FreeSWITCH, not an Asterisk conversion. Keep branding original and preserve the existing implementation.

**Feature preservation is mandatory:** no existing feature, route, API, permission, database field, FreeSWITCH configuration, or working telephony behavior may be removed just to simplify the UI.

## 2. Audit-first gate

Before implementation, inspect and document:
- Framework and versions, frontend entry points, layouts, components, styles, and build/test commands.
- Route inventory, controllers, API endpoints, request/response contracts, validation, and authentication/authorization middleware.
- Database schema, migrations, models, tenant boundaries, and billing/ledger behavior.
- FreeSWITCH event handling, ESL/API integration, SIP profiles, gateways, registrations, dialplan, CDRs, recordings, queues, and call control.
- Existing modules, settings, feature flags, background jobs, webhooks, third-party integrations, tests, CI, deployment, and known limitations.

Deliver an audit report and complete feature-to-navigation mapping before modifying application code. Mark each capability as implemented, partially implemented, externally dependent, or unsupported. Never represent a mock/demo action as a working telephony feature.

## 3. Issabel-inspired navigation model

Adapt menu labels to the actual source implementation. Suggested structure:

- **Dashboard**
- **PBX:** Extensions, Devices, SIP Trunks/Gateways, Inbound Routes, Outbound Routes, Time Conditions, Ring Groups, Queues, IVR, Voicemail, Recordings, CDR/Call History.
- **Applications:** application modules actually present in FS PBX.
- **Contact Center:** Agents, Teams, Skills, Supervisor, Live Calls, Monitoring, Wallboard, Dispositions, QA.
- **Campaigns:** supported dialer modes, import validation, schedules, DNC, retries, concurrency, and results.
- **Messaging:** SMS, Voice SMS, templates, provider configuration, delivery reports, and configured chatbot/channel integrations.
- **Reports:** call, queue, agent, campaign, gateway, usage, and billing reports backed by real source data.
- **Reseller & Billing:** tenants, resellers, plans/tariffs, usage, wallet/ledger, invoices, and payments where supported.
- **System:** users/roles, tenant settings, security, provisioning, backups, integrations, audit logs, and platform health.

Use Issabel's menu organization as the reference, but do not drop or mislabel features that exist in FS PBX. Add menu groups when needed.

## 4. UX and visual direction

- Familiar top-level menu and nested submenu patterns.
- Clean white primary workspace, pale gray surfaces/dividers, readable dark text, restrained accent color.
- Compact operational typography, tables, filters, pagination, practical forms, status indicators, breadcrumbs, and clear confirmations.
- Responsive navigation for desktop, tablet, and mobile.
- Avoid marketing-style dashboards, unnecessary decorative cards, gradients, glassmorphism, and excessive animation.
- Use the current FS PBX design system and components where practical; do not add a new framework without a justified reason.

## 5. Easy Mode and Advanced Mode

- Easy Mode provides guided common workflows and sensible defaults.
- Advanced Mode exposes detailed SIP, routing, dialplan, queue, campaign, and operational settings that are genuinely supported.
- Mode selection changes presentation only. It must not delete, reset, overwrite, or irreversibly hide configuration.
- Respect server-side permissions in both modes.

## 6. Role-specific workspaces

Define role-appropriate landing pages for Super Admin, Company Admin, Reseller, Customer Admin, Supervisor, Agent, Finance/Billing, and NOC/Operations. Base access on the actual authorization model. A hidden menu is not an authorization boundary; enforce tenant isolation in backend policies.

## 7. Advanced contact-center requirements

Audit support before implementing each capability. Where supported, provide inbound IVR, business-hours/holiday routing, queues, skills, overflow, callbacks, VIP handling, agent presence/pause reasons, supervisor wallboard and authorized listen/whisper/barge, supported outbound dialer modes, CSV validation, DNC, retries, concurrency and abandonment safeguards, recording/retention controls, QA scorecards, CRM context, missed-call follow-up, configured messaging channels, and reports based on verified source data.

## 8. Automatic SIP gateway failover

If supported and requested:
- Preserve existing gateway configuration.
- Primary gateway plus prioritized backups.
- Consecutive health-check failure threshold: **1**.
- Recovery confirmation: **60 seconds** of stable health before failback.
- Monitor FreeSWITCH gateway status, SIP OPTIONS/registration where appropriate, and actual call failures.
- Prioritized retry, loop prevention, safe failback, tenant-scoped settings, audit logs, clear UI status, and tests.
- Registration alone is not proof of successful calls.
- Never silently change production routing; validate and test with a rollback plan.

## 9. Compatibility, security, and acceptance

Preserve API paths/contracts, database semantics, integrations, auth, tenant isolation, and telephony behavior. Prefer UI-layer changes. Any necessary contract/schema change requires a documented migration, backward compatibility, tests, rollback, and owner approval. Never commit secrets or execute arbitrary FreeSWITCH commands from untrusted input.

Acceptance:
- Every audited feature maps to a menu destination.
- Existing routes, APIs, auth, billing, tenant isolation, and call flows pass regression tests.
- UI tests cover roles, modes, responsiveness, validation, and keyboard accessibility.
- Report exact test results and any untested areas.
- Provide audit, feature matrix, role/permission matrix, design tokens, incremental implementation, test report, and rollback instructions.

## 10. Current blocker

This repository is currently a project specification/scaffold, not the FS PBX application source. Connect or import the actual FS PBX source before attempting source-code audit or implementation. Do not generate a replacement PBX from scratch or fabricate integrations.

## 9. End-to-end Issabel-style PBX setup workflow (required)

The goal is not merely similar colors or a sidebar. The complete operator workflow must feel familiar to someone who already uses Issabel: add external telephony connectivity, create extensions, connect endpoints, define inbound/outbound call handling, then assemble IVR/queues and verify calls. Use the actual capabilities discovered in the existing FS PBX source; do not invent fields or pretend an unsupported backend action works.

### Guided setup order

1. **Numbering and provider connectivity**
   - Add/edit a SIP trunk or gateway with a clear provider name, host/domain, port, transport, authentication mode, username/auth ID, secret, registration mode, caller-ID defaults, codecs and other fields only where supported by the current backend.
   - Where the provider supplies public DID/phone numbers, manage them as numbers/DIDs associated with the relevant tenant and provider. Clearly distinguish a DID/phone number from a SIP gateway/trunk; do not treat them as the same object.
   - Validate required fields, show a masked credential summary, provide safe test/status actions if supported, and show registration/reachability state with last-check time and useful error details.
   - Never expose secrets after save, log secrets, or apply unreviewed live routing changes automatically.

2. **Create extensions**
   - Offer a familiar Extensions list and “Add Extension” flow with extension number, display/name, tenant/domain, endpoint/device association and voicemail options where supported.
   - Generate or configure SIP credentials using existing FS PBX logic. Show the secret only when safely generated/displayed initially; never reveal it again from stored configuration.
   - Detect duplicate extension numbers within the correct tenant scope and explain validation errors inline.
   - Provide a safe way to copy/download endpoint configuration only if implemented; do not invent provisioning functionality.

3. **Connect numbers to call handling**
   - Inbound: DID/number → inbound route → destination (extension, ring group, queue, IVR or other supported destination).
   - Outbound: extension/tenant → outbound route/pattern → prioritized gateway/trunk → caller-ID policy and permissions.
   - Explain route priority and matching patterns in plain language; provide a summary before applying changes.
   - Validate destination existence and tenant ownership before saving. A number must not silently route into another tenant.

4. **Build call flows**
   - Use familiar forms and destination selectors for ring groups, queues, IVR/virtual receptionist, time conditions, voicemail, call forwarding and recordings, where present in the source.
   - Make the workflow discoverable through PBX and Applications menus, with breadcrumbs, clear save/cancel actions and status feedback.
   - A guided wizard may help new users, but every setting must remain available in the normal advanced screens.

5. **Verify and troubleshoot**
   - Provide a setup checklist: gateway status, extension/device status where available, inbound route match, outbound route match, test-call result where safely supported, and recent CDR/log references.
   - Distinguish configuration saved from service reloaded and from a real successful call. Never show “test successful” without evidence.
   - Use safe validation and preview/dry-run capabilities if the backend supports them; require explicit confirmation for disruptive reloads or live routing changes.

### Issabel-style information architecture

Use a stable left navigation with familiar PBX groupings, then map every real feature from the source:
- **PBX:** Extensions, Devices, SIP Trunks/Gateways, Numbers/DIDs, Inbound Routes, Outbound Routes, Ring Groups, Queues, IVR, Time Conditions, Voicemail, Call Forwarding, Recordings, CDR/Call History.
- **Applications:** Virtual Receptionist/IVR and other application modules actually supported.
- **Contact Center / Campaigns / Messaging:** only real modules found in the codebase.
- **Reports:** CDR, usage, recordings and operational reports actually implemented.
- **Reseller & Billing:** tenant/reseller plans, rates, usage and payments only where existing modules/APIs support them.
- **System:** users/roles, domains/tenants, provisioning, firewall and platform settings actually supported.

Menu labels may be refined after the source audit. Do not create dead links or placeholder pages.

### Acceptance criteria
- A first-time tenant admin can follow the visible sequence to add a gateway, create an extension, attach a device, assign an inbound DID destination and configure an outbound route without guessing which screen comes next.
- Existing features remain reachable in Advanced Mode; Easy Mode must not remove or irreversibly hide them.
- All writes use existing authorization, tenant scoping, backend validation and APIs wherever possible.
- Existing database schemas and API contracts remain backward-compatible unless a separately reviewed migration is essential.
- Automated tests cover gateway validation, duplicate extensions, tenant isolation, route destination validation, and regressions in existing call handling.


## 10. Product UX direction: 3CX-level simplicity, beyond-3CX capability

Treat 3CX as a usability benchmark and Issabel as a familiar PBX configuration/navigation reference. The product goal is not a visual clone: deliver a coherent, easy-to-learn interface over the existing FreeSWITCH platform, with a broader advanced feature set where the real backend can support it.

### Two layers, one complete product
- **Quick Setup / Easy Mode:** task-oriented guided flows, plain-language descriptions, sensible validated defaults, progress checklist, contextual help, global search and a clear next step.
- **Advanced Mode:** exposes every supported setting, detailed routing/permissions, diagnostics, operational controls and expert configuration.
- Both modes must use the same underlying configuration and permissions. Easy Mode must never delete, reset, or permanently hide features. Users can switch modes without losing work.

### Primary tasks must be exceptionally easy
1. Add a SIP gateway/trunk and confirm registration/health.
2. Add a phone number/DID and map it to a tenant and provider.
3. Create an extension, securely configure SIP credentials, and associate a device.
4. Configure inbound number routing and outbound dialing rules.
5. Create an IVR, ring group or queue using familiar destination selectors.
6. Test/verify configuration with honest status and diagnostics.
7. Manage users, permissions, working hours, voicemail, recordings and call history.
8. For authorized roles, operate queues/campaigns, supervise calls, review reports and manage reseller billing.

Use guided task cards and a setup checklist, but do not make a wizard the only way to configure the system. Keep lists, search, filters, bulk actions (only where safe), inline validation, clear save/cancel, breadcrumbs, and actionable error messages consistent.

### Advanced capabilities strategy
Aim to exceed a typical small-business PBX in breadth through modular, permission-controlled capabilities: multi-tenant/reseller hierarchy; granular role and feature permissions; contact center with supervisor listen/whisper/barge where technically supported; progressive and IVR campaigns where implemented; multi-provider gateway routing and safe automatic failover; usage/tariff/wallet billing; audit trail; API/webhooks; monitoring; data exports; and integrations. These are target capabilities, not permission to display fake modules. Audit what exists, classify each item as implemented/partial/missing, and build in tested phases.

### Non-negotiable compatibility
FreeSWITCH remains the call engine. Preserve current APIs, database behavior, authentication, tenant boundaries, dialplan and live call safety. Do not copy 3CX/Issabel branding or proprietary source. Do not convert Asterisk-specific commands or configuration into the FreeSWITCH implementation. Changes affecting live routing must be previewed, confirmed, audited and recoverable.


## 11. Usability benchmark: 3CX-simple, advanced beyond a basic PBX

Use 3CX as the ease-of-use benchmark and Issabel as the familiar PBX workflow reference. Create an original FS PBX UI. Prioritize Quick Setup for Gateway/Trunk → Number/DID → Extension/Device → Inbound/Outbound Routes → IVR/Queue → Verification. Use plain language, validated defaults, a visible checklist, contextual help, global search, breadcrumbs and actionable errors. Keep all advanced options in Advanced Mode and ensure switching modes preserves data and permissions. Advanced breadth should include multi-tenant/reseller operations, granular permissions, contact center, campaigns, gateway failover, billing, audit, APIs and monitoring only as each capability is confirmed in source and fully implemented. No fake pages or controls. FreeSWITCH remains the engine; preserve APIs, DB, auth, tenant isolation and live routing safety. Never copy proprietary code/branding or introduce Asterisk-only behavior.


## 12. Business panel: advanced billing, finance and profit/loss

FS PBX must include a dedicated business/finance workspace alongside PBX administration. Keep the visual language clean, white, easy to scan and fast for daily operations. The UI should be approachable like 3CX while exposing detailed financial controls to authorized roles.

### Dashboard layout
- KPI cards: revenue, collected, receivables/overdue, provider cost, operating expenses, gross profit, net profit/loss and margin.
- Trend charts: revenue vs cost, profit by period, service mix and collection trend.
- Recent activity: payments, invoices, refunds, rate changes, provider-cost imports and expense entries.
- Filters: date range, tenant, reseller, customer, provider and service; all values must be sourced from real records.
- Click-through from every KPI/report row to the supporting invoice, payment, usage record, CDR or expense entry.

### Advanced billing modules
1. Tariff/rate-plan management with tenant/reseller/customer scope, effective dates and audit history.
2. CDR usage rating, customer charge calculation and provider cost comparison.
3. Invoice, invoice items, due dates, partial payments, receipts, credit notes, refunds and overdue aging.
4. Prepaid wallet and postpaid credit/limit policies, alerts, controlled adjustments and full ledger history.
5. Reseller sales/collections, assigned customer pricing, commission/discount handling and receivable/payable views.
6. Provider usage and invoice reconciliation; distinguish estimated from confirmed cost.
7. Expense management for hosting, carriers, numbers, payment fees and other operating costs.
8. Payment gateway transaction reconciliation and safe webhook handling; integrate only configured, supported gateways.
9. Role-controlled exports and audit history.

### Profit & Loss rules
- Show revenue, direct service costs, gross profit, operating expenses, net profit/loss and margin for a selected period.
- Separate invoiced revenue from cash received and separate provider usage estimates from reconciled provider invoices.
- Do not double-count wallet top-ups as earned revenue; show wallet movements separately and apply a documented accounting policy.
- Posted transactions must not be silently edited/deleted; use audited reversals, credit notes or adjustments.
- Use decimal-safe currency calculations, explicit rounding and currency-aware records. Show provisional/estimated status when costs or invoices are incomplete.
- Every number must drill down to its underlying records; no sample or hardcoded figures in production.

### Permissions and tests
Enforce server-side access for Super Admin, Finance, Reseller and Customer Admin with tenant boundaries applied to screens, API endpoints and exports. Test reconciliation, duplicate payment callbacks, partial payment/refund, wallet accounting, P&L fixtures, rounding, provider cost variance and cross-tenant access. Financial reports must clearly state filters and generation time.


## 13. Billing mode configuration: Prepaid, Postpaid and Hybrid

The Billing UI must expose three supported modes and explain them in plain language:
- **Prepaid:** fund wallet before usage; rate eligible usage and debit once; show balance, ledger, low-balance alerts, top-up history and insufficient-funds policy.
- **Postpaid:** accumulate rated usage in a billing period; issue itemized invoices with due date, credit limit, payments, credits, overdue state and service policy.
- **Hybrid:** recurring fixed fees (extension subscription, DID rental or enabled add-ons) follow their billing schedule, while eligible variable usage can be charged to wallet or postpaid invoice independently. Clearly show the charge destination for each service.

For each customer/tenant, expose the mode, effective date, applicable tariff, currency, billing cycle, credit limit, low-balance threshold and any authorized service restriction policy. Show a preview before saving, and audit changes. Prevent duplicate charges and double-counting between wallet movements, invoices and revenue. Test all three modes with retries, refunds, partial payments, rate changes and tenant-isolation checks. Do not claim unsupported behavior is implemented.


## 14. Enterprise Billing v2 modules

Extend the billing workspace beyond basic wallets and invoices into a complete, auditable telecom revenue-management area:
- **Catalog & Plans:** recurring/usage products, bundles/allowances, setup fees, discounts and versioned effective-dated tariffs.
- **Usage Rating:** CDR/event mediation, deduplication, late/malformed event handling, explainable calculations, tariff version tracking and unbilled-usage queue.
- **Credit & Risk:** wallet thresholds, authorized auto-top-up, credit limits, exposure alerts, spending caps, review queues and reversible service policies.
- **Invoices & Collections:** invoice lifecycle, partial payments, credits/refunds, payment allocation, overdue aging, statements and reminders.
- **Reseller Settlement:** buy/sell rates, margin/commission, transfers, receivable/payable and settlement statements across a platform→reseller→customer hierarchy.
- **Accounting:** balanced ledger, chart-of-account mapping, immutable posted entries, reversal/adjustment workflow, period close and reconciliation.
- **Provider Reconciliation:** compare rated usage with provider bills, track unmatched items and cost variance, and report actual vs estimated margin.
- **Reports:** P&L, collections/cash movement, receivables aging, payables, wallet liabilities, journal, expenses, reseller settlement, rating exceptions and forecast (separated from actuals).
- **Operations:** permission-controlled approvals, idempotent background jobs, dry-run invoice runs, retry/error reports, audit trails and safe rollout/rollback.

The UI must explain each financial state, show actual/estimated/provisional/final labels, and drill down every total to source transactions. Preserve tenant boundaries in screens, APIs, exports and background jobs. Do not silently change posted charges or historical tariffs; use versioned rates and audited reversals. Implement only after a source audit and add automated reconciliation, duplicate-event, refund, rounding, hybrid-charge and cross-tenant tests.
