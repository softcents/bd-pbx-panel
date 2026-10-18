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

Define role-appropriate landing pages for Super Admin, Company Admin, Reseller, Customer Admin, Supervisor, Agent, Finance/Billing, and NOC/Operations. Base access on the actual authorization model. A hidden menu is not an authorization boundary. Tenant isolation must be enforced by backend policies.

## 8. Advanced contact-center requirements

Audit support before implementing each capability. Where supported, provide inbound IVR, business-hours/holiday routing, queues, ring strategies, skills, overflow, callbacks, VIP handling, blacklists, agent presence and pause reasons, dispositions, supervisor wallboard and authorized listen/whisper/barge, supported outbound dialer modes, DNC, retries, concurrency limits, safe abandonment controls, recording access/retention, QA scorecards, CRM context, missed-call follow-up, supported messaging channels, and reports based on verified source data.

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

## 10. Compatibility and security

Preserve existing API paths, payload contracts, database semantics, integrations, tenant-scoped authorization, and telephony behavior. Prefer presentation-layer changes over backend rewrites. Any unavoidable schema/contract change requires a documented migration, backward compatibility, tests, rollback, and owner approval. Never commit secrets or execute arbitrary FreeSWITCH commands from untrusted input.

## 11. Verification and deliverables

- Every audited existing feature has a destination or a documented reason it cannot be surfaced.
- Existing routes, APIs, auth, billing, tenant isolation, and call flows pass regression tests.
- UI tests cover roles, Easy/Advanced Mode, responsive navigation, validation, and keyboard accessibility.
- Report exact tests run and any tests that could not be run.
- Deliver the audit, feature matrix, role/permission matrix, custom design tokens, incremental implementation, tests, and deployment/rollback instructions.

## 12. Current blocker

This repository is currently a project specification/scaffold, not the FS PBX application source. Connect or import the actual FS PBX source before attempting a source-code audit or implementation. Do not generate a replacement PBX from scratch or fabricate integrations in place of the existing system.