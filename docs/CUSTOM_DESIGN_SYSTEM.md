# Custom SoftCents PBX Design System

## Design intent

A quiet, professional telecom operations workspace. It should feel straightforward for customers and resellers while retaining the depth engineers and contact-center supervisors need. It is an original SoftCents interface, not a clone of a legacy PBX.

## 1. Visual language

- **Base:** white workspace (`#FFFFFF`), pale gray page canvas (`#F8FAFC`), subtle border (`#E2E8F0`).
- **Text:** dark slate (`#0F172A`) for primary content; muted slate (`#64748B`) for secondary information.
- **Accent:** a restrained blue/indigo (`#4F46E5`) for primary actions and focus states only.
- **Success/warning/error:** use consistent semantic colors and pair them with text/icon labels; never communicate status by color alone.
- **Typography:** use the existing application's font if it is readable and consistent. Avoid adding font dependencies unless justified.
- **Shape:** modest corner radii, thin borders, minimal shadows. No glassmorphism, neon, gradients, or decorative background art.

These are initial tokens. Inspect the source design system first and adapt without unnecessary dependency churn.

## 2. Layout

### Desktop
- A compact left navigation with workspace groups.
- A top utility bar for global search, notifications/status, help, and user/tenant context where supported.
- A main content area with one clear page title, short context, and primary action.
- Tables and forms should use the available width efficiently.

### Mobile/tablet
- Collapse the sidebar into a drawer.
- Keep primary actions accessible without horizontal scrolling.
- Convert wide tables to a responsive table/card treatment only where it remains clear; preserve filters and record identity.

## 3. Navigation model

Use these workspaces as a starting point and adjust after the source audit:

- Home
- Phone System
- Contact Center
- Campaigns
- Messaging
- Analytics
- Customers & Billing
- Administration

Only expose features that exist and are authorized, but never delete an existing capability simply because it does not fit this first list. Add a destination or contextual link as required.

## 4. Interaction rules

- One obvious primary action per page where practical.
- Place related settings together and reveal advanced options progressively.
- Searchable selectors for long lists such as extensions, tenants, agents, and gateways.
- Use confirmation dialogs for destructive or service-impacting actions; show the impact clearly.
- Show save progress, success, validation errors, backend failures, and stale/offline states explicitly.
- Keep filters, pagination, and sorting predictable.
- Do not fake live updates. Label data freshness and connectivity where relevant.
- Use empty states that explain the next valid action.
- Avoid duplicate settings pages and ambiguous labels.

## 5. Easy Mode / Advanced Mode

- Easy Mode: guided flows, common settings, sensible defaults, concise explanations.
- Advanced Mode: full expert settings and diagnostic information allowed by the user's permissions.
- Mode preference can change presentation only; it must not delete, reset, or overwrite configuration.
- Provide a clear path to the advanced setting related to a simplified control.

## 6. Role-based home screens

- **Super Admin:** platform health, tenant overview, service alerts, global administration.
- **Company Admin:** tenant service health, extensions/gateways, team activity, usage.
- **Reseller:** authorized customer portfolio, provisioning, plan/tariff and usage information.
- **Customer Admin:** own extensions, routes, IVR, recordings and usage according to permissions.
- **Supervisor:** live queue/agent state, service level, interventions allowed by policy.
- **Agent:** own presence, assigned queue/campaign, customer context and disposition workflow.
- **Finance/Billing:** usage, ledger, invoices, payments, reconciliation.
- **NOC/Operations:** gateway health, registrations, call failures, alarms, maintenance and audit trails.

This is a target UX, not a substitute for the existing backend's authorization rules.

## 7. Tables and forms

- Keep important identifiers and status visible.
- Provide server-backed pagination for large datasets.
- Preserve server-side validation; render errors next to the corresponding fields.
- Never put secrets into prefilled UI, URLs, or client logs.
- Use explicit units, time zones, currencies, and date ranges.
- Avoid destructive bulk actions without a clear count, scope, and confirmation.

## 8. Accessibility

- Keyboard-operable navigation and dialogs.
- Visible focus indicators.
- Proper labels and accessible names.
- Sufficient text and status contrast.
- Respect reduced-motion preferences if animation exists.
- Do not rely on hover-only interactions.

## 9. Component inventory to map against the existing stack

- Application shell and workspace navigation
- Page header and breadcrumbs
- Search and filter bar
- Data table and pagination
- Form field and validation message
- Status badge and health indicator
- Confirmation dialog
- Toast/alert
- Empty/loading/error states
- Role-aware quick action
- Live operational panel (only when backed by real data)

Reuse existing project components where possible. Do not create a parallel UI framework.

## 10. Acceptance criteria

- A first-time user can find common phone-system tasks without knowing legacy PBX terminology.
- Advanced users retain access to all audited expert settings.
- All existing features remain reachable and authorized.
- The UI remains usable on mobile and keyboard.
- No mock metric or fake button is presented as real functionality.
- All changed workflows pass relevant regression tests.