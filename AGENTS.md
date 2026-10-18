# Agent Instructions — SoftCents BD PBX Panel

These instructions apply to all AI coding agents and contributors working in this repository.

## Mission

Create a custom, task-focused, easy-to-use operations experience for the existing FS PBX (FreeSWITCH) platform. Do not clone Issabel or any other PBX product. Design around the actual users, workflows, and capabilities of this codebase.

## Hard rules

1. **Never delete or disable existing features to make the interface simpler.**
2. **Never replace the existing PBX with a newly generated demo or mock application.**
3. Do not change production routes, API contracts, database schema, authentication, permissions, billing logic, or FreeSWITCH behavior without an audit, justification, tests, migration/compatibility plan, and rollback instructions.
4. Do not claim a feature is implemented merely because a button, card, or screen exists. Every action must connect to real backend behavior or be clearly marked unavailable.
5. Do not invent support for FreeSWITCH commands or Asterisk-only integrations. Verify actual platform support.
6. Do not expose SIP passwords, API tokens, customer data, recordings, or other secrets in source control, logs, screenshots, or test fixtures.
7. Keep strict multi-tenant data isolation. UI visibility is not a substitute for server-side authorization.
8. Never perform destructive changes, production deployments, live telephony changes, database resets, or irreversible migrations without explicit approval.
9. Do not copy third-party source code, product branding, or proprietary visual assets.
10. Never remove a menu item or capability before its underlying implementation and dependencies have been traced and documented.

## Required first step

The repository may initially contain only project specifications. Before application implementation:
- Confirm the actual FS PBX source location and obtain access to it.
- Inspect the existing codebase, framework, routes, controllers, APIs, permissions, database, migrations, UI components, tests, deployment, and FreeSWITCH integration.
- Record baseline build/test results.
- Create a feature inventory and old-feature-to-new-navigation mapping.
- Identify incomplete features and unsupported integrations honestly.
- If the original source code is not available, do not pretend the audit is complete. Work only on project specifications and UX design until source access is provided.

## Custom design principles

- White, calm operational workspace with subtle gray surfaces and restrained accent color.
- Task-oriented navigation that reflects what users need to do, not a copy of a legacy PBX menu.
- Compact, readable tables; focused forms; searchable lists; helpful defaults; clear confirmation for risky actions.
- Responsive layouts for desktop, tablet, and mobile.
- Consistent loading, empty, success, validation, error, offline, and permission-denied states.
- Avoid marketing dashboards, oversized decorative cards, gradients, glassmorphism, and needless animation.
- Follow existing framework, component, accessibility, and design conventions where practical.
- Easy Mode may simplify common workflows; Advanced Mode must preserve access to expert settings. Switching modes must never delete or reset configuration.
- Prefer one clear place for each task, with contextual links for related tasks. Do not duplicate the same settings across confusing menus.

## Suggested workspaces

Adapt these to audited capabilities and permissions:
- **Home:** relevant status, recent activity, actionable alerts, favorites, and quick actions.
- **Phone System:** extensions, devices, gateways, inbound/outbound routing, IVR, ring groups, queues, voicemail, recordings, and call history.
- **Contact Center:** agents, teams, queues, supervisor tools, live activity, wallboard, and quality assurance.
- **Campaigns:** supported outbound dialer modes, imports, schedules, DNC, retry policy, and results.
- **Messaging:** SMS, Voice SMS, provider configuration, delivery reports, and supported chatbot/channel integrations.
- **Analytics:** call, queue, agent, campaign, gateway, usage, and billing reports based on available source data.
- **Customers & Billing:** tenants, resellers, plans, tariffs, usage, wallet/ledger, invoices, and payments where implemented.
- **Administration:** users/roles, tenant settings, security, provisioning, backups, integrations, audit logs, and health.

These are UX groupings, not permission grants. Preserve every existing feature, even if the final navigation needs additional sections.

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