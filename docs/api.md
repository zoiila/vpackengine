# Java API

Add VPackEngine as a server dependency and `compileOnly` its JAR. Do not shade it or Paper. Declare `depend: [VPackEngine]` or an optional dependency if your plugin has its own vanilla behavior.

```java
VPackApi api = Bukkit.getServicesManager().load(VPackApi.class);
if (api == null) { /* service still initializing or absent; use your fallback */ return; }
api.pack("ui").ifPresent(pack -> getLogger().info(pack.activeVersion()));
api.profile("minigames").ifPresent(profile -> getLogger().info(profile.entries().toString()));
api.apply(player, "minigames").thenAccept(result -> {
    if (result.applied() && api.ready(player, "resource-demo")) {
        api.open(player, "resource-demo");
    } else {
        api.open(player, "demo");
    }
});
```

**Threading:** resolving immutable pack/profile snapshots may be done from any thread after initialization. `apply`, `status`, `ready`, `open` and all Bukkit events require the Paper main thread. Application futures complete on the main thread, including disconnect/supersede/disable results; attaching a callback after completion executes on the attaching thread, as specified by CompletableFuture. Always schedule to the main thread when attaching from an asynchronous caller. `thenAcceptAsync` requires explicitly scheduling any Bukkit operation back to main. No Folia scheduler implementation is advertised.

```java
@EventHandler
public void onPackState(PackStateEvent event) {
    PackModels.Status status = event.status(); // immutable current event value
    // REQUESTED, ACCEPTED, DOWNLOADED if reported, APPLIED, DECLINED,
    // FAILED, TIMED_OUT, REMOVED, DISCONNECTED or SUPERSEDED.
}
@EventHandler
public void onMenuAction(MenuActionEvent event) {
    if (!event.menu().equals("resource-demo")) return;
    if (event.action().equals("minigame.join")) {
        if (!event.player().hasPermission("yourgame.join")) {
            event.setCancelled(true); return;
        }
        // Call your own application service here. Do not alter VPack inventory items.
    }
}
```

The public records expose logical/version identity, stable version application UUID, request-specific wire UUID, generation, attempt, timestamps and client-reported state. Treat client messages as UX state, not authorization/security attestation. API callers must enforce their own domain permissions. No internal manager implementation is part of the API contract.
