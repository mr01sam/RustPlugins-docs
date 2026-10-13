# WarMode

Flag players as PVP or PVE. Controls what players can attack, loot, target, and mount based on their current mode. Ships with PVP, PVE, and NPC modes out of the box; fully custom modes can be added via the config or the Admin Panel extension.

## Permissions

| Permission | Effect |
|---|---|
| `warmode.admin` | Access to all admin commands (`warmode.setplayer`, `warmode.setall`, etc.). |

Modes are backed by Oxide permission **groups**, not individual permissions. The group name for each mode is set in the config under `Group` (e.g. `warmodepvp`, `warmodepve`). Players in a mode's group will be assigned that mode on next login/mode resolution. When a player qualifies for multiple mode groups, the one with the lowest `Priority` value wins.

## Commands

### Player commands

| Command | Default | Description |
|---|---|---|
| `/<flag command>` | `/flag` | Toggle between available modes (cycles through `Mode Options` in config), or specify a mode name directly, e.g. `/flag pvp`. Configurable via `Chat Command` in the Flagging section. |

### Admin commands (require `warmode.admin`)

| Command | Arguments | Description |
|---|---|---|
| `/warmode` | — | Prints a list of all available admin commands. |
| `/warmode.setplayer` | `<player> <mode>` | Sets the mode for the specified player by name or Steam ID. |
| `/warmode.setall` | `<mode>` | Sets the mode for every player currently on the server. |
| `/warmode.mode` | `[player]` | Displays the current mode for the given player, or your own if omitted. |
| `/warmode.tc` | — | Displays the mode of the base (Tool Cupboard) you are currently standing in. |
| `/warmode.getmode` | — | Displays the mode of the entity or player you are looking at. |
| `/warmode.debugmode` | `[id] [mode\|clear]` | Assign a fake mode to any player ID for testing. Omit all args to clear all debug overrides. |
| `/wmc` or `/warmode.config` | — | Opens the WarMode Admin Panel UI *(requires WarMode Admin Panel extension)*. |

## Configuration

Plugin integration sections (ZoneManager, RaidableBases, BetterChat, SimpleStatus, etc.) are **hidden by default** and only appear in the config once the relevant plugin is loaded. After installing a new integration plugin, reload WarMode and the new section will be written to your config automatically.

---

### Settings

| Key | Default | Description |
|---|---|---|
| `Initial Player Mode` | `pvp` | Mode assigned to brand-new players on their first spawn. |
| `Npc Mode` | `npc` | Mode assigned to NPC players spawned by other plugins. Vanilla scientists are not affected. |
| `Allow Vehicle Mode Ownership` | `false` | When enabled, vehicles and horses owned by a player (via VehicleLicense or similar) are treated as `claimedvehicles` / `claimedhorses` rather than generic `vehicles` / `horses`, giving them separate rule entries. |
| `Always Allow Twig Damage` | `true` | When true, twig building blocks can always be damaged regardless of mode rules. |
| `Always Allow Twig Placement` | `false` | When true, twig placement is allowed inside any base regardless of mode rules. When false, players cannot place twig blocks in bases they could not otherwise attack. |
| `Can Kill Trespassing Sleepers` | `false` | When true, players who log out inside another player's base can be killed regardless of their mode. |
| `Show Debug Messages In Console` | `false` | Prints rule decision messages to the server console. Extremely spammy — only enable when debugging. |
| `Allow Plugin Override` | `true` | When true, other plugins (Sputnik, Convoy, etc.) can override WarMode rules inside their own zones. When false, WarMode retains its own rules even inside third-party zones. |

---

### Flagging

Controls the `/flag` command players use to switch their own mode.

