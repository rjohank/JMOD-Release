# XeSS Frame Generation (`xefg`)

## What it does

This module makes the frames between two real frames with Intel's XeSS Frame Generation (XeFG) instead of DLSS Frame Generation. The game keeps using DLSS Frame Generation as usual: its pacing, Reflex, the multiplier and Dynamic MFG stay the same, and only the generated pixels come from XeFG. It can generate up to 6x (five generated frames per real frame), because NVIDIA Streamline still does the pacing and presenting. It runs on the tensor cores of your RTX card and works in DirectX 12 and, with [Vulkan support](../install.md#vulkan-games) turned on, in Vulkan games.

It offers two methods, which you pick on the overlay's Frame Generation page:

| Method (overlay name) | What it is |
|---|---|
| XeMFG | Intel's 2x model: one pass for each generated frame. |
| XeMFG XMX | Intel's multi-frame model: all generated frames between two real frames in one pass. Lower GPU cost at high multipliers. |

You can switch between these methods and DLSS Frame Generation while the game runs.

## Where to find it

Overlay, **Frame Generation** page. Choose XeMFG or XeMFG XMX as the method, and the options below appear. Less common options are under **Advanced**. Without the Frame Generation page, the module has its own **XeSS Frame Generation** page with the same options.

| Option | What it does |
|---|---|
| UI Compose | Keeps the HUD crisp in generated frames. Needs HUD data from the game or from the UI Layer. |
| Interpolate HUD | Moving HUD elements also move in the generated frames. Replaces UI Compose while on. |
| HUD interpolation preset | **Light** (lowest GPU cost), **Quality** (smoothest, most GPU time), **Performance** (close to Quality for less GPU time). |
| Generate all frames of a pair together (Advanced) | Roughly halves the GPU time at 4x. Turn it off only if you see problems. |
| Use tensor cores (Advanced) | Speeds up XeSS Frame Generation on NVIDIA GPUs. |
| HUD debug view, Save diagnostics, Log options (Advanced) | For bug reports. See below. |

The **Status** section shows whether the XeSS library is ready (and its version), which method is running, the state of the HUD preset, and any problem. If the library could not be loaded, **Retry loading the library** tries again. If a HUD preset failed, **Retry the HUD preset** tries again.

## Settings

All settings are in the `[xefg]` section of `jmod\jmod.ini`. Most are also in the overlay. Settings marked "live" apply while the game runs.

| Key | Default | Values | What it does | Applies |
|---|---|---|---|---|
| `XeFGReplaceDlssg` | `0` | `0`, `1` | `1` makes XeFG produce the generated frames. Set by choosing XeMFG or XeMFG XMX in the overlay. | When the game next creates frame generation. Set it before launching the game. |
| `XeFGModel` | `0` | `0`, `1` | `0` = 2x model (XeMFG). `1` = multi-frame model (XeMFG XMX). If the multi-frame model cannot start, the 2x model is used and `jmod.log` says why. | Next XeFG start |
| `XeFGMaxGeneratedFrames` | `5` | `1` to `5` | The most frames generated per real frame (the multiplier is this plus 1, so `5` = 6x). The multiplier you use is chosen on the Frame Generation page. | Next XeFG start |
| `XeFGUiComposition` | `1` | `0`, `1` | UI Compose: keeps the HUD crisp in generated frames. Starts once two real frames with HUD data were captured. | Live |
| `XeFGHudInterpolation` | `0` | `0`, `1`, `2` to `8` | `1` = interpolate the HUD (replaces `XeFGUiComposition`). `2` to `8` = HUD debug views 1 to 7. | Live, from the next frame |
| `XeFGHudPreset` | `0` | `0`, `1`, `2` | `0` = Light, `1` = Quality, `2` = Performance. | Live |
| `XeFGBatchExecute` | `1` | `0`, `1` | `1` = all generated frames of a pair in one pass. `0` = one pass per generated frame (a fallback). | Live, from the next pair |
| `XeFGTensorCores` | `1` | `0`, `1` | `1` = run part of XeFG on the tensor cores. `0` = XeFG's own shaders only. | Next XeFG start |
| `XeFGReuseGenerator` | `1` | `0`, `1` | `1` = when the game releases frame generation and creates it again right away (a menu, a window resize), keep XeFG's state and reuse it instead of rebuilding. This skips the ramp back to the full multiplier. A changed resolution, frame count or method still builds a new one. `0` = always rebuild. | Live |
| `XeFGLibraryPath` | empty | a path | Use this `libxess_fg.dll` instead of the one in `jmod\xefg`. Empty = the `jmod\xefg` folder. | Next library load |
| `XeFGHudDiagHotkey` | `0` | a key code | A key that saves diagnostic data of the next 6 real frames to `jmod\hud-diag` (HUD interpolation must be on). `0` = none. Set it on the Controls page. | Live |
| `XeFGHudTiming` | `0` | `0`, `1` | Logs the time of each HUD interpolation step every 10 seconds, and shows the time per real and per generated frame on the page. | Live |
| `XeFGProfile` | `0` | `0`, `1` | Measures where the GPU time goes, writes a summary to `jmod.log` every 10 seconds and shows the top steps on the page. Costs a little GPU time. Set it before launching the game, or toggle the game's frame generation. | Live |

## Hotkeys

- **Save HUD diagnostics (6 frames)**: set on the Controls page (`XeFGHudDiagHotkey`). Does the same as the **Save diagnostics (6 frames)** button under Advanced. Attach the saved folder to a bug report.

The multiplier hotkeys are part of the Frame Generation page, not this module.

## Needs

- An NVIDIA GeForce RTX 40 series GPU.
- Intel's `libxess_fg.dll` in `jmod\xefg`. Version 1.3.4.95 is included in the zip. The versions 1.3.1.78 and 1.3.1.68 are also supported. You can use a different copy with `XeFGLibraryPath`.
- A game that uses DLSS Frame Generation through Streamline.
- Vulkan games: `VulkanBridge=1` in `[vk-bridge]`, see [install.md](../install.md#vulkan-games). Until then DLSS Frame Generation is used.

## Known limits

- Changes to `XeFGReplaceDlssg` apply when the game creates frame generation, so set it before launching, or toggle frame generation in the game's options.
- HUD interpolation needs HUD data from the game or from the UI Layer. The Status section shows "Waiting for HUD data" until it has some.
- `XeFGReuseGenerator` keeps its state until you switch to DLSS Frame Generation or close the game, so frame generation turned off in the game's own options keeps that memory until then.
