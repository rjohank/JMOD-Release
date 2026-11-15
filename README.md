# JMOD: Multi Frame Generation for RTX 40 Series

JMOD brings DLSS Multi Frame Generation and Intel's XeSS Multi Frame Generation to GeForce RTX 40 series cards. It also adds DLSS Neural Rendering, and it can add DLSS to games that ship without it. It works in DirectX 11, DirectX 12 and Vulkan games.

## Latest release

**Version 1.4.0.0**

- XeSS Frame Generation (XeMFG and XeMFG XMX up to 6x, HUD interpolation, live switching) now also works in Vulkan games.
- DLSS Neural Rendering in Vulkan games, before or after Super Resolution.
- The UI Layer gives frame generation a HUD-less image in Vulkan games.
- New `[vk-bridge] VulkanBridge=1` setting turns on the Vulkan support (restart to apply). It is off by default and has no effect in DirectX games.
- Frame generation live switching also works in Vulkan games.

Download: [GitHub Releases](https://github.com/rjohank/JMOD-Release/releases) or [Nexus Mods](https://www.nexusmods.com/site/mods/2575). Full history: [CHANGELOG.md](CHANGELOG.md).

## What it does

You set everything up in an in-game overlay. You can change the multiplier while you play, and switch frame generation off at any time.

**DLSS Frame Generation (DLSS-G).** NVIDIA's own frame generation, running on your RTX 40 card, with Multi Frame Generation multipliers and Dynamic MFG.
- Image quality presets: **Preset 1** (default, for games with a separate HUD layer), **Preset 2** (HUD is part of the image), **Preset 3** (conservative fallback).
- Supported DLSS-G versions: 310.9.0, 310.9.1 and 310.9.16.
- Pacing options (VSync / Reflex) and hotkeys for the multiplier (up / down) are in the overlay.

**XeFG (XeSS Frame Generation).**
- Two methods: XeMFG (2x model, one pass per generated frame) and XeMFG XMX (multi-frame model, one pass per pair), up to 6x.
- Runs on the tensor cores of your RTX card, for a lower GPU cost.
- HUD interpolation with three presets: Light (lowest cost), Performance and Quality (smoothest).
- Live switching between XeMFG, XeMFG XMX and DLSS-G while the game runs.
- Supported `libxess_fg.dll` versions: 1.3.4.95, 1.3.1.78 and 1.3.1.68. Version 1.3.4.95 is included in the download.

**UI Layer.** Gives frame generation a HUD-less image, so the HUD stays sharp and does not smear in generated frames. It works with both DLSS-G and XeFG in DirectX 12 and Vulkan games. In games that do not provide a HUD-less image, the UI Layer can build one.

**DLSS Neural Rendering.** Adds NVIDIA's DLSS Neural Rendering on top of the game's DLSS Super Resolution or Ray Reconstruction. See [docs/neural-rendering.md](docs/neural-rendering.md).

**Frame generation for games that do not have it.** Games that use DLSS Super Resolution but have no frame generation can get it through JMOD. DirectX 11 games that ship no DLSS at all can run DLSS (DLAA, at native resolution) through JMOD, with frame generation and Neural Rendering on top. See [docs/install.md](docs/install.md).

## Supported hardware

- NVIDIA GeForce RTX 40 series (Ada) GPUs only
- Windows 10 / 11, 64-bit games
- DirectX 11, DirectX 12 and Vulkan

## Quick start

1. Download the one zip from [Releases](https://github.com/rjohank/JMOD-Release/releases) or [Nexus Mods](https://www.nexusmods.com/site/mods/2575).
2. Extract it into your game's folder, next to the game `.exe`. You get `winmm.dll` and a `jmod` folder.
3. Start the game. Open the overlay (the key is shown on first start) and set up frame generation.

**Update:** replace `winmm.dll` and the whole `jmod\modules` folder together. Your `jmod\jmod.ini` and `jmod\streamline` folder are kept.

**Uninstall:** delete `winmm.dll` and the `jmod` folder. Nothing else is changed.

If the game already uses a `winmm.dll`, rename ours to `version.dll` or `dxgi.dll` (use only one).

**Files you supply.** NVIDIA's files are not included. Depending on what you use, put them in `jmod\streamline`: the Streamline DLLs for frame generation in games that do not have it, `nvngx_dlss.dll` for DLSS in DirectX 11 games without it, and `nvngx_dlssnr.dll` for Neural Rendering. Details are in [docs/install.md](docs/install.md).

## Vulkan games

XeFG and DLSS Neural Rendering also work in Vulkan games that use DLSS Frame Generation. Set `VulkanBridge=1` in the `[vk-bridge]` section of `jmod\jmod.ini` and restart the game. See [docs/install.md](docs/install.md#vulkan-games).

## Documentation

- [Install guide](docs/install.md): what is in the zip, files you supply, Vulkan games, safe mode, uninstall
- [DLSS Neural Rendering](docs/neural-rendering.md)
- [Changelog](CHANGELOG.md)
- [Third-party notices](THIRD-PARTY-NOTICES.md)

## Releases

Each release is one zip, `JMOD-MFG-RTX40-v<version>.zip`, on the [Releases](https://github.com/rjohank/JMOD-Release/releases) page. The source is not published. GitHub shows the SHA-256 of each file on the release page.

## Reporting a problem

Open an [issue](https://github.com/rjohank/JMOD-Release/issues) and include:

- `jmod\jmod.log` from the game folder
- your GPU
- your graphics driver version
- the mod version

## Credits

Projects:
- [OptiScaler](https://github.com/optiscaler/OptiScaler)
- [MFG-Unlocker](https://github.com/dashdogy/MFG-Unlocker)
- [dlssg-to-xefg-xmx](https://github.com/realdody/dlssg-to-xefg-xmx)
- [DLSSG-Transfusion](https://github.com/TonyJoaca/DLSSG-Transfusion)
- [MFGAdaUnlock-RenoDx](https://github.com/mavismmg/MFGAdaUnlock-RenoDx)

SDKs and libraries:
- [NVIDIA Streamline](https://github.com/NVIDIA-RTX/Streamline)
- [NVIDIA DLSS SDK](https://github.com/NVIDIA/DLSS)
- [Intel XeSS SDK](https://github.com/intel/xess)
- [Microsoft Detours](https://github.com/microsoft/Detours)
- [Dear ImGui](https://github.com/ocornut/imgui)
- [nlohmann/json](https://github.com/nlohmann/json)
- [Vulkan-Headers](https://github.com/KhronosGroup/Vulkan-Headers)
- [gtl](https://github.com/greg7mdp/gtl)
- [frozen](https://github.com/serge-sans-paille/frozen)

## Disclaimer

JMOD is free for personal use and is provided as is, without warranty. It is not made, endorsed or supported by NVIDIA, Intel or any game developer. All product names and trademarks belong to their owners. Use it at your own risk, and not in games or modes where modifying the game is not allowed, such as online play with anti-cheat. See [LICENSE](LICENSE).