| Key | Default | Description |
|---|---|---|
| `Chat Command` | `flag` | The chat command players type to change their mode. |
| `Mode Options` | `["pve", "pvp"]` | Which modes are available for players to cycle through with the flag command. |
| `Requires Safe Zone` | `true` | Players must be inside a safe zone to use the flag command. If team sync is on, all active team members must be in a safe zone. |
| `Requires No Hostile` | `true` | Players must not be marked as hostile. If team sync is on, all active team members must be non-hostile. |
| `Requires Team Leader` | `false` | Only the team leader can use the flag command. Solo players are not affected. |
| `Cooldown Seconds` | `60` | Seconds before the flag command can be used again. Set to `0` to disable the cooldown. |

---

### Teams

| Key | Default | Description |
|---|---|---|
| `Sync Mode With Team Members` | `true` | When a player's mode changes, all active team members are synced to the mode with the lowest priority among the team. |

---

### Notifications

| Key | Default | Description |
|---|---|---|
| `Chat` | `true` | Send mode change notifications via chat. |
| `Toastify` | `false` | Send mode change notifications via Toastify *(requires Toastify plugin)*. |

---

### Chat

| Key | Default | Description |
|---|---|---|
| `Show Plugin Prefix` | `true` | Prepends `[WarMode]` to plugin chat messages. |
| `Plugin Prefix Color` | `#aaee32` | Hex color for the `[WarMode]` prefix. |
| `Steam Id For Icon` | `0` | Steam ID whose avatar is used as the chat icon for WarMode messages. |
| `Sfx Fail` | *(keypad denied)* | Sound effect played for negative notifications. |
| `Sfx Success` | *(keypad updated)* | Sound effect played for positive notifications. |

---

### Marker

The crosshair marker that appears when a player aims their weapon at another player who has `Show Marker When Aimed At` enabled for their mode.

| Key | Default | Description |
|---|---|---|
| `Show` | `true` | Master toggle for all mode markers. |
| `Distance` | `100` | Maximum distance (meters) at which the marker is visible. |
| `Update Interval` | `0.5` | How often (seconds) the marker refreshes. Lower values are more responsive but may impact performance. |
| `Image` | `assets/icons/peace.png` | Sprite asset path for the marker icon. |
| `Size` | `16` | Marker icon size in pixels. |
| `Color` | `0 0 1 0 0.5` | RGBA color of the marker. |

---

### Modes

An array of mode objects. The three default modes are `pvp`, `pve`, and `npc`.

| Key | Default | Description |
|---|---|---|
| `Name` | *(required)* | Unique internal identifier for the mode (lowercase, no spaces). Used in all config rule arrays, commands, and API calls. |
| `Display Name` | *(mode name uppercased)* | Name shown to players in chat messages. If blank, falls back to the localization key `mode <name>`. |
| `Color Hex` | *(varies)* | Hex color used when the mode name is referenced in chat messages from WarMode itself. |
| `Group` | `null` | Oxide permission group that places players into this mode. Required for player-usable modes; leave `null` for NPC-only modes. |
| `Priority` | `null` | Lower number = higher precedence. When a player is in multiple mode groups (or team sync is active), the mode with the lowest priority wins. Required when `Group` is set. |
| `Show Marker When Aimed At` | `true` | Whether the crosshair marker is shown when another player aims at someone in this mode. |
| `Always Allow Fire Damage` | `false` | If true, fire/incendiary damage always applies to this player and their property, regardless of what mode caused it. |
| `Can Take Patrol Heli Damage` | `true` | If false, patrol helicopters cannot damage players or bases of this mode. |
| `Deep Sea Mode` | `""` | Mode players automatically switch to when entering the deep sea. Leave blank to keep their current mode. |

#### Rule arrays

Each mode has five rule dictionaries — **Can Attack Types**, **Can Loot Types**, **Can Target Types**, **Can Mount Types**, and **Can Enter Types**. Each dictionary maps an entity category to an array of mode names that this mode is allowed to interact with.

**Example:** `"Can Attack Types": { "players": ["pvp", "npc"] }` means a player in this mode can attack players who are in `pvp` or `npc` mode.

**Attacking** entity categories:

