# Interpolation quality (`kernels`)

## What it does

This module improves how DLSS Frame Generation builds the frames between two real frames:

- thin geometry and content entering from off-screen are not smeared;
- generated frames land at their own point in time instead of all at the halfway point (the **temporal fix**).

It applies when you use DLSS-G as the frame generation method. It has no effect when XeSS Frame Generation ([xefg](xefg.md)) makes the generated frames.

It is on by default and needs no setup. You only choose a preset when the default does not suit a game.

## Where to find it

- Overlay, **Frame Generation** page, **Interpolation** section (shown for the DLSS-G method). The header shows "applied" or "not applied yet".
- Overlay, **Home** page: an **Interpolation preset** choice.
- Overlay, **Diagnostics** page: **Interpolation**.

The section has:

- **Kernel patch:** the on/off switch, the state ("Applied" or "Not applied yet"), and the **Preset** choice.
- **Temporal fix:** the temporal fix switch and its state.
- **A/B switch:** two saved selections to flip between with a key.

### Presets

| Preset | For |
|---|---|
| **Preset 1** (default) | The most complete fix: thin geometry and content entering from off-screen are not smeared. Made for games that draw their HUD as a separate layer. In a game that bakes the HUD into the image, the HUD is not protected, and it can smear or flicker while frames are generated. Try this first. |
| **Preset 2** | Games that bake the HUD into the image: it protects the HUD while frames are generated. It does not include Preset 1's fix for content entering from off-screen. Pick it when Preset 1 damages the HUD. |
| **Preset 3** | The mildest change, a conservative fallback. Pick it when the other presets show artifacts in your game. It fixes the least. |

The preset list also holds a few debug entries that replace the image with a measurement. They are not for playing.

The **Advanced** group lets you override single values of the preset. It shows "custom" once a value differs from every preset. Only combinations that are included with JMOD are offered. For any other, the game uses its unmodified output.

## Settings

All settings are in the `[kernels]` section of `jmod\jmod.ini`.

| Key | Default | Values | What it does | Applies |
|---|---|---|---|---|
| `KernelPatch` | `1` | `0`, `1` | The master switch. `0` = off: DLSS Frame Generation produces its stock, uncorrected output. | Live from the next frame when possible, otherwise at the next game start (see Known limits) |
| `KernelPatchPreset` | `ui-layer` | `ui-layer`, `no-ui-layer`, `weights-only` | The preset: `ui-layer` = Preset 1, `no-ui-layer` = Preset 2, `weights-only` = Preset 3. | As above |
| `KernelPatchTemporal` | `1` | `0`, `1` | The temporal fix. Recommended on. Turn it off only to compare against the uncorrected behaviour. | Restart the game |
| `KernelPatchABHotkey` | `0` | a key code | A key that flips between the two A/B selections. `0` = none. Set it on the Controls page. | Live |

The overlay's **A/B switch** has **Set A from current** and **Set B from current**. Each slot shows the saved selection, and "(active)" for the one in use.

## Hotkeys

- **Flip Interpolation slot A / B** (`KernelPatchABHotkey`): set on the Controls page. It is ignored while the overlay panel is open.

## Needs

- DLSS-G as the frame generation method, on an NVIDIA GeForce RTX 40 series GPU.
- `kernels.jmod` in `jmod\modules`. It is part of the zip.
- If `kernels.jmod` is missing, everything else still works, and you only lose this module's corrections.

## Known limits

- A change of the preset or the switch applies live (from the next frame) when the game allows it. Otherwise it applies at the next game start. The **Takes effect** line under Advanced shows which one applies.
- The corrections are made for specific versions of NVIDIA's DLSS Frame Generation files. When the game ships a version this module does not know, the output stays stock, and the section shows "Not applied yet".
- A preset that does not suit a game can show artifacts. Switch to another preset, or turn the module off, to compare.
