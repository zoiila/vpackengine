# VPackEngine

**Vack Studio plugin profile**

VPackEngine manages existing resource packs for Paper servers and provides a public API for add-ons that build resource-driven inventory menus.

## Capabilities

- Import, validate, version, activate and roll back resource packs.
- Organize pack profiles and dependencies.
- Deliver approved packs to players and expose request status events.
- Provide native administrator menus, resource bindings and readiness checks.

VPackEngine manages pack files; it does not create gameplay behavior or reconcile conflicting resources. Resource packs change client-supported assets. New gameplay behavior requires a server-side implementation, while client behavior beyond pack capabilities requires a client mod.

- **Platform:** Paper 26.2 · Java 25
- **Source:** maintained in the private VPackEngine repository

Copyright (c) Vack Studio. All rights reserved.
