# Debug tools (`diagnostics`)

## What it does

This module adds the **Debug** page to the overlay. It holds troubleshooting tools: use them to check a problem in a game, or to collect information for a bug report. Nothing here changes how the game looks or runs until you use a tool. Leave them off in normal play.

## Where to find it

Overlay, **Debug** page (in the Advanced group). It shows:

- **UI layer tools:** the HUD-less view and the detected-UI view, the trace and the difference snapshot. They are described in the [ui-layer](ui-layer.md) page.
- **Motion-vector dilation probe:** a troubleshooting choice for ghosting. It shows how the game declares its motion vectors ("dilated" or "raw", or "not seen yet"), and lets you choose what is declared to DLSS Frame Generation: **Game's value**, **Declare raw** or **Declare dilated**. It changes only the declaration, never the vectors. Compare the two forced settings: if the picture changes, the setting matters for the effect you are checking. Set it to **Game's value** again to go back.

## Settings

| Key | Section | Default | Values | What it does | Applies |
|---|---|---|---|---|---|
| `MvDilatedOverride` | `[framegen]` | `-1` | `-1`, `0`, `1` | The choice of the motion-vector probe: `-1` = the game's value, `0` = declare raw, `1` = declare dilated. | Live |
| `TagLogHotkey` | `[diagnostics]` | `0` | a key code | A key that records which buffers the game hands to NVIDIA Streamline, and writes one table to `jmod.log` at the end. Press it again to stop early. `0` = none. It needs `DebugHotkeys=1` in `[core]`, and a restart after you change it. | Restart the game |
| `TagLogFrames` | `[diagnostics]` | `120` | `1` to `600` | How many frames the Streamline tag log records. | Restart the game |

## Hotkeys

- **Streamline tag log** (`TagLogHotkey`): set it on the Controls page, in the **Debug tools** group. The debug keys bind only with `DebugHotkeys=1`.

## Needs

- The [framegen](framegen.md) module, for the motion-vector probe and the tag log.
- The [ui-layer](ui-layer.md) module, for the UI layer tools.

## Known limits

- The tools are for troubleshooting. A key bound in `[diagnostics]` or in [ui-layer](ui-layer.md) is read at launch, so restart the game after you change it.
- Some tools cost a little CPU while they record.
