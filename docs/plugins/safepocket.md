# Safe Pocket

Gives players a small, death-safe storage pocket. Items kept in the Safe Pocket are not lost when the player dies.

## Permissions

| Permission | Effect |
|---|---|
| `safepocket.use` | Required to open the Safe Pocket button/UI. |
| `safepocket.1slot` | Grants 1 pocket slot. |
| `safepocket.2slot` | Grants 2 pocket slots. |
| `safepocket.3slot` | Grants 3 pocket slots. |
| `safepocket.3slot.all` | Grants 3 slots with every item category allowed (overrides the default resource/component/misc-only restriction). |

Permission tiers are resolved by **Importance** (highest wins) — a player with both `safepocket.3slot` and `safepocket.3slot.all` gets the `all` tier's settings since it has higher importance.

## Configuration

### Top level

| Key | Default | Description |
|---|---|---|
| `Enabled` | `true` | Master on/off switch. |

### `DefaultSettings` / `Overrides[]`

Each entry (default + per-permission overrides) supports:

| Key | Default | Description |
|---|---|---|
| `Permission` | *(none for default)* | Permission required for this tier. |
| `CreatePermission` | `true` | Whether the plugin should auto-register this permission. |
| `Slots` | `1` (default tier: `0`) | Number of pocket slots granted. `0` disables the Safe Pocket entirely for that tier. |
| `OpenDelay` | `3` | Seconds the pocket takes to finish opening. `0` opens instantly. |
| `Importance` | `0` | Resolution priority — higher wins when a player qualifies for multiple tiers. |
| `AllowedItemCategories` | Resources, Component, Misc | Item categories that can be placed in the pocket. |
| `ItemBlacklist` | *(empty)* | Item shortnames always rejected, regardless of category. |

### `UI`

| Key | Default | Description |
|---|---|---|
| `SlotsBeforeWrap` | `4` | How many slots are shown before the preview wraps to an overflow `+N` indicator. |
| `OpenEffect` | beartrap rustle fx | Effect played when opening starts. Empty string disables it. |
| `OpenFinishEffect` | cloth impact fx | Effect played when opening finishes. Empty string disables it. |
| `ShowProgressBar` | `true` | Whether the animated fill bar shows during the opening delay. |
| `ShowItemToasts` | `true` | Whether moving items in/out of the pocket shows the vanilla pickup/loss toast. |
| `ButtonAnchorX` / `ButtonAnchorY` | `0.5` / `0` | Anchor point (0–1) for the Safe Pocket button. |
| `ButtonOffsetLeft` / `ButtonOffsetUp` | `532` / `170` | Pixel offset from the anchor point. |
| `ShowTitle` | `true` | Whether "SAFE POCKET" text shows above the button. |
| `ShowTooltip` | `true` | Whether hovering the button shows the explanatory tooltip. |

## Behavior notes

- The pocket is backed by a hidden, non-solid `StorageContainer` spawned at `Vector3.zero` with networking limited to the owning player — it never appears in the world.
- Item category/blacklist restrictions are enforced live via `CanAcceptItem`; a rejected item shows an on-screen warning message.
- The pocket button and item preview redraw automatically when: items move in/out while open, the player's permissions or group membership change, or on wake/respawn.
- Saved contents persist in the plugin's data file (`oxide/data/SafePocket.json` on Oxide, equivalent on Carbon) and are restored the next time the pocket opens.
- While the item icon/amount preview survives most container operations, the preview re-sync additionally fires on `OnItemAddedToContainer`/`OnItemRemovedFromContainer` — toasts are suppressed during the initial restore of saved items so opening the pocket doesn't look like a fresh pickup.

## Commands

| Command | Description |
|---|---|
| `safepocket.open` (console command, bound to the UI button) | Opens or closes the caller's Safe Pocket. Requires `safepocket.use`. |
