# Sharpening (`sharpen`)

## What it does

A small neural network sharpens the image right after DLSS, on every frame DLSS upscales (the game's own Super Resolution or Ray Reconstruction), before frame generation reads it. Only brightness is sharpened, so colours stay. It works in DirectX 12 games, in DirectX 11 games with `D3D11Bridge=1` and in Vulkan games with `VulkanBridge=1`. It is off by default, and costs nothing while it is off.

The network is based on [Barbatos Neural Sharpening](https://github.com/BarbatosAWLS/Reshade-Shaders) by Barbatos (MIT license).

## Where to find it

Overlay, **Sharpening** page. Every setting applies at once.

## Settings

All settings are in the `[sharpen]` section of `jmod\jmod.ini`.

| Key | Default | Values | What it does | Restart |
|---|---|---|---|---|
| `Sharpen` | `0` | `0`, `1` | `1` = sharpen. | No |
| `SharpenStrength` | `1` | `0` to `2` | How strongly to sharpen. | No |
| `SharpenModel` | `0` | `0`, `1` | `0` = model A (standard), `1` = model B (edge-enhanced). | No |
| `SharpenEdgeResponse` | `0` | `-1` to `1` | Below `0` is softer on edges, above `0` is stronger on them, `0` is the same everywhere. | No |
| `SharpenMotionAdaptive` | `0` | `0`, `1` | `1` = sharpen moving pixels more (DLSS keeps less detail on them), up to 1.5x the strength on fast motion. | No |
| `SharpenDebugView` | `0` | `0`, `1`, `2` | `1` = sharpening map (grey = unchanged). `2` = map of the detail the network finds (brighter = stronger edge). | No |

## Hotkeys

None.

## Needs

- A game that uses DLSS Super Resolution or Ray Reconstruction (or DLSS injection). Without a DLSS upscale there is nothing to sharpen.
- In DirectX 11 games, `D3D11Bridge=1`. In Vulkan games, `VulkanBridge=1`.

## Known limits

- Only frames DLSS upscales are sharpened.
- Very high strength can show halos around bright edges.

## Log lines

Lines are in `jmod\jmod.log`.

| Line | Meaning |
|---|---|
| `jmod: sharpen: running on <w>x<h> ...` | Sharpening started on frames of that size. |
| `jmod: sharpen: skipped: <reason>.` | A frame was left as DLSS made it, for the reason given. Logged once per reason. |
| `jmod: sharpen: off until switched off and on again: <reason>.` | Sharpening stopped after a failure. Switch it off and on in the overlay to retry. |
