# SoftCents FS PBX UI Design System

## Design intent

A clean, white Issabel-inspired PBX operations interface. Preserve the familiar PBX navigation and nested submenu experience the owner likes, while making forms, tables, and workflows easier to use. This is an original FS PBX implementation, not a copy of Issabel branding or source code.

## 1. Visual language

- White primary workspace (`#FFFFFF`), pale gray canvas (`#F8FAFC`), subtle border (`#E2E8F0`).
- Dark slate text (`#0F172A`) and muted secondary text (`#64748B`).
- Restrained blue/indigo accent (`#4F46E5`) for primary actions and focus.
- Semantic status colors paired with text/icons; do not rely on color alone.
- Use the existing application's font and components where possible.
- Modest corner radii, thin borders, minimal shadows. No glassmorphism, neon, decorative gradients, or marketing-style hero panels.

## 2. Navigation and shell

- Use a familiar PBX-style top-level navigation and nested submenus inspired by Issabel.
- Candidate groups: Dashboard, PBX, Applications, Contact Center, Campaigns, Messaging, Reports, Reseller/Billing, System.
- The exact labels and menu items must be mapped to the actual source audit. Preserve all existing features, adding groups as needed.
- Keep current page context clear with page titles and breadcrumbs.
- Responsive navigation for desktop, tablet, and mobile.
- Do not copy Issabel logos, product identity, proprietary assets, or source code.

## 3. Page layout

- Compact operational header with page title and a primary action where appropriate.
- Readable tables with search, filters, pagination, and status.
- Focused forms with sensible grouping, clear labels, validation, and help text.
- Loading, empty, success, warning, offline, error, and permission-denied states.
- Avoid oversized decorative metric cards, excessive whitespace, unnecessary animations, and duplicate settings pages.

## 4. Easy Mode and Advanced Mode

- Easy Mode: common settings, guided forms, sensible defaults, short explanations.
- Advanced Mode: detailed SIP, routing, dialplan, queue, campaign, and diagnostic settings that are actually supported.
- Switching modes changes presentation only; it must not delete, reset, or overwrite configuration.
- Provide a path from a simplified control to the corresponding advanced settings.

## 5. Role-specific workspaces

Target appropriate landing pages for Super Admin, Company Admin, Reseller, Customer Admin, Supervisor, Agent, Finance/Billing, and NOC/Operations. Follow existing authorization policies; a hidden menu is not a security boundary.

## 6. Safety and usability

- Confirm risky or service-impacting actions and explain impact.
- Preserve server-side validation and show errors next to fields.
- Do not prefill or log secrets.
- Use server-backed pagination for large datasets.
- Label stale/offline state honestly; do not fake live updates.
- Ensure keyboard access, visible focus, readable contrast, and narrow-screen usability.

## 7. Acceptance criteria

- Users recognize the PBX navigation pattern and can find common settings quickly.
- All audited existing features remain reachable and permission-protected.
- The design is white, compact, responsive, and consistent.
- Easy Mode and Advanced Mode both retain the complete capability set.
- No mock metric or fake button is presented as production functionality.
- Changed workflows pass relevant regression tests.

## Usability benchmark: 3CX-simple, enterprise-capable

Design for quick comprehension: white uncluttered interface, clear primary actions, consistent object pages, search/filterable lists, inline validation, plain-language labels and task checklists. Make gateway, DID, extension, routing and IVR/queue setup easy for a new admin. Advanced Mode must retain every supported option. Show the difference between saved, applied, registered and call-tested states. Require confirmation and audit for disruptive changes. The goal is ease comparable to 3CX with a broader modular capability set, not a visual or code clone. Keep FreeSWITCH and current app behavior intact.
