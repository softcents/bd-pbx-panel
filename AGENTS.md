# Agent Instructions — SoftCents BD PBX Panel

These instructions apply to all AI coding agents and contributors working in this repository.

## Mission

Prepare and implement an Issabel-inspired, user-friendly UI/UX for the existing FS PBX (FreeSWITCH) platform. Preserve the existing application and all working features. Issabel is a reference for information architecture and ease of use, not a source to copy wholesale or a reason to replace FreeSWITCH with Asterisk.

## Hard rules

1. **Never delete or disable existing features to make the interface look simpler.**
2. **Never replace the existing PBX with a newly generated demo or mock application.**
3. Do not change production routes, API contracts, database schema, authentication, permissions, billing logic, or FreeSWITCH behavior without an audit, justification, tests, migration/compatibility plan, and rollback instructions.
4. Do not claim a feature is implemented merely because a button, card, or screen exists. Every action must connect to real backend behavior or be clearly marked unavailable.
5. Do not invent support for FreeSWITCH commands or Asterisk-only integrations. Verify actual platform support.
6. Do not expose SIP passwords, API tokens, customer data, recordings, or other secrets in source control, logs, screenshots, or test fixtures.
7. Keep strict multi-tenant data isolation. UI visibility is not a substitute for server-side authorization.
8. Never perform destructive changes, production deployments, live telephony changes, database resets, or irreversible migrations without explicit approval.
9. Do not commit vendor code or third-party code copied from Issabel. Respect licenses and attribution.
10. Never remove a menu item or capability before its underlying implementation and dependencies have been traced and documented.

## Required first step

The repository may initially contain only project specifications. Before application implementation:
- Confirm the actual FS PBX source location and obtain access to it.
- Inspect the existing codebase, framework, routes, controllers, APIs, permissions, database, migrations, UI components, tests, deployment, and FreeSWITCH integration.
- Record baseline build/test results.
- Create a feature inventory and old-feature-to-new-menu mapping.
- Identify incomplete features and unsupported integrations honestly.
- Submit the audit and implementation plan before risky edits.

If the original source code is not available, do not pretend the audit is complete. Work only on project specifications, test plans, and scaffolding until source access is provided.

## UI principles

- White, calm operational interface; subtle gray surfaces and dividers; restrained indigo accent.
- Compact, readable tables and practical forms.
- Clear side navigation, breadcrumbs, filters, pagination, status indicators, validation, loading, empty, error, and permission-denied states.
- Responsive behavior for desktop, tablet, and mobile.
- Avoid marketing dashboards, oversized decorative cards, gradients, glassmorphism, and needless animations.
- Follow the existing source repository's design system and component conventions where available.
- Easy Mode may simplify common workflows; Advanced Mode must preserve access to expert settings. Switching modes must never delete or reset configuration.

## Navigation direction

Adapt the following groups to the actual audited feature set:
- Dashboard
- PBX: Extensions, Devices, SIP Trunks/Gateways, Inbound Routes, Outbound Routes, Time Conditions, Ring Groups, Queues, IVR, Voicemail, Recordings, CDR/Call History
- Contact Center: Agents, Teams, Skills, Supervisor, Live Calls, Monitoring, Wallboard, QA
- Campaigns: supported dialer modes, imports, schedules, DNC, retry policy, outcomes
- Messaging: SMS, Voice SMS, provider configuration, delivery reports, chatbot/channel integrations when configured
- Reports
- Reseller & Billing
- System: Users/Roles, Tenant Settings, Security, Provisioning, Backups, Integrations, Audit Logs, Health

Only show capabilities supported by the audited platform, but preserve all existing features even if they need a different menu group.

## Role and tenant rules

Respect the actual authorization model and define/verify views for Super Admin, Company Admin, Reseller, Customer Admin, Supervisor, Agent, Finance/Billing, and NOC/Operations. Verify server-side policy checks for every sensitive read and write. A reseller/customer can manage only their authorized tenant scope and delegated permissions.

## Contact-center safety and correctness

- Verify real FreeSWITCH capability before implementing listen/whisper/barge, queue interventions, or dialer features.
- For predictive dialing, include configurable concurrency and abandonment safeguards and applicable compliance controls.
- Use actual event/CDR/provider data for metrics; document formulas and limitations.
- Prevent duplicate campaigns/messages with idempotency and auditable retry behavior.
- Preserve recording access controls and retention rules.

## SIP gateway failover target

If requested and supported by the source platform, implement only after auditing current gateway/routing logic:
- Explicit enablement; do not silently change production behavior.
- Primary and prioritized backup gateways.
- Consecutive health-check failure threshold: 1.
- Recovery confirmation: 60 seconds of stable health before failback.
- Monitor FreeSWITCH gateway status, SIP OPTIONS/registration health where appropriate, and actual call failures.
- Prioritized retry, loop prevention, safe failback, tenant-scoped settings, audit logs, customer-friendly status, and tests.
- Do not treat registration alone as proof that calls work.

## Quality gates

Before considering a task done:
- Run available formatting, lint, static analysis, build, and automated tests.
- Add or update tests for changed behavior.
- Verify authorization and tenant isolation.
- Check keyboard access and responsive layout for UI changes.
- Report exactly what ran, what passed, what failed, and what could not be tested.
- Provide a file/change summary and rollback notes for risky changes.

## Current repository status

This repository initially contains project documentation, not the complete FS PBX application. The original source must be connected/imported before production-code implementation can be audited or safely changed.