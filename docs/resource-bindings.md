# Resource binding contract

Native inventories provide slot-based clicks. Resource packs supply item models, fonts/glyphs and audio; they do not add arbitrary interactive pixel regions or widgets. No finished production pack artwork is included. The default demonstration and admin GUI use vanilla icons/fonts and can be inspected without a pack.

Configure CHEST (9–54 slots, multiples of 9), HOPPER (5), DISPENSER or DROPPER (9), title MiniMessage, a permission, cooldown, buttons and either explicit slots or a one-character layout. `pages` defaults to 1; each button's `page` defaults to -1 (all pages), or selects a zero-based page. NEXT/PREVIOUS stop at the configured boundaries. `<player>`, `<page>`, `<menu>` and `<ready>` placeholders are inserted unparsed.

Each button supports material, name, lore, permission, `condition: always|ready|not-ready`, disabled/selected states, `item-model: namespace:path`, and `model-floats: [1001.0]` through the verified ItemMeta component API. Assets must contain the corresponding Minecraft 26.2 item model and optional custom model data binding.

Actions are typed: OPEN (menu ID), BACK, CLOSE, NEXT, PREVIOUS, MESSAGE (plain configured message payload), APPLY_PROFILE (requires `vpack.apply`), API (emits MenuActionEvent). Arbitrary console commands are not a GUI action type. Other Vack plugins implement API actions and their own permissions/game logic.

Example after importing/activating a pack named `ui` with version `v1`:

```yaml
bindings:
  panel:
    font: vack:ui
    glyph: '\uE001' # In actual YAML use a double-quoted unicode escape or literal glyph.
    offset: '' # User-supplied spacing glyphs only; no assumed font offsets.
menus:
  custom:
    type: CHEST
    size: 27
    title: '<white>Menu'
    title-binding: panel
    permission: vpack.use
    cooldown-ms: 300
    required-packs: {ui: v1}
    fallback: demo
    buttons:
      play:
        slots: [13]
        material: PAPER
        item-model: vack:play
        model-floats: [1001.0]
        name: '<white>Play'
        action: API
        value: minigame.join
```

All specified pack/version requirements (and all packs in `required-profile`) must be reported APPLIED in the current session, including optional profile entries when used as menu dependencies. Active-version requirements follow activation changes; pinned requirements are stable. When pack readiness is lost, `application.unavailable-menu-policy: FALLBACK|CLOSE` controls rebuilding/closure. Reload closes stale inventories. Use `fallback: demo` for a visible vanilla alternative.

The holder/session identity cancels every click/drag path before interpreting actions. Only ordinary top-inventory left/right clicks execute. Shift-click, number keys, double-click, drops, outside slots, stale inventories and repeated activations are blocked. No menu items may enter player inventory.

Resource binding JSON, actual item models, glyph positioning, custom audio and client rendering require client verification with the supplied pack. The plugin's compilation and service tests do not prove those visuals.
