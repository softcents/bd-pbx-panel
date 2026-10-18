# SoftCents BD PBX Panel

A custom, task-focused operations interface for the existing FS PBX / FreeSWITCH platform. The goal is to make everyday PBX and advanced contact-center work easier without forcing users through a complicated legacy-style administration UI.

## Product direction

Build a purpose-designed SoftCents experience—not a clone of another PBX interface. Use a clean white workspace, clear task-based navigation, smart defaults, useful search, focused forms, and role-specific workspaces.

## Non-negotiable rule

**Improve usability without removing existing capabilities or breaking working behavior.** The existing FS PBX application remains the source of truth for backend logic, FreeSWITCH integration, database schema, routes, APIs, permissions, and telephony behavior.

## Core experience

- Simple, white, responsive operational UI.
- Task-based navigation and global search/quick actions.
- Easy Mode for common tasks and Advanced Mode for detailed controls.
- Role-specific workspaces for Super Admin, Company Admin, Reseller, Customer Admin, Supervisor, Agent, Finance/Billing, and NOC/Operations.
- Advanced contact-center workflows only when real platform capabilities and integrations support them.
- Clear health, status, validation, empty, loading, error, and audit states.
- Full feature inventory and regression testing before release.

## Required workflow

1. Connect or import the actual FS PBX source code and audit the application.
2. Document existing features, routes, APIs, permissions, database behavior, and FreeSWITCH integrations.
3. Design the custom UX and map every existing feature to a destination.
4. Implement incrementally without replacing the PBX or inventing mock behavior.
5. Run automated and telephony regression tests and report exact results.

## Current status

This repository currently contains project specifications and implementation instructions, not the complete FS PBX application source. No production UI or telephony code has been imported or changed yet.

## Source reference

- Existing FS PBX repository: https://github.com/nemerald-voip/fspbx
- Existing FS PBX design guide: https://github.com/nemerald-voip/fspbx/blob/main/DESIGN.md

The reference repository is the source to audit. The new panel should use a custom SoftCents design language rather than copying another product's UI or branding.