# Release acceptance checklist

Automated tests cover ZIP metadata/CRC/traversal/limits, hashes, atomic registry failure/restart, dependency order, staging/activation/rollback, immutable version rejection, quarantine, request transitions, cached direct success, terminal states, stale retry/supersede/disconnect identities, unrelated pack identity isolation, HTTP allowlist/method/range behavior and unsafe remote destinations.

Server smoke checks must cover descriptor load, asynchronous initialization, command dispatch, doctor, import/activation/profile persistence, invalid reload preserving working settings and shutdown. A startup check is not a multiplayer test.

A real client must still check native/additional packs, cached application, decline/failure, reconnect, superseded callbacks, ordering/conflicts with other pack plugins, dependent custom-menu fallback, permissions, shift-click/drag/number-key/double-click/drop paths, model/glyph positioning, and native/custom audio. Leaf and Folia require separate scheduler and runtime acceptance. Generated concept artwork does not prove visual functionality.
