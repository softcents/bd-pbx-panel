# SoftCents BD PBX Panel

Issabel-inspired UI/UX redesign for the existing FS PBX / FreeSWITCH platform. The goal is to use Issabel PBX's familiar navigation and information architecture while preserving the existing FS PBX implementation and all working features.

## Product direction

- Follow the familiar Issabel-style PBX navigation and settings organization.
- Keep the interface clean, white, readable, and straightforward.
- Adapt the menu to real FS PBX capabilities; do not copy branding or assume Asterisk-only behavior.
- Preserve every existing feature, API, database behavior, permission, integration, and FreeSWITCH call flow.
- Provide role-specific workspaces and Easy/Advanced Mode without removing advanced capabilities.
- Implement advanced contact-center features only when supported by the actual code and integrations.

## Non-negotiable rule

**Redesign and simplify the interface without removing existing capabilities or breaking working behavior.** The existing FS PBX application remains the source of truth for backend logic, FreeSWITCH integration, database schema, routes, APIs, permissions, and telephony behavior.

## Required workflow

1. Connect or import the actual FS PBX source code and audit the application.
2. Document existing features, routes, APIs, permissions, database behavior, and FreeSWITCH integrations.
3. Map every existing feature to an Issabel-inspired menu/page before modifying code.
4. Implement incrementally without replacing the PBX or inventing mock behavior.
5. Run automated and telephony regression tests and report exact results.

## Current status

This repository currently contains project specifications and implementation instructions, not the complete FS PBX application source. No production UI or telephony code has been imported or changed yet.

## Source references

- Existing FS PBX repository: https://github.com/nemerald-voip/fspbx
- Existing FS PBX design guide: https://github.com/nemerald-voip/fspbx/blob/main/DESIGN.md

Issabel is the UX and navigation reference; FreeSWITCH remains the telephony engine.