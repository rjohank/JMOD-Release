# Modules

JMOD is made of modules: one `.jmod` file each, in `jmod\modules`. Keep the whole folder together. Each page below lists what the module does, where to find it in the overlay, its settings in `jmod\jmod.ini`, hotkeys, what it needs and its limits.

## Frame generation

| Module | What it does |
|---|---|
| [framegen](modules/framegen.md) | Multiplier (2x to 6x) or Off, frame generation method, Dynamic MFG, VSync and Reflex pacing. |
| [xefg](modules/xefg.md) | XeSS Frame Generation (XeMFG and XeMFG XMX), with HUD interpolation. |
| [unlock](modules/unlock.md) | Makes Multi Frame Generation available on RTX 40 series cards. |
| [kernels](modules/kernels.md) | Interpolation quality: presets for thin geometry and content entering from off-screen. |
| [ui-layer](modules/ui-layer.md) | Keeps the HUD sharp in generated frames. |

## Upscaling

| Module | What it does |
|---|---|
| [presets](modules/presets.md) | DLSS quality mode, upscale ratio, Super Resolution and Ray Reconstruction presets, DLSS Frame Generation preset. |
| [dlssnr](modules/dlssnr.md) | DLSS Neural Rendering. A short guide is in [neural-rendering.md](neural-rendering.md). |
| [sharpen](modules/sharpen.md) | Neural sharpening of the image right after DLSS. |

## Games without DLSS or frame generation

| Module | What it does |
|---|---|
| [fg-inject](modules/fg-inject.md) | Adds frame generation to a DirectX 12 game that has none. |
| [sr-inject](modules/sr-inject.md) | Runs DLSS (DLAA) in a DirectX 11 game that has none. |

## Graphics API support

| Module | What it does |
|---|---|
| [dx11-bridge](modules/dx11-bridge.md) | Lets frame generation and Neural Rendering run in DirectX 11 games. |
| [vk-bridge](modules/vk-bridge.md) | Lets XeSS Frame Generation and Neural Rendering run in Vulkan games. |

## Display and tools

| Module | What it does |
|---|---|
| [osd](modules/osd.md) | On-screen display of performance and hardware readings. |
| [perf](modules/perf.md) | Performance graphs in the overlay. |
| [window](modules/window.md) | Borderless window, snap, size and offset. |
| [diagnostics](modules/diagnostics.md) | The Debug page: troubleshooting tools. |
| [ngx](modules/ngx.md) | Tracks the game's DLSS features for the other modules. No settings. |

## Safe mode and turning modules off

See [install.md](install.md#safe-mode-and-turning-modules-off).
