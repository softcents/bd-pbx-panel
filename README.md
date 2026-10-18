# SoftCents BD PBX Panel

Issabel-inspired UI/UX redesign project for the existing FS PBX / FreeSWITCH platform.

## Non-negotiable rule

**Redesign and simplify the interface without removing existing features or breaking working behavior.** The existing FS PBX application remains the source of truth for backend logic, FreeSWITCH integration, database schema, routes, APIs, permissions, and telephony behavior.

## Goals

- Organize navigation and settings using Issabel PBX as an information-architecture reference, without copying its branding or forcing Asterisk-only behavior into FreeSWITCH.
- Keep a clean white operational UI, compact tables, readable forms, responsive side navigation, and consistent status indicators.
- Provide Easy Mode and Advanced Mode. Easy Mode may simplify presentation but must not remove advanced capabilities.
- Build role-specific views for Super Admin, Company Admin, Reseller, Customer Admin, Supervisor, Agent, Finance/Billing, and NOC/Operations.
- Support a full advanced contact-center experience only where the underlying platform and integrations genuinely support the capability.
- Document feature parity and verify regressions before release.

## Required workflow

1. Import or connect the existing FS PBX source code and inspect its actual framework, routes, APIs, permissions, database, and FreeSWITCH integration.
2. Produce a feature inventory and old-feature-to-new-navigation mapping before changing application code.
3. Propose the UI architecture and implementation plan.
4. Implement in small, reviewable changes; preserve existing routes and contracts unless a migration is explicitly designed and tested.
5. Run automated tests, build checks, and telephony regression checks. Never mark a feature complete based only on a mock screen.

## Current status

This repository was newly created and is currently a project specification/scaffold, not the FS PBX application source. No production code has been imported or changed yet.

## Design reference

- Existing FS PBX repository: https://github.com/nemerald-voip/fspbx
- Existing FS PBX design guide: https://github.com/nemerald-voip/fspbx/blob/main/DESIGN.md
- Issabel is a UX/navigation reference only; retain FreeSWITCH-compatible implementation.
