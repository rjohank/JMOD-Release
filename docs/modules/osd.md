# On-screen display (`osd`)

## What it does

A display drawn over the game that shows live readings: frame rate, frame times, 1% and 0.1% lows, the bottleneck, GPU and CPU load, temperatures, clocks, power and memory, the cost of the upscaler and of frame generation, the upscale ratio, the resolution and more. You choose which readings show, in which order, and how the display looks and where it sits on screen.

It is off by default.

## Where to find it

- Overlay, **On-screen display** page. It has these sections: On-screen display (the switch and the row presets), Readings, Options, Appearance, Placement and Share settings.
- The keys that toggle the display and cycle the row presets are on the **Controls** page.

## Settings

The display is switched in the `[core]` section. Everything else is in the `[osd]` section of `jmod\jmod.ini`. Almost everything is also on the page, and changes apply live.

### Switch and hotkeys

| Key | Section | Default | Values | What it does |
|---|---|---|---|---|
| `FpsCounter` | `[core]` | `0` | `0`, `1` | `1` = show the display from launch. In the overlay: **Show on-screen display**. |
| `FpsHotkey` | `[core]` | `0` | a key code | A key that toggles the display in the game. `0` = none. It needs the overlay hotkey to be set. |
| `OsdPresetHotkey` | `[osd]` | `0` | a key code | A key that cycles the row presets: Minimal, Standard, Full. `0` = none. |

Key codes are decimal virtual-key codes. You can also bind both keys on the Controls page.

### Readings

Each reading is a row of the display, and `1` = shown. In the overlay: **Readings**.

| Group | Key (default) | Reading |
|---|---|---|
| Performance | `OsdFps` (`1`) | FPS |
| | `OsdFrameTime` (`0`) | Frame time |
| | `OsdGameFps` (`0`) | Game FPS, before frame generation |
| | `OsdLows` (`0`) | 1% / 0.1% lows |
| | `OsdBottleneck` (`0`) | Bottleneck |
| | `OsdSession` (`0`) | Session timer |
| Upscaler and frame generation | `OsdUpscale` (`0`) | Upscale ratio |
| | `OsdUpscaler` (`0`) | Upscaler GPU time |
| | `OsdFramegen` (`0`) | Frame generation GPU time |
| GPU | `OsdGpuName` (`0`) | GPU model |
| | `OsdGpuLoad` (`0`) | GPU load |
| | `OsdGpuTemp` (`0`) | GPU temperature |
| | `OsdGpuHotspot` (`0`) | GPU hot spot |
| | `OsdGpuFan` (`0`) | GPU fan |
| | `OsdGpuClock` (`0`) | GPU clock |
| | `OsdMemClock` (`0`) | VRAM clock |
| | `OsdGpuPower` (`0`) | GPU power |
| | `OsdGpuVoltage` (`0`) | GPU voltage |
| | `OsdVram` (`0`) | VRAM used, whole GPU |
| | `OsdGameVram` (`0`) | VRAM used by the game |
| CPU | `OsdCpuName` (`0`) | CPU model |
| | `OsdCpu` (`0`) | CPU load |
| | `OsdCpuTemp` (`0`) | CPU temperature |
| | `OsdCpuHotspot` (`0`) | CPU hot spot |
| | `OsdCpuClock` (`0`) | CPU clock |
| | `OsdCpuPower` (`0`) | CPU power |
| | `OsdCpuVoltage` (`0`) | CPU voltage |
| Memory | `OsdRam` (`0`) | RAM used |
| | `OsdGameRam` (`0`) | RAM used by the game |
| | `OsdCommit` (`0`) | Commit charge |
| Info | `OsdResolution` (`0`) | Resolution and graphics API |
| | `OsdClock` (`0`) | Time |
| | `OsdDate` (`0`) | Date |

| Key | Default | What it does |
|---|---|---|
| `OsdOrder` | all rows in the order of the table | The order of the rows, top to bottom: row names joined by commas (`fps,frametime,gamefps,lows,bottleneck,session,upscale,upscaler,framegen,gpuname,gpuload,gputemp,gpuhotspot,gpufan,gpuclock,memclock,gpupower,gpuvoltage,vram,gamevram,cpuname,cpu,cputemp,cpuhotspot,cpuclock,cpupower,cpuvoltage,ram,gameram,commit,resolution,clock,date`). Rows left out follow at the end. In the overlay, drag a row to move it. |

**Row presets**, in the overlay and on `OsdPresetHotkey`: **Minimal** (FPS), **Standard** (FPS, frame time, lows, GPU load, GPU temperature, VRAM, CPU load) and **Full** (every row).

### Options

