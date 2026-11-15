# DirectX 11 support for frame generation (`dx11-bridge`)

## What it does

Frame generation and DLSS Neural Rendering need JMOD to control how the game's image reaches the screen. In a DirectX 11 game that is not the case by default. When you turn this module on, JMOD takes over the presentation of a DirectX 11 game, so that these features can run there. The game itself runs as before.

It does nothing in DirectX 12 and Vulkan games, and it is off by default. Even when on, it acts only on a DirectX 11 game's display.

## Where to find it

- Overlay, **Status**: the "D3D11 bridge" line shows the state:

| Status line | Meaning |
|---|---|
| `D3D11 bridge: off` | The setting is off. |
| `D3D11 bridge: on, no D3D11 swapchain yet` | On, and waiting for the game to create its display. The game may not be a DirectX 11 game. |
| `D3D11 bridge: on for frame generation, no D3D11 swapchain yet` | The same, with frame generation added by [fg-inject](fg-inject.md). |
| `D3D11 bridge: <width>x<height> through D3D12` | Working. `, with frame generation` is added when frame generation runs. |
| `D3D11 bridge: swapchain made natively (<reason>)` | JMOD did not take over this display, and the game runs unchanged. The reason is shown. |

- Overlay, **Upscaler** page, **Injection** section ([sr-inject](sr-inject.md)): shows whether `D3D11Bridge` is on.

## Settings

| Key | Section | Default | Values | What it does | Applies |
|---|---|---|---|---|---|
| `D3D11Bridge` | `[dx11-bridge]` | `0` | `0`, `1` | `1` = take over the presentation of a DirectX 11 game, so that frame generation and Neural Rendering can run in it. Without `InjectFrameGen=1` it changes only the presentation, with no frame generation. | Restart the game |

## Hotkeys

None.

## Needs

- A DirectX 11 game on an NVIDIA RTX 40 series GPU.
- For frame generation: `InjectFrameGen=1` in `[fg-inject]` and NVIDIA's Streamline files in `jmod\streamline` (see [fg-inject](fg-inject.md)), and a DLSS source for the game: its own, or [sr-inject](sr-inject.md) for a game without one.
- For Neural Rendering in a DirectX 11 game: `nvngx_dlssnr.dll` in `jmod\streamline` ([neural-rendering.md](../neural-rendering.md)).

## Known limits

- The setting is read when the game starts.
- Direct3D 11 only. In other games, the setting has no effect.
- If the status line says "swapchain made natively", the game runs unchanged and features that need the bridge are not available in that game.
