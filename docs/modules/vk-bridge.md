# Vulkan support (`vk-bridge`)

## What it does

This module lets features that normally run only in DirectX 12 games work in a Vulkan game: XeSS Frame Generation ([xefg](xefg.md)), DLSS Neural Rendering ([neural-rendering.md](../neural-rendering.md)), and the UI Layer's HUD-less image for HUD interpolation ([ui-layer](ui-layer.md)).

It is off by default, and it does nothing by itself until one of these features uses it. It does nothing in DirectX 11 and DirectX 12 games.

If it cannot work for a game (it is off, the graphics driver lacks something it needs, or a frame cannot be handled), the game keeps running on its own path: DLSS Frame Generation stays NVIDIA's, Neural Rendering stays off, and `jmod.log` has one line saying why. If work gets stuck for a few seconds, the module turns itself off for that session and the game continues.

## Where to find it

- Overlay, **Status**, **Vulkan bridge** lines:

| Status line | Meaning |
|---|---|
| `Vulkan bridge: off` | The setting is off. |
| `Vulkan bridge: on, waiting for a Vulkan device` | On, and waiting for the game to create its graphics device. |
| `Vulkan bridge: refused (<reason>)` | It cannot work for this game. The reason is shown, and the game runs on its own path. |
| `Vulkan bridge: ready` | It works. |
| `Device: <name>, LUID <id>` | The graphics device in use. |
| `Device additions: <n>` | How many extra device features JMOD asked the driver for when the game created its device. `none needed` when the game already had them. |
| `Shared textures: <n> alive, <size>` | How many shared textures are alive and how much memory they use. They are freed a couple of seconds after the last feature stops using them. |

- The XeSS Frame Generation and Neural Rendering options say when a Vulkan game is waiting for the bridge.

## Settings

| Key | Section | Default | Values | What it does | Applies |
|---|---|---|---|---|---|
| `VulkanBridge` | `[vk-bridge]` | `0` | `0`, `1` | `1` = turn on the Vulkan support. | Restart the game. It acts when the game creates its graphics device. |

If `jmod\jmod.ini` does not exist yet, start the game once and close it, then edit the file.

## Hotkeys

None.

## Needs

- A Vulkan game that uses DLSS Frame Generation through NVIDIA Streamline, on an NVIDIA GPU.
- Keep the whole `jmod\modules` folder as it came in the zip.
- `libxess_fg.dll` in `jmod\xefg` for XeSS Frame Generation, and `nvngx_dlssnr.dll` in `jmod\streamline` for Neural Rendering.

## Known limits

- NVIDIA GPUs only, and only games that use DLSS Frame Generation through Streamline.
- A change to `VulkanBridge` needs a restart.
- It uses extra VRAM for the shared textures (freed when idle) and adds a small per-frame cost while a feature uses it.
- The overlay in Vulkan games runs in layered mode.
- Dynamic MFG, VSync control and the flip-metering workaround of the [framegen](framegen.md) page are DirectX 12 only.
