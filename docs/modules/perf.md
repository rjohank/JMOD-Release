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

### Performance page

The **Performance** page shows the game's frame pipeline, read from the graphics driver's latency report (DirectX 11, DirectX 12 and Vulkan games that report it). It has these sections:

- **Frame pipeline:** the time of each stage of the game's frame, with the median time from input to GPU done.
- **CPU and GPU busy:** how long each is busy per frame, next to the game's frame time.
- **Frame times:** the output frame time and the game's own.
- **GPU time** of the upscaler and of frame generation, per evaluate.
- **PC latency (experimental):** the time from your input to the picture on screen, split into input to simulation start and simulation start to screen. It needs a game that reports latency markers and answers latency pings, shows `--` until one arrives, and is not available in Vulkan games. The same reading can be shown in the [on-screen display](osd.md).

The page shows the panel's own cost per frame. Nothing runs while the page is closed.

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

- PC latency is experimental and not available in Vulkan games.
- GPU time of frame generation is shown per evaluate, not per displayed frame.
