# DLSS for games without DLSS (`sr-inject`)

## What it does

This module runs DLSS (DLAA, at native resolution) in a DirectX 11 game that ships no DLSS. JMOD finds the game's depth buffer itself and builds motion vectors from the image, with a camera model for static scenery. DLAA gives anti-aliasing on its own, with no frame rate gain. It also lets frame generation ([fg-inject](fg-inject.md)) and DLSS Neural Rendering ([neural-rendering.md](../neural-rendering.md)) run in a game that has no DLSS.

It is off by default. Turn it on per game, and only for a game **without** its own DLSS. If the game starts its own DLSS anyway, JMOD steps aside.

## Where to find it

- Overlay, **Upscaler** page, **Injection: DLSS for games without DLSS** section. It has the on/off switch, the state, the number of frames processed, the DLSS preset in use, and the options below. The preset is the Super Resolution preset of the [presets](presets.md) module.
- Overlay, **Diagnostics** page, **Upscaler**, **Injection**: timings, statistics, and the state of each stage (depth, motion vectors, DLSS).
- Overlay, **Status**: the state of the injection.

**Debug views**, on the Diagnostic page (none is saved):

| View | Shows |
|---|---|
| Show the chosen depth | The depth over the game image, in grey. Near is bright. |
| Show motion vectors | The vectors over the game image. The hue is the direction, the brightness the length, dark grey is no motion. |
| Show camera model classes | Needs the camera model on. Green: follows the camera model. Yellow: still on screen, and the model explains it as well as zero motion. Red: moving, keeps its own motion. Cyan: unreliable in the background. |

## Settings

All settings are in the `[sr-inject]` section of `jmod\jmod.ini`. The overlay shows most of them.

| Key | Default | Values | What it does | Applies |
|---|---|---|---|---|
| `SrInject` | `0` | `0`, `1` | `1` = run DLSS in a game that has none. In the overlay: **Run DLSS (DLAA) in a game that has none**. Use it only for a game without native DLSS. | Restart the game (**Save to ini**, then restart) |
| `SrInjectMvClean` | `1` | `0` to `2` | **Vector cleanup.** Frame generation warps with the motion vectors, so a wrong one smears. `0` = raw vectors. `1` = check, dilate and smooth: vectors that fail a consistency check are dropped, edges of moving objects take the object's motion, and vectors are smoothed inside one depth layer. `2` = the same without the check. Screen-static pixels such as the HUD are kept at zero motion. | Live |
| `SrInjectMvTolerance` | `2` | `0.5` to `8` | How strict the check is, in pixels. A lower value drops more vectors. | Live |
| `SrInjectCameraModel` | `2` | `0` to `2` | **Camera motion model.** `0` = off (the cleaned vectors). `1` = camera rotation. `2` = rotation and translation, using the depth (falls back to `1` without depth). Static scenery gets exact camera vectors, and moving objects keep their own. A frame the model explains poorly keeps the cleaned vectors. | Live |
| `SrInjectCameraFov` | `0` | `0` to `170` | Vertical field of view in degrees, for camera model `2`. `0` = measure it automatically. The measured value is remembered for the next launch. The overlay slider goes to 120. | Live |
| `SrInjectFlowPerf` | `2` | `0` to `2` | **Flow quality** of the motion estimation: `0` = slow, `1` = medium, `2` = fast. | Live |
| `SrInjectFlowGrid` | `2` | `1`, `2`, `4` | **Flow grid:** pixels per vector. A coarser grid costs less. `4` is the only one supported on RTX 20 series GPUs. An unsupported value falls back to the next one that works. | Live |

## Hotkeys

None.

## Needs

- A DirectX 11 game **without** its own DLSS. Other graphics APIs are not supported yet.
- NVIDIA's `nvngx_dlss.dll` in `jmod\streamline` (create the folder next to the game `.exe`). It is not included.
- For frame generation in such a game, also:
  - NVIDIA's Streamline files in `jmod\streamline`, and `InjectFrameGen=1` in `[fg-inject]` (see [fg-inject](fg-inject.md));
  - `D3D11Bridge=1` in `[dx11-bridge]`, which presents the game through JMOD's own swap chain;
  - the UI Layer Mode set to **Off** on the Frame Generation page (`UiLayer=0`): the HUD is part of the image in these games.
- For Neural Rendering in such a game: `D3D11Bridge=1` and `nvngx_dlssnr.dll` ([neural-rendering.md](../neural-rendering.md)).

The overlay's Injection section shows whether `InjectFrameGen` and `D3D11Bridge` are on. All of these are read at launch.

## Known limits

- DirectX 11 only.
- Very fast camera turns can show more ghosting than native DLSS, because the motion comes from the image itself.
- The motion estimation costs some CPU while the camera moves.
- DLAA alone gives no frame rate gain.
