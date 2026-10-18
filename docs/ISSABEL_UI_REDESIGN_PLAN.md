# Custom SoftCents FS PBX UX Plan

## 1. Product vision

Create a purpose-designed operations interface for FS PBX that feels simple to new users and powerful to telecom engineers. Do not clone Issabel or another PBX product. Learn from common usability patterns, but build a coherent SoftCents experience based on the real application and user workflows.

**Feature preservation is mandatory:** no existing feature, route, API, permission, database field, FreeSWITCH configuration, or working telephony behavior may be removed just to simplify the interface.

## 2. Audit-first gate

Before implementation, inspect and document:
- Framework and versions, frontend entry points, layouts, components, styles, and build/test commands.
- Route inventory, controllers, API endpoints, request/response contracts, validation, and authentication/authorization middleware.
- Database schema, migrations, models, tenant boundaries, and billing/ledger behavior.
- FreeSWITCH event handling, ESL/API integration, SIP profiles, gateways, registrations, dialplan, CDRs, recordings, queues, and call control.
- Existing modules, settings, feature flags, background jobs, webhooks, and third-party integrations.
- Existing tests, CI, deployment process, and known limitations.

Deliver an audit report and complete feature-to-navigation mapping before modifying application code. Mark each capability as implemented, partially implemented, externally dependent, or unsupported. Never represent a mock/demo action as a working telephony feature.

## 3. Custom navigation model

Use a small number of clear workspaces, with labels based on real user tasks rather than copying a legacy menu:

1. **Home** — role-relevant service health, recent activity, actionable alerts, favorites, and quick actions.
2. **Phone System** — extensions, devices, SIP gateways/trunks, inbound/outbound routes, time conditions, ring groups, queues, IVR, voicemail, recordings, and call history.
3. **Contact Center** — agents, teams, skills, queues, supervisor workspace, live calls, authorized monitoring, wallboard, dispositions, and QA.
4. **Campaigns** — supported dialer modes, import validation, schedules, DNC, retries, concurrency, and results.
5. **Messaging** — SMS, Voice SMS, templates, provider configuration, delivery reports, and configured chatbot/channel integrations.
6. **Analytics** — call, queue, agent, campaign, gateway, usage, and billing reports using verified source data.
7. **Customers & Billing** — tenants, resellers, plans/tariffs, usage, wallet/ledger, invoices, and payments where supported.
8. **Administration** — users/roles, tenant settings, security, provisioning, backups, integrations, audit logs, and platform health.

Adapt the names and groupings after auditing the source. All existing features must have a clear destination; add another section if needed rather than dropping features.

## 4. UX interactions

- Persistent left navigation on desktop; compact drawer on mobile.
- Global search for extensions, users, customers, call records, and settings, scoped by permission.
- Quick actions for frequent safe tasks; require confirmation for risky actions.
- Breadcrumbs, saved filters/favorites, readable tables, bulk actions with explicit scope, and useful empty states.
- Keep forms focused; show advanced options progressively instead of placing every field on one screen.
- Preserve existing validation and display server errors clearly.
- Use live status only when backed by real data and label stale/offline state honestly.
- No hard-coded sample metrics or fake live-call data.

## 5. Visual direction

- White primary workspace, pale gray canvas and dividers, readable dark text, one restrained accent color.
- Compact operational typography and clear hierarchy.
- Consistent spacing, button sizes, field labels, status badges, dialogs, toasts, tables, pagination, and filters.
- Avoid decorative card grids, gradients, glassmorphism, unnecessary animation, and excessive color.
- Make the interface responsive, keyboard accessible, and usable on narrow screens.
- Reuse the existing repository's design system and framework where possible; do not introduce a new framework without a justified reason.

## 6. Easy Mode and Advanced Mode

- Easy Mode prioritizes common safe tasks, guided forms, and sensible defaults.
- Advanced Mode exposes detailed SIP, routing, dialplan, queue, campaign, and operational settings that are genuinely supported.
- Mode selection changes presentation only. It must not delete, reset, overwrite, or irreversibly hide configuration.
- Respect server-side permissions in both modes.
- Risky settings must show their impact and provide a safe confirmation.

