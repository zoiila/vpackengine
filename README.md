# VPackEngine | Vack Studio

Copyright (c) Vack Studio. All rights reserved.

VPackEngine manages existing resource packs and exposes a public API for Paper plugins that build resource-driven inventory menus.

## Verified public build

**1.0.0-preview.1** · Paper 26.2 · Java 25

- [VPackEngine JAR](downloads/VPackEngine-1.0.0-preview.1.jar)
- [Install and first-use guide](docs/INSTALL.md)
- [Private source repository](https://github.com/zoiila/vpackengine-private)

SHA-256: `24ad8aec7dd804c4e4adcaed16547891496e41915371185ffe48f85e64e7f4e2`

This public repo shares plugin information and the verified JAR. Development source remains private.

## What it provides

- Immutable pack versions, staged validation, activation and rollback.
- Persistent pack and profile records, dependencies and stack ordering.
- Local ZIP imports and allowlisted HTTPS downloads with bounded size, CRC and decompression checks.
- Optional confined GET/HEAD hosting, immutable URLs, ETags and single byte ranges.
- Per-session player requests, retries, cooldowns, timeouts and status events.
- Native admin menus, resource bindings, readiness gating, fallbacks and a public Java API.

VPackEngine manages packs that already exist; it does not create textures or reconcile conflicting pack contents. Resource packs can change assets such as textures, models, sounds and fonts. A pack alone cannot add gameplay logic or freely redesign vanilla screens. New gameplay behavior requires server-side implementation; client-side behavior outside resource-pack capabilities requires a client mod.

## Version status

The private repository's publication record says a newer **1.1.0-preview.2** source tree must be recovered before runtime changes or a release can be published. That source/build is not available in the current working files, so this public repository identifies the verified **1.0.0-preview.1** build explicitly and does not claim it is the newest development version.

See the private repository's validation notes for execution details. Production GUI artwork and real-client rendering are not claimed for this preview.
