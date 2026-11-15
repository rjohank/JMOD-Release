# Performance graphs (`perf`)

## What it does

This module shows live performance graphs in a strip at the bottom of the overlay panel:

- **Output frame time:** the frame time the display gets, with the frame rate and its average, and the 1% and 0.1% lows, the worst frame time and the standard deviation.
- **Game frame time:** the game's own frame time, before frame generation, with the same pacing figures.
- **Upscaler GPU time:** the GPU time of the game's DLSS upscaler.
- **Frame generation GPU time:** the GPU time of frame generation, per evaluate (DLSS Frame Generation or XeFG).

The graphs measure each graphics API separately. They cost nothing while the overlay panel is closed. The readings are also used by the [on-screen display](osd.md), which needs this module for frame rates, lows, bottleneck, graphs and GPU times.

## Where to find it

Overlay, the **strip under the panel**. Click its header to collapse it to one line (the summary), and click again to open it. The graphs keep recording while it is collapsed.

The strip says when a reading is not available:

| Message | Meaning |
|---|---|
| `<name>: collecting...` | Not enough samples yet. |
| Upscaler / Frame generation GPU time: needs the ngx module | The ngx module is not loaded. |
| Frame generation GPU time: not measurable in this game | The game's graphics API does not allow the measurement. |
| Frame generation GPU time: no DLSS frame generation through NGX | The game is not running DLSS Frame Generation. |

## Settings

| Key | Section | Default | Values | What it does | Applies |
|---|---|---|---|---|---|
| `StripCollapsed` | `[perf]` | `0` | `0`, `1` | `1` = the strip shows one summary line instead of the graphs. Click the strip's header to toggle it. | Live |

## Hotkeys

None.

## Needs

- The graphs for the upscaler and for frame generation need the `ngx` module.
- The overlay panel must be open for the GPU time measurements to run.

## Known limits

- There is no render latency reading.
- GPU time of frame generation is shown per evaluate, not per displayed frame.
