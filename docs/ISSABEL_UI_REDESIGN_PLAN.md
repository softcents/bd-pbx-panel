# FS PBX Issabel-Inspired UI Redesign Plan

## 1. Product principle

Use Issabel's recognizable PBX information architecture to make FS PBX easier to navigate. Do not copy Issabel branding, and do not assume Asterisk-specific commands, APIs, or modules are valid for FreeSWITCH. The existing FS PBX codebase and actual runtime behavior are authoritative.

**Feature preservation is mandatory:** no existing feature, route, API, permission, database field, FreeSWITCH configuration, or working telephony behavior may be removed just to simplify the UI.

## 2. Audit-first gate

Before implementation, inspect and document:

- Framework and versions, frontend entry points, layouts, components, styles, and build/test commands.
- Route inventory, controllers, API endpoints, request/response contracts, validation, and authentication/authorization middleware.
- Database schema, migrations, models, tenant boundaries, and billing/ledger behavior.
- FreeSWITCH event handling, ESL/API integration, SIP profiles, gateways, registrations, dialplan, CDRs, recordings, queues, and call control.
- Existing modules, settings, feature flags, background jobs, webhooks, and third-party integrations.
- Existing tests, CI, deployment process, and known limitations.

Deliver an audit report and a complete feature-to-navigation mapping before modifying application code. Mark each capability as implemented, partially implemented, externally dependent, or unsupported. Never represent a mock/demo action as a working telephony feature.

## 3. Navigation information architecture

Adapt labels and groups to the actual features found during the audit. Suggested grouping:

- **Dashboard:** overview appropriate to the signed-in role.
- **PBX:** Extensions, Devices, SIP Trunks/Gateways, Inbound Routes, Outbound Routes, Time Conditions, Ring Groups, Queues, IVR, Voicemail, Recordings, CDR/Call History.
- **Contact Center:** Agents, Teams, Queues, Skills, Supervisor, Live Calls, Monitoring, Dispositions, QA, Wallboard.
- **Campaigns:** Outbound Campaigns, Preview/Progressive/Power/Predictive Dialing (only when truly supported), IVR Broadcast, Schedules, Imports, DNC, Results.
- **Messaging:** SMS, Voice SMS, templates, delivery reports, webhooks, chatbot/channel integrations where configured.
- **Reports:** CDR, queue/service-level, agent, campaign, gateway, billing and usage reports according to available data.
- **Reseller & Billing:** tenants/customers, plans/tariffs, usage, wallet/ledger, invoices, payment status and provider configuration according to existing permissions.
- **System:** users/roles, tenant settings, security/firewall, provisioning, backups, integrations, audit logs, platform health.

Do not create empty menu items or claim unsupported capabilities. Map all existing features into the final information architecture, including features not listed above.

## 4. Role-specific dashboards

Define navigation and data visibility from the existing authorization model, then verify each role server-side:

1. Super Admin
2. Company Admin
3. Reseller
4. Customer Admin
5. Supervisor
6. Agent
7. Finance/Billing
8. NOC/Operations

A hidden menu is not an authorization boundary. Backend policies must continue enforcing tenant isolation and permissions. A tenant administrator may grant only permissions allowed by the platform's role/permission policy.

## 5. Visual system

- White primary surfaces, quiet gray backgrounds and dividers, restrained indigo accent.
- Compact operational typography and dense but readable data tables.
- Persistent/collapsible side navigation, clear page titles, breadcrumbs, filters, pagination, and predictable forms.
- Responsive layouts for desktop, tablet, and mobile.
- Consistent loading, empty, success, validation, error, offline, and permission-denied states.
- Avoid marketing-style gradients, glassmorphism, oversized decorative metric cards, and unnecessary UI clutter.
- Reuse the existing repository's DESIGN.md and component conventions where applicable.

## 6. Easy Mode and Advanced Mode

- Easy Mode provides guided common workflows and simpler labels.
- Advanced Mode exposes detailed SIP, routing, dialplan, queue, campaign, and operational settings that are actually supported.
- Switching modes must not delete, reset, hide irreversibly, or overwrite configuration.
- Preserve the existing configuration model and permissions. Add confirmation and audit events for risky operations.

## 7. Advanced contact-center requirements

Audit support before implementing each capability. Where supported, provide:

- Inbound IVR, business-hours/holiday routing, queues, ring strategies, skills, overflow, callbacks, VIP handling and blacklists.
- Agent presence, pause reasons, dispositions, team/skill assignment and campaign assignment.
- Supervisor live wallboard, listen/whisper/barge, coaching and queue interventions, gated by real FreeSWITCH support and explicit permissions.
- Outbound preview/progressive/power/predictive/manual and IVR-only campaigns where supported; CSV import validation, DNC, retry policies, concurrency limits, and safe abandonment controls.
- Call recording, access controls, retention policies and QA scorecards.
- CRM/customer context, missed-call follow-up and screen pop where integrations exist.
- SMS, Voice SMS, WhatsApp, Messenger and web chat only through configured and supported providers/APIs.
- Reports for service level, ASA, AHT, abandon rate, agent performance, campaign outcomes and billing only when source data supports the calculation.

## 8. Automatic SIP gateway failover

If included in the source platform, audit and preserve existing gateway configuration. The requested target behavior is:

- Failover enabled by default only if explicitly configured; never silently change a customer's production routing.
- Primary gateway plus prioritized backup gateway(s).
- Consecutive health-check failure threshold: **1**.
- Recovery confirmation: **60 seconds** of stable health before failback.
- Monitor FreeSWITCH gateway status, SIP OPTIONS/registration health where applicable, and actual call failures.
- Prioritized retry, loop prevention, safe failback, multi-tenant settings, audit logs, clear customer-friendly status, and test cases.
- Distinguish signaling health from real call success; do not rely on registration status alone.
- No destructive gateway, routing, or dialplan changes without validation and rollback strategy.

## 9. API, database and telephony compatibility

- Preserve existing API paths, payload contracts, database semantics, and integrations.
- Prefer presentation-layer changes and existing service methods over backend rewrites.
- Any unavoidable contract/schema change requires a documented migration, backward-compatibility plan, tests, and rollback.
- Never place secrets, SIP passwords, API tokens, phone lists, or customer recordings in source control or logs.
- Use tenant-scoped authorization and audit sensitive changes.
- Do not execute arbitrary FreeSWITCH commands from untrusted user input.

## 10. Verification and acceptance criteria

- Every audited existing feature has a destination in the new navigation or a documented reason why it cannot be surfaced.
- Existing routes, APIs, authentication, authorization, billing and tenant isolation pass regression tests.
- Existing call flows, SIP registration, inbound/outbound routing, IVR, queues, CDRs and recordings pass environment-appropriate tests.
- Gateway failover tests cover primary failure, backup selection, actual call failure, recovery stability, safe failback, and multi-tenant isolation.
- UI tests cover all eight roles, Easy/Advanced Mode, responsive navigation, validation, loading/error states and keyboard accessibility.
- Build, static checks, and automated tests pass; report any tests that could not be run.
- No feature is declared complete without evidence from implementation and tests.

## 11. Required deliverables

1. Repository audit report.
2. Feature inventory and old-to-new menu mapping.
3. Role/permission matrix.
4. UI design tokens and reusable components.
5. Incremental implementation plan and small reviewable changes.
6. Automated tests and regression results.
7. Deployment/rollback instructions and known limitations.

## 12. Current blocker

This repository is a newly created, empty project scaffold. Import or connect the actual FS PBX source before attempting a source-code audit or implementation. Do not generate a replacement PBX from scratch or fabricate integrations in place of the existing system.