| Key | Default | Values | What it does |
|---|---|---|---|
| `OsdFpsRow` | `0` | `0` to `2` | What the FPS row adds: `0` = nothing, `1` = frame time, `2` = the average FPS. |
| `OsdUpscalerName` | `1` | `0`, `1` | Shows DLSS and its version on the upscale row (or on the FPS row when that row is off). |
| `OsdFpsCost` | `0` | `0`, `1` | Shows how many FPS the upscaler and frame generation cost, on their rows. This assumes the GPU is the limit. It is lower when the CPU is the limit. |
| `OsdHoldMs` | `0` | `0` to `1000` | How long FPS and times are held before they change, in ms. The FPS is averaged over it. `0` = every frame. |
| `OsdLabels` | `1` | `0`, `1` | Shows labels. |
| `OsdCompactLabels` | `0` | `0`, `1` | Short labels and shorter values, for a narrower display. |
| `OsdGraph` | `0` | `0`, `1` | Shows a graph of the frame time. |
| `OsdUpscalerGraph` | `0` | `0`, `1` | Shows a graph of the upscaler time. |
| `OsdFramegenGraph` | `0` | `0`, `1` | Shows a graph of the frame generation time. |
| `OsdGraphHeight` | `1.00` | `0.5` to `3` | Height of the graphs, as a multiplier. Each graph sits at its row's place in the order: under the row when it is shown, on its own when not. "no data yet" means nothing was recorded for it. |
| `OsdUpdateMs` | `1000` | `250` to `5000` | Milliseconds between hardware sensor reads. |

### Appearance

| Key | Default | Values | What it does |
|---|---|---|---|
| `OsdFont` | empty | a file name or path | A `.ttf` or `.otf` file in `C:\Windows\Fonts` (or your own fonts folder), or a full path. Empty = the overlay panel's font. If the file is not found, the panel font is used. |
| `OsdFontSize` | `0` | `8` to `48` | Font size in pixels. `0` = the panel's size. |
| `OsdLetterSpacing` | `0.0` | `-2` to `8` | Extra pixels between characters. |
| `OsdLabelScale` | `1.00` | `0.5` to `2` | Label size, as a multiplier of the font size. |
| `OsdValueScale` | `1.00` | `0.5` to `2` | Value size, as a multiplier of the font size. |
| `OsdUseTheme` | `0` | `0`, `1` | `1` = use the overlay panel theme's colours. |
| `OsdLabelColor` | `16777215` | a number | Label colour, as the decimal number of `0xRRGGBB`. |
| `OsdValueColor` | `16777215` | a number | Value colour. |
| `OsdGraphColor` | `6737151` | a number | Graph colour. |
| `OsdBgColor` | `0` | a number | Background colour. |
| `OsdShadowColor` | `0` | a number | Shadow colour. |
| `OsdBgAlpha` | `0.35` | `0` to `1` | Background opacity. |
| `OsdShadow` | `0` | `0` to `2` | Text shadow: `0` = off, `1` = drop shadow, `2` = outline. |
| `OsdShadowStrength` | `0.40` | `0` to `1` | Shadow strength. |
| `OsdRounding` | `4` | `0` to `16` | Corner rounding, in pixels. |
| `OsdRowSpacing` | `2.0` | `0` to `32` | Space between rows, in pixels. |

The overlay also has six **Styles** (Default, Classic, Afterburner-like, Strip, Complete and Emerald). A style sets its look and its rows. Settings it does not name go back to their defaults, and the position stays.

### Placement

| Key | Default | Values | What it does |
|---|---|---|---|
| `OsdLayout` | `0` | `0`, `1` | `0` = the rows as a list. `1` = one horizontal bar with short labels, in the row order. Anchor the bar to the top or bottom edge. |
| `OsdAnchor` | `0` | `0` to `8` | Where the display is anchored: `0` top-left, `1` top, `2` top-right, `3` left, `4` center, `5` right, `6` bottom-left, `7` bottom, `8` bottom-right. |
| `OsdOffsetX` | `8` | `-2000` to `2000` | Pixels inward from the anchored edge. On a centered axis, it shifts right. |
| `OsdOffsetY` | `8` | `-2000` to `2000` | The same, vertically. On a centered axis, it shifts down. |
| `OsdScale` | `1.00` | `0.5` to `3` | Size of the whole display. |

In the overlay, **Move on screen** lets you drag the display with the mouse while the panel is open. It picks the nearest of the nine anchors and sets the offsets. It needs the display to be on, and it is not saved: it turns off when the game restarts.

### Share settings

**Copy code** puts the whole display (rows, order, options, look and placement) on the clipboard as one line of text that starts with `jmod-osd:`, to keep or to share. **Paste code** applies a copied code. Settings the code does not name go back to their defaults.

## Hotkeys

- **Toggle the display** (`FpsHotkey`) and **Cycle on-screen display preset** (`OsdPresetHotkey`): set them on the Controls page.

## Needs

- Frame rates, lows, the bottleneck, the graphs and the GPU times need the `perf` module. The upscale ratio needs the `ngx` module.
- GPU readings come from the NVIDIA driver, for the first NVIDIA GPU. The hot spot is not reported by RTX 50 series cards.
- CPU temperature, hot spot, power and voltage need HWiNFO (with Shared Memory Support on), MSI Afterburner or Core Temp running. Without one, they read "n/a".
- Game VRAM is available in DirectX games only.

## Known limits

- The display needs the overlay hotkey to be set for the toggle key to work.
- The hardware readings update at the `OsdUpdateMs` interval, not every frame.
