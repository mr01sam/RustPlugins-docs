# Safe Pocket

Gives players a small, death-safe storage pocket. Items kept in the Safe Pocket are not lost when the player dies.

## Permissions

Permissions are **not fixed** — they're dynamically created from the `Permission Tier Overrides` section of the config. Each tier you define there gets its own permission (if `Auto-Register Permission` is enabled for it), so the list below is just what ships by default, not the full set of permissions the plugin can have.

Out of the box:

| Permission | Effect |
|---|---|
| `safepocket.1slot` | Grants 1 pocket slot. |
| `safepocket.2slot` | Grants 2 pocket slots. |
| `safepocket.3slot` | Grants 3 pocket slots. |
| `safepocket.3slot.all` | Grants 3 slots with every item category allowed (overrides the default resource/component/misc-only restriction). |

Add, remove, or rename tiers in the config to change this list — grant a player any tier's permission and the plugin will register it and apply that tier's settings automatically. When a player qualifies for more than one tier, the one with the highest `Priority` wins.

## Configuration

### Default Settings

Baseline settings used for any player who doesn't hold a permission from `Permission Tier Overrides` below.

| Key | Default | Description |
|---|---|---|
| `Permission Required` | *(none)* | Not set on the default tier — it's the fallback everyone gets. |
| `Auto-Register Permission` | `true` | Whether the plugin should auto-register this tier's permission. |
| `Slot Count` | `0` | Number of pocket slots granted. `0` disables the Safe Pocket entirely for that tier. |
| `Open Delay (seconds)` | `3` | Seconds the pocket takes to finish opening. `0` opens instantly. |
| `Priority (higher wins when multiple tiers apply)` | `0` | Resolution priority. |
| `Allowed Item Categories` | Resources, Component, Misc | Item categories that can be placed in the pocket. |
| `Item Blacklist (shortnames)` | *(empty)* | Item shortnames always rejected, regardless of category. |

### Permission Tier Overrides

A list of tiers, each gated behind its own permission. Any field left unset on a tier falls back to the matching value in `Default Settings`. Add as many tiers as you want — these keys are the same as `Default Settings` above, plus `Permission Required` to gate the tier.

### UI

| Key | Default | Description |
|---|---|---|
| `Slots Shown Before Overflow Wrap` | `6` | How many slots are shown before the preview wraps to an overflow `+N` indicator. |
| `Effect - Opening Safe Pocket (empty disables)` | beartrap rustle fx | Effect played when opening starts. Empty string disables it. |
| `Effect - Safe Pocket Finished Opening (empty disables)` | cloth impact fx | Effect played when opening finishes. Empty string disables it. |
| `Show Opening Progress Bar` | `true` | Whether the animated fill bar shows during the opening delay. |
| `Show Item Toast Messages` | `true` | Whether moving items in/out of the pocket shows the vanilla pickup/loss toast. |
| `Button Position - Anchor X (0-1)` / `Button Position - Anchor Y (0-1)` | `0.5` / `0` | Anchor point for the Safe Pocket button. |
| `Button Position - Offset Left` / `Button Position - Offset Up` | `532` / `170` | Pixel offset from the anchor point. |
| `Show Button Title` | `true` | Whether "SAFE POCKET" text shows above the button. |
| `Show Button Tooltip` | `true` | Whether hovering the button shows the explanatory tooltip. |

## Commands

| Command | Description |
|---|---|
| `safepocket.open` (console command, bound to the UI button) | Opens or closes the caller's Safe Pocket. |
