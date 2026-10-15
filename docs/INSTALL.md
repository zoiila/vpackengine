# Install VPackEngine 1.0.0-preview.1

Copyright (c) Vack Studio. All rights reserved.

## Requirements

- Paper 26.2
- Java 25

## Install and inspect

1. Place `VPackEngine-1.0.0-preview.1.jar` in the server's `plugins/` folder.
2. Start Paper and wait for **VPackEngine ready**.
3. Configure trusted CDN hostnames under `remote.allowed-hosts` before importing remote URLs.
4. Check the server with `/vpack doctor`. Open `/vpack menu admin` or `/vpack menu demo` for the native inventory interfaces.

## Import and apply an existing pack

Copy a pack ZIP into `plugins/VPackEngine/imports/`. Use an immutable HTTPS URL for player delivery, then run:

```text
/vpack import lobby v1 lobby.zip https://cdn.example.com/lobby-v1.zip
/vpack activate lobby v1
/vpack profile add global lobby optional
/vpack apply PlayerName global
```

Use the commands and profile options listed by `/vpack help` for your server's configuration. Back up the plugin registry and pack files together. Do not edit the registry while the plugin is running.

The exact downloaded bytes are compared with the imported archive. Give each changed pack version a new immutable URL; reusing a URL for different bytes can conflict with client caching and validation.

## Limits

VPackEngine manages and delivers existing resource packs. It does not create textures or merge conflicting assets. Resource packs can provide visual/audio assets and custom fonts/models supported by the client. They do not add gameplay logic, create server-side item behavior, or freely restyle vanilla screens; those need server code or a client mod.

The 1.0.0-preview.1 JAR has server-side validation. A real Minecraft client's resource-pack application/rendering was not verified for this publication.