| Category | Entities |
|---|---|
| `players` | Other players |
| `buildings` | Building blocks, doors, deployables, IO entities |
| `traps` | Bear traps, barricades, gun traps |
| `containers` | Storage boxes and similar containers |
| `turrets` | Auto turrets, flame turrets |
| `samsites` | SAM sites |
| `horses` | Unclaimed ridable horses |
| `vehicles` | Unclaimed vehicles (cars, helicopters, etc.) |
| `tugboats` | Tugboats and anything attached to them |
| `legacyshelters` | Legacy shelter entities |
| `claimedvehicles` | Player-owned vehicles *(requires Allow Vehicle Mode Ownership)* |
| `claimedhorses` | Player-owned horses *(requires Allow Vehicle Mode Ownership)* |
| `stashes` | Hidden stashes |
| `sleepingbags` | Sleeping bags and beds |
| `miningquarries` | Mining quarries and pump jacks |
| `modularboats` | Player-built modular boats |

**Looting** entity categories: same as Attacking plus `growables`, `droppedbackpacks`, `publiccontainers`, `droppeditems`.

**Targeting** entity categories (traps, turrets, samsites auto-targeting): `traps`, `turrets`, `samsites`.

**Mounting** entity categories: `vehicles`, `horses`, `tugboats`, `claimedvehicles`, `claimedhorses`, `modularboats`.

**Entering** entity categories: `claimedmonuments`.

---

### ZoneManager *(appears when ZoneManager is installed)*

| Key | Default | Description |
|---|---|---|
| `Force Mode In Zone` | *(example entries)* | Dictionary mapping zone selectors to mode names. Key format: `<property><operator><value>` where `property` is `id` or `name`, `operator` is `=` (exact) or `~` (partial). Example: `"id=abc123"` → `"pvp"` or `"name~arena"` → `"pvp"`. |
| `Zone Exit Delay` | `3` | Seconds to wait after a player leaves a forced zone before reverting their mode. Prevents rapid flickering near zone borders. Re-entering the zone during the delay cancels the revert. Set to `0` for immediate revert. |

---

### Raidable Bases *(appears when Raidable Bases is installed)*

| Key | Default | Description |
|---|---|---|
| `Protection Radius` | `50` | Must match the Protection Radius set in the Raidable Bases config. |
| `Can Enter Pvp Raidable Bases` | `["pvp"]` | Modes allowed to enter PVP raidable bases. Players in other modes are ejected. |
| `Can Enter Pve Raidable Bases` | `["pve", "pvp"]` | Modes allowed to enter PVE raidable bases. |
| `Forced Mode Pvp Zone` → `Forced Mode` | `""` | Mode players appear to be in while inside a PVP raidable base. Leave blank to not force a mode. |
| `Forced Mode Pve Zone` → `Forced Mode` | `""` | Mode players appear to be in while inside a PVE raidable base. Leave blank to not force a mode. |

---

### BetterChat *(appears when BetterChat is installed)*

| Key | Default | Description |
|---|---|---|
| `Show Mode Prefix In Chat` | `true` | Show the player's current mode as a title prefix in BetterChat. |
| `Modes` → `<mode>` → `Show Mode Title In Chat` | `true` | Whether this mode's title appears in chat for players who hold it. |
| `Modes` → `<mode>` → `Mode Title Format` | `[{0}]` | Format string for the mode title. `{0}` is replaced with the mode's display name. |

---

### SimpleStatus *(appears when SimpleStatus is installed)*

| Key | Default | Description |
|---|---|---|
| `Mode Status Bars` → `<mode>` → `Show` | `true` | Whether the status bar for this mode is displayed. |
| `Mode Status Bars` → `<mode>` → `Background Color` | *(varies)* | RGBA background color of the status bar. |
| `Mode Status Bars` → `<mode>` → `Image` | *(varies)* | Sprite asset path for the status bar icon. Only sprites are currently supported. |
| `Mode Status Bars` → `<mode>` → `Image Color` | *(varies)* | RGBA tint color applied to the icon. |
| `Mode Status Bars` → `<mode>` → `Title Color` | *(varies)* | RGBA color of the mode title text in the status bar. |
| `Mode Status Bars` → `<mode>` → `Text Color` | *(varies)* | RGBA color of the subtitle text in the status bar. |

