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