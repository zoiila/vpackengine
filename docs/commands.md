# Commands and permissions

Every administrative operation needs `vpack.admin` plus the specific permission below. Console may use operations with explicit online player names. `menu` and own `status` need a player. `vpack.use` defaults to everyone; administrator permissions default to operators. Completion hides administrator operations from non-admins.

| Syntax following `/vpack` | Extra permission | Behavior |
|---|---|---|
| `help` | `vpack.use` | Show permission-relevant usage |
| `menu [admin\|menu-id]` | `vpack.use`; admin menu also `vpack.admin` | Open management or registered menu |
| `list` | None | Pack status/active version list |
| `info <pack>` | None | Metadata and all version hashes/validation |
| `import <pack> <version> <file.zip\|https-url> [delivery-url\|-] [expected-sha1]` | `vpack.import` | Bounded validate/stage; never auto-activate |
| `validate <pack> [version]` | `vpack.import` | Recheck local integrity and external hosted bytes |
| `activate <pack> <version>` | `vpack.activate` | Validate artifact, atomically activate |
| `rollback <pack> <validated-version>` | `vpack.activate` | Explicitly select any previous validated version |
| `enable <pack> <true\|false>` | `vpack.activate` | Update enabled state and invalidate readiness |
| `describe <pack> <display-name> <description>` | `vpack.activate` | Underscores represent spaces |
| `depends <pack> <dependency-csv\|-> <after-csv\|->` | `vpack.profile` | Set prerequisite and relative ordering rules |
| `delete <pack> confirm` | `vpack.remove` | Reject referenced entries; retain immutable artifact files |
| `profile list` / `profile show <id>` | `vpack.profile` | Inspect persisted profiles |
| `profile create <id>` | `vpack.profile` | Create empty ordered profile |
| `profile add <id> <pack> <required\|optional> [version\|active]` | `vpack.profile` | Append dependency-valid pack |
| `profile remove <id> <pack>` | `vpack.profile` | Remove reference if dependencies remain valid |
| `profile move <id> <pack> <zero-based-index>` | `vpack.profile` | Reorder; reject dependency violations |
| `profile policy <id> <pack> <required\|optional>` | `vpack.profile` | Change policy |
| `profile assign <id> <on-join:true\|false> <world-csv\|-> <permission\|->` | `vpack.profile` | First matching profile in persisted order wins |
| `apply <player\|*> <profile> [confirm]` | `vpack.apply` | Replace this plugin's profile requests; `*` requires `confirm` |
| `remove <player\|*> [confirm]` | `vpack.remove` | Remove only plugin-owned packs; `*` requires `confirm` |
| `status [player]` | `vpack.use`; other player also `vpack.admin` | State, version, attempt, reason and elapsed seconds |
| `reload` | `vpack.reload` | Fully validate settings before installing |
| `doctor` | None | Registry/artifact diagnostics and actual environment |

Pack, profile and version IDs match `[a-z0-9][a-z0-9_-]{0,47}`. Imported filenames are simple `.zip` names inside `imports/`, with no paths or symlinks. URL arguments contain no spaces. Import/validation/hashing is asynchronous on two workers with an eight-job queue. Rejection, unreachable URLs, invalid hashes and queue saturation produce actionable errors.

The GUI confirms destructive and selected-player profile actions. Command `activate` is an explicit direct action; the GUI confirms it. Player selection is checked again at execution. Profile edits use optimistic snapshot checks so concurrent edits fail with a refresh instruction instead of overwriting another edit.
