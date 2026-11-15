# Upscaler overrides (`presets`)

## What it does

This module lets you change how the game's DLSS runs, without touching the game's own settings:

- force the **DLSS quality mode** (Performance, Balanced, Quality, Ultra Performance, Ultra Quality or DLAA);
- change the **upscale ratio**, which sets how far below the output resolution the game renders;
- choose the **render preset** of DLSS Super Resolution and of Ray Reconstruction;
- choose the **DLSS Frame Generation preset**.

The overrides apply to Super Resolution and to Ray Reconstruction. A game that has DLSS off stays off.

## Where to find it

- Overlay, **Upscaler** page: the quality mode, the upscale ratio, the Super Resolution and Ray Reconstruction presets, and a **Resolution** section.
- Overlay, **Frame Generation** page, **Frame generation preset** section: the DLSS Frame Generation preset, with its status.
- Overlay, **Diagnostics** page, **Upscaler**: the preset each feature was last created with.

The **Resolution** section shows the render size and the upscaled size the game created each feature with, and the percentage per axis. A game that uses dynamic resolution renders below the size shown.

## Settings

All settings are in the `[presets]` section of `jmod\jmod.ini`. The overlay shows `-1` as "Game's choice".

| Key | Default | Values | What it does | Applies |
|---|---|---|---|---|
| `UpscalerQualityOverride` | `-1` | `-1` to `5` | Forces the DLSS quality mode. `-1` = the game's own, `0` = Performance, `1` = Balanced, `2` = Quality, `3` = Ultra Performance, `4` = Ultra Quality, `5` = DLAA. The render size follows when the game asks for its settings again, which most games do after a settings or resolution change. Until then the game renders at its own size. | Live |
| `SrPresetOverride` | `-1` | `-1`, `0` to `15` | Forces the Super Resolution render preset, for every quality mode. `-1` = the game's own, `0` = default, otherwise the letter of the preset (`10` = J, `11` = K, `12` = L, `13` = M). The overlay offers the presets that are still available: Default, E and F (both deprecated), J, K, L and M. | Live in most games; otherwise when the game next recreates its upscaler |
| `RrPresetOverride` | `-1` | `-1`, `0` to `15` | The same for Ray Reconstruction. The overlay offers Default, D, E and F. | As above |
| `DlssgPresetOverride` | `-1` | `-1`, `1` to `3` | The DLSS Frame Generation preset. `-1` = the NVIDIA driver profile or the NVIDIA app decides. `1` = A (UI recomposition off), `2` = B (UI recomposition on), `3` = C (FG Lite, needs `nvngx_dlssg.dll` 310.9.16 or newer; older versions ignore it). Nothing is written to the driver profile. The status line on the Frame Generation page and on the Diagnostics page shows whether the preset was applied. | Restart the game |
| `UpscaleRatioOverrideAll` | `0` | `0`, `1` | `1` = use `UpscaleRatioAll` for every quality mode. It wins over the per-preset ratios. | Live, after the game asks for its settings again |
| `UpscaleRatioAll` | `1.5` | `1.0` to `3.0` | The ratio for every quality mode. | As above |
| `UpscaleRatioOverridePerQuality` | `0` | `0`, `1` | `1` = use the ratio of each quality mode below. | As above |
| `UpscaleRatioPerformance` | `2.0` | `1.0` to `3.0` | Ratio for Performance. | As above |
| `UpscaleRatioBalanced` | `1.724` | `1.0` to `3.0` | Ratio for Balanced. | As above |
| `UpscaleRatioQuality` | `1.5` | `1.0` to `3.0` | Ratio for Quality. | As above |
| `UpscaleRatioUltraPerformance` | `3.0` | `1.0` to `3.0` | Ratio for Ultra Performance. | As above |
| `UpscaleRatioUltraQuality` | `1.3` | `1.0` to `3.0` | Ratio for Ultra Quality. | As above |

The upscale ratio is the output size divided by the render size. `2.0` renders at half the output resolution on each axis. The defaults of the per-preset ratios are NVIDIA's own. DLAA always stays 1:1. The same ratios apply to Ray Reconstruction.

For the ratio to take effect, the game has to ask for its render size again. Change the quality mode or the resolution in the game to make it do so. A game that uses a fixed render scale of its own is not affected.

## Hotkeys

None.

## Needs

- A game that uses DLSS Super Resolution, Ray Reconstruction or DLSS Frame Generation. A game with DLSS off stays off.
- DLSS Frame Generation preset C needs `nvngx_dlssg.dll` 310.9.16 or newer.

## Known limits

- The upscale ratio is not applied until the game asks for its render size again.
- A game with its own fixed render scale ignores the ratio.
- The DLSS Frame Generation preset needs a restart of the game.
- A game with dynamic resolution renders below the render size shown on the page.
