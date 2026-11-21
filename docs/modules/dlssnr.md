# DLSS Neural Rendering (`dlssnr`)

## What it does

This module runs NVIDIA's DLSS Neural Rendering on top of the image the game's own DLSS Super Resolution or Ray Reconstruction produces, for a cleaner and more detailed picture. It can run after Super Resolution (on the upscaled image) or before it (on the colour input, which is cheaper). It works in DirectX 11, DirectX 12 and Vulkan games.

It is off by default. For the install steps and the short settings overview, see [neural-rendering.md](../neural-rendering.md). This page lists every option.

## Where to find it

Overlay, **Neural Rendering** page. Use **Save to ini** (bottom of the sidebar) to keep your changes. The page has these sections:

- **Neural Rendering:** the on/off switch, the state, the cost, the toggle hotkey.
- **Model:** placement, number of passes, working scale and tuning.
- **Composition:** how the model's result is blended into the frame.
- **Debugging:** views and A/B comparison.
- **Presets:** saved looks (see below).

The state shows one of:

| State | Meaning |
|---|---|
| Waiting for the game's upscaler | Starts on the first Super Resolution or Ray Reconstruction frame, then needs one more frame to bring the model up. The page shows what was last skipped and why. |
| Starting... | Loading the model. The game may hitch once. |
| Running | Shows where it runs, the model size, the number of passes, whether the frame is linear HDR or tone-mapped, the cost in ms, and whether the game supplies an exposure value. |
| Failed for this session | The reason is shown. Fix it, then press **Retry**. |

## Settings

All settings are in the `[DlssNr]` section of `jmod\jmod.ini`. Settings in **Tuning** apply when you release the control, and rebuild the model after about 30 frames. All other settings apply live.

### Basics

| Key | Default | Values | What it does |
|---|---|---|---|
| `Enabled` | `0` | `0`, `1` | Turns Neural Rendering on. Overlay: **Enable Neural Rendering**. |
| `ToggleHotkey` | `0` | a key code | A key that toggles `Enabled` in the game (for example `118` = F7). `0` = none. Bind it on the Neural Rendering or Controls page. |
| `RunBeforeSR` | `0` | `0`, `1` | `0` = edit the upscaler's output. `1` = edit the colour input before Super Resolution (cheaper). An input that cannot be used falls back to after the upscale. Overlay: **Run before Super Resolution**. |
| `Passes` | `1` | `1` to `3` (`1` to `30` with `UnlockPasses=1`) | Sequential model layers. Each extra pass costs about one more model run, and 2 and 3 are deliberately over-processed. Start with `1`. |
| `UnlockPasses` | `0` | `0`, `1` | `1` = allow more than 3 passes. Overlay: **Unlock more than 3 passes**. |
| `WorkingScale` | `1` | `0.25` to `2` | The model's size relative to the frame. Below `1` is cheaper (the cost falls with the square), above `1` supersamples. The frame itself is never reduced. |

### Tuning

| Key | Default | Values | What it does |
|---|---|---|---|
| `Preset` | `0` | `0` to `3` | The model preset. |
| `Style` | `0` | `0` to `2` | `0` = Standard, `1` = Natural, `2` = Cinematic. |
| `Intensity` | `1` | `0` to `2` | How strong the model's effect is. |
| `LocalStructure` | `1` | `0` to `2` | Strength of local structure (detail). |
| `LocalTone` | `1` | `0` to `2` | Strength of local tone. |
| `SkinStructure` | `-1` | `-1` to `2` | Structure strength on skin. `-1` = follow `LocalStructure`. |
| `AutoMask` | `1` | `0`, `1` | Automatic skin mask. |

With more than one pass, each pass from 2 up follows pass 1 unless you override it (local tone follows `0`). The overlay shows a **Pass N** group for each. In the ini, the keys are `Pass<N>Preset`, `Pass<N>Style`, `Pass<N>Intensity`, `Pass<N>LocalStructure`, `Pass<N>LocalTone`, `Pass<N>SkinStructure` and `Pass<N>AutoMask`, for N = 2 to 30. Leave a key out to follow pass 1.

### Composition

