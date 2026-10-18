# Agent Instructions — SoftCents BD PBX Panel

These instructions apply to all AI coding agents and contributors working in this repository.

## Mission

Make the existing FS PBX (FreeSWITCH) interface familiar and easy to navigate by following Issabel PBX's information architecture and interaction patterns. The target is an Issabel-inspired FS PBX—not an Asterisk replacement and not a generic SaaS dashboard.

## Hard rules

1. **Never delete or disable existing features to make the interface simpler.**
2. **Never replace the existing PBX with a newly generated demo or mock application.**
3. Do not change production routes, API contracts, database schema, authentication, permissions, billing logic, or FreeSWITCH behavior without an audit, justification, tests, migration/compatibility plan, and rollback instructions.
4. Do not claim a feature is implemented merely because a button, card, or screen exists. Every action must connect to real backend behavior or be clearly marked unavailable.
5. Do not force Asterisk-only commands or modules into FreeSWITCH. Verify actual platform support.
6. Do not expose SIP passwords, API tokens, customer data, recordings, or other secrets in source control, logs, screenshots, or test fixtures.
7. Keep strict multi-tenant data isolation. UI visibility is not a substitute for server-side authorization.
8. Never perform destructive changes, production deployments, live telephony changes, database resets, or irreversible migrations without explicit approval.
9. Do not copy Issabel branding, proprietary visual assets, or third-party source code.
10. Never remove a menu item or capability before its underlying implementation and dependencies have been traced and documented.

## Required first step

The repository may initially contain only project specifications. Before application implementation:
- Confirm the actual FS PBX source location and obtain access to it.
- Inspect the existing codebase, framework, routes, controllers, APIs, permissions, database, migrations, UI components, tests, deployment, and FreeSWITCH integration.
- Record baseline build/test results.
- Create a feature inventory and old-feature-to-new-menu mapping.
- Identify incomplete features and unsupported integrations honestly.
- If the original source code is not available, do not pretend the audit is complete. Work only on project specifications and UX design until source access is provided.

## Issabel-inspired UI principles

- Use familiar PBX navigation groups and submenu structure inspired by Issabel, adapted to actual FS PBX features.
- Suggested top-level groups: Dashboard, PBX, Applications, Contact Center, Reports, Messaging, Reseller/Billing, and System.
- Use familiar operational patterns for extension/trunk/routing/IVR/queue settings, tables, forms, filters, and status indicators.
- Keep a clean white background, quiet gray surfaces, readable typography, and restrained accent color.
- Prefer practical compact forms and tables over marketing dashboards, decorative card grids, gradients, and glassmorphism.
- Responsive side navigation for desktop, tablet, and mobile.
- Easy Mode may simplify common workflows; Advanced Mode must preserve expert controls. Switching modes must never delete, reset, overwrite, or irreversibly hide configuration.
- Use Issabel as a UX reference only. Do not copy branding/source code or assume Asterisk-specific behavior.

## Feature and navigation preservation

Map every audited existing feature to the new menu. Candidate sections include:
- **Dashboard**
- **PBX:** Extensions, Devices, SIP Trunks/Gateways, Inbound Routes, Outbound Routes, Time Conditions, Ring Groups, Queues, IVR, Voicemail, Recordings, CDR/Call History.
- **Applications:** actual application modules found in source.
- **Contact Center:** Agents, Teams, Skills, Supervisor, Live Calls, Monitoring, Wallboard, QA.
- **Campaigns:** supported dialer modes, imports, schedules, DNC, retry policy, results.
- **Messaging:** SMS, Voice SMS, provider configuration, delivery reports, configured chatbot/channel integrations.
- **Reports:** call, queue, agent, campaign, gateway, usage, and billing reports where data supports them.
- **Reseller/Billing:** tenants, plans, tariffs, usage, ledger, invoices, payments where implemented.
- **System:** users/roles, tenant settings, security, provisioning, backups, integrations, audit logs, platform health.

These are candidate groups, not a reason to omit existing functionality. Add more menu sections where required. Do not show unsupported capabilities as working.

## Role and tenant rules

Respect the actual authorization model and define/verify views for Super Admin, Company Admin, Reseller, Customer Admin, Supervisor, Agent, Finance/Billing, and NOC/Operations. Verify server-side policy checks for every sensitive read and write. A reseller/customer can manage only their authorized tenant scope and delegated permissions.

## Contact-center correctness

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

This repository currently contains project documentation, not the complete FS PBX application. The original source must be connected/imported before production-code implementation can be audited or safely changed.