---

### NoEscape *(appears when NoEscape is installed)*

| Key | Default | Description |
|---|---|---|
| `Prevent Flagging While Combat Blocked` | `true` | Players cannot use the flag command while combat blocked. |
| `Prevent Flagging While Raid Blocked` | `true` | Players cannot use the flag command while raid blocked. |
| `Prevent Flagging While Escape Blocked` | `true` | Players cannot use the flag command while escape blocked. |

---

### Toastify *(appears when Toastify is installed)*

| Key | Default | Description |
|---|---|---|
| `Positive Toast ID` | `success` | Toast type ID for positive mode notifications. Must match a toast defined in your Toastify config. |
| `Negative Toast ID` | `error` | Toast type ID for negative/blocked notifications. |
| `Duration` | `4` | How long (seconds) toasts remain visible. |

---

### Event plugin forced modes *(each section appears when the respective plugin is installed)*

Each supported event plugin has its own config section containing a single key:

| Key | Default | Description |
|---|---|---|
| `Forced Mode` | `pvp` | Mode players appear to be in while inside that plugin's event zone. Leave blank (`""`) to not force a mode. |

Supported plugins:

| Config section | Plugin | Author |
|---|---|---|
| `Sputnik` | Sputnik | Adem |
| `Convoy Reforged` | Convoy Reforged | Adem |
| `Caravan` | Caravan | Adem |
| `Shipwreck` | Shipwreck | Adem |
| `Ziggurat Event` | Ziggurat Event | Simil3x |
| `Lighthouse Event` | Lighthouse Event | Simil3x |
| `Operation Fortress` | Operation Fortress | Mevent |
| `Dangerous Treasures` | Dangerous Treasures | nivex |
| `Gas Station Event` | Gas Station Event | KpucTaJl |
| `Triangulation` | Triangulation | KpucTaJl |
| `Supermarket Event` | Supermarket Event | KpucTaJl |
| `Water Event` | Water Event | KpucTaJl |
| `Arctic Base Event` | Arctic Base Event | KpucTaJl |
| `Air Event` | Air Event | KpucTaJl |
| `Ferry Terminal Event` | Ferry Terminal Event | KpucTaJl |

---

## Developer API

### Hooks (callable by other plugins)

```csharp
// Called whenever a player's effective mode changes, or when config is reloaded.
// userid: the player's Steam ID string
// modeId: the mode's internal Name (e.g. "pvp", "pve")
private void WarMode_PlayerModeUpdated(string userid, string modeId) { }

// Return false to prevent a player from using the /flag command.
// Return null or true to allow it.
private object WarMode_CanUseFlagCommand(string userid) { }

// Called when the flag command is about to execute.
// Return a non-null value to cancel WarMode's default mode-switching logic
// (e.g. if your plugin is handling it).
private object WarMode_OnFlagCommand(string userid, string requestedMode) { }
```

### Methods (callable on the WarMode plugin reference)

```csharp
// Returns the internal mode name of an entity, taking forced zones into account.
string GetEntityMode(BaseEntity entity);

// Returns the Oxide group name for a given mode id (e.g. "pve" → "warmodepve").
string GetModeGroup(string modeId);

// Returns the target-type category name for an entity (e.g. "players", "buildings").
string GetEntityTargetType(BaseEntity entity);
```

---

## Extension plugins

| Plugin | Description |
|---|---|
| **WarMode Admin Panel** | In-game UI for editing modes, rules, settings, zones, players, and event plugin forced modes. Opens with `/wmc`. |
| **WarMode Badges** | Displays a configurable HUD badge showing each player's current mode. |
| **WarMode Rules UI** | In-game rules board that shows players what the PVP/PVE rules are on the server. |
| **WarMode Spawn UI** | Selection screen shown to new players so they can choose their starting mode. |