| Key | Default | Values | What it does |
|---|---|---|---|
| `TransferStrength` | `1` | `0` to `2` | **Detail transfer.** How far the frame moves toward the model's picture. `0` gives back exactly what the upscaler produced, `1` is the model's picture, and above `1` goes past it. |
| `ColourStrength` | `1` | `0` to `2` | **Colour transfer.** `0` keeps the game's own colour and takes only brightness from the model. `1` brings the model's colour too. Above `1` over-saturates. |
| `MaxRatio` | `2` | `1` to `8` | **Brightening guard.** The most a pixel may be brightened, as a multiple of what it was. Darkening is not capped. |
| `Transfer` | `1` | `0`, `1` | **Resize transfer** (shown when `WorkingScale` is not `1`): `0` = Classic, `1` = Matched residual. |
| `ProxyMode` | `0` | `0` to `4` | **Encode curve.** `0` = Soft knee (default). Change it only if you need to. |
| `WhitePointSource` | `1` | `0`, `1` | **White point source.** `0` = paper white only. `1` = the game's own exposure, when it supplies one. Applies only to a linear HDR frame. A tone-mapped frame is left untouched. |
| `WhitePointScale` | `1` | `0.01` to `4096` | **Paper white.** |
| `WhitePointTrim` | `1` | `0.25` to `4` | **Trim:** a multiplier on the game's exposure (when `WhitePointSource=1`). |

### Skin filter

A colour-based filter on the final edit. Warm scenery may match and coloured lighting can hide skin, so preview before relying on it.

| Key | Default | Values | What it does |
|---|---|---|---|
| `SkinProtection` | `0` | `0`, `1` | Turns the filter on. |
| `SkinDetail` | `1` | `0` to `1` | Detail kept on skin. |
| `SkinToneEnabled` | `1` | `0`, `1` | Allows a colour change on skin. |
| `SkinColour` | `1` | `0` to `1` | Colour change on skin. |
| `EnvironmentDetail` | `1` | `0` to `1` | Detail kept outside skin. |
| `EnvironmentColour` | `1` | `0` to `1` | Colour change outside skin. |
| `ShowSkinMask` | `0` | `0`, `1` | Shows the mask, for checking. |

### Debugging

| Key | Default | Values | What it does |
|---|---|---|---|
| `DebugView` | `0` | `0` to `3` | `0` = off, `1` = the picture the model sees, `2` = the model's raw answer, `3` = what it changed (x20). |
| `Compare` | `0` | `0` to `2` | `0` = off, `1` = side by side, `2` = wipe. |
| `CompareSplit` | `0.5` | `0` to `1` | Wipe position. |
| `CompareZoom` | `1` | `1` to `4` | Zoom for the side-by-side view. |
| `CompareSwap` | `0` | `0`, `1` | Swaps the two sides. |
| `ApplyModel` | `1` | `0`, `1` | `0` outputs the clean upscaler frame while the pass keeps running, for A/B comparison. |
| `HoldFrame` | `0` | `0`, `1` | Freezes the model's input so a setting change re-renders the same frame. |

## Presets

A preset keeps the page's look: tuning, passes, overrides, composition, white point and the skin filter. It does not keep the on switch, the caller check or the debugging views. On the page, type a name and press **Save** (or **Overwrite**). Each preset is a file, `jmod\dlssnr\presets\<name>.ini`. Pick a saved preset from the list to load it, or delete it.

## Custom models

If you have your own model files, put each in its own folder under `jmod\dlssnr\models`. While that folder exists, the Model section shows a **Model** choice (for pass 1 and for each pass) and a status line. Without the folder there is no such choice. If a model does not match, the frame is left as the upscaler made it and the page says why.

## Hotkeys

- **Toggle Neural Rendering** (`ToggleHotkey`): bind it on the Neural Rendering or Controls page. It works while the game window has focus.

## Needs

- `nvngx_dlssnr.dll` in `jmod\streamline`. It is not included: you supply it.
- A game that uses DLSS Super Resolution or Ray Reconstruction (its own, or through [sr-inject](sr-inject.md)).
- In a DirectX 11 game: `D3D11Bridge=1` ([dx11-bridge](dx11-bridge.md)). In a Vulkan game: `VulkanBridge=1` ([vk-bridge](vk-bridge.md)).

## Known limits

- Each extra pass costs about one more model run. Start with one pass.
- The cost depends on `WorkingScale`.
- A tone-mapped frame gets no colour transform from the white point settings.
- The Tuning settings rebuild the model, so the picture may change for a moment after you release a control.