## 7. Role-specific workspaces

Define role-appropriate landing pages for:
1. Super Admin
2. Company Admin
3. Reseller
4. Customer Admin
5. Supervisor
6. Agent
7. Finance/Billing
8. NOC/Operations

Base access on the actual authorization model. A hidden menu is not an authorization boundary. Tenant isolation must be enforced by backend policies.

## 8. Advanced contact-center requirements

Audit support before implementing each capability. Where supported, provide:
- Inbound IVR, business-hours/holiday routing, queues, ring strategies, skills, overflow, callbacks, VIP handling, and blacklists.
- Agent presence, pause reasons, dispositions, team/skill assignment, and campaign assignment.
- Supervisor live wallboard, listen/whisper/barge, coaching and queue interventions, gated by real FreeSWITCH support and explicit permissions.
- Supported outbound modes (preview/progressive/power/predictive/manual and IVR-only), CSV import validation, DNC, retries, concurrency limits, and safe abandonment controls.
- Call recording, access controls, retention policies, and QA scorecards.
- CRM/customer context, missed-call follow-up, and screen pop where integrations exist.
- SMS, Voice SMS, WhatsApp, Messenger, and web chat only through configured and supported providers/APIs.
- Reports for service level, ASA, AHT, abandon rate, agent performance, campaign outcomes, and billing only when source data supports the calculation.

## 9. Automatic SIP gateway failover

If included in the source platform, audit and preserve existing gateway configuration. Target behavior:
- Explicit enablement; never silently change a customer's production routing.
- Primary gateway plus prioritized backup gateways.
- Consecutive health-check failure threshold: **1**.
- Recovery confirmation: **60 seconds** of stable health before failback.
- Monitor FreeSWITCH gateway status, SIP OPTIONS/registration health where appropriate, and actual call failures.
- Prioritized retry, loop prevention, safe failback, multi-tenant settings, audit logs, customer-friendly status, and test cases.
- Distinguish signaling health from real call success; do not rely on registration status alone.
- No destructive gateway, routing, or dialplan changes without validation and rollback strategy.

## 10. API, database, and telephony compatibility

- Preserve existing API paths, payload contracts, database semantics, and integrations.
- Prefer presentation-layer changes and existing service methods over backend rewrites.
- Any unavoidable contract/schema change requires a documented migration, backward-compatibility plan, tests, and rollback.
- Never place secrets, SIP passwords, API tokens, phone lists, or customer recordings in source control or logs.
- Use tenant-scoped authorization and audit sensitive changes.
- Do not execute arbitrary FreeSWITCH commands from untrusted user input.

## 11. Verification and acceptance criteria

- Every audited existing feature has a destination in the new navigation or a documented reason why it cannot be surfaced.
- Existing routes, APIs, authentication, authorization, billing, and tenant isolation pass regression tests.
- Existing call flows, SIP registration, inbound/outbound routing, IVR, queues, CDRs, and recordings pass environment-appropriate tests.
- Gateway failover tests cover primary failure, backup selection, actual call failure, recovery stability, safe failback, and tenant isolation.
- UI tests cover all roles, Easy/Advanced Mode, responsive navigation, validation, loading/error states, and keyboard accessibility.
- Build, static checks, and automated tests pass; report any tests that could not be run.
- No feature is declared complete without implementation and test evidence.

## 12. Required deliverables

1. Source-backed repository audit report.
2. Feature inventory and custom navigation mapping.
3. Role/permission matrix.
4. Custom design tokens and reusable components.
5. Incremental implementation plan and reviewable changes.
6. Automated tests and regression results.
7. Deployment/rollback instructions and known limitations.

## 13. Current blocker

This repository is currently a project specification/scaffold, not the FS PBX application source. Connect or import the actual FS PBX source before attempting a source-code audit or implementation. Do not generate a replacement PBX from scratch or fabricate integrations in place of the existing system.