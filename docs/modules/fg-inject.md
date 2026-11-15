# Frame generation for games without it (`fg-inject`)

## What it does

This module adds DLSS Frame Generation to a DirectX 12 game that uses DLSS Super Resolution but ships no frame generation. It can also swap in your own NVIDIA Streamline files for a game that already uses Streamline. The frame generation then works like in any other game: you pick the multiplier and the method on the [framegen](framegen.md) page, and the HUD is handled by the [ui-layer](ui-layer.md) module.

DirectX 11 games that ship no DLSS at all are covered by the [sr-inject](sr-inject.md) module, which feeds this one.

NVIDIA's files are not included. You supply them (see Needs).

## Where to find it

- Overlay, **Frame Generation** page, **Injection** section. The section header shows "hosting on", "hosting off" or "restart to apply".
- Overlay, **Home** page: a notice "Frame generation hosting was changed" when a change waits for a restart.

Changes in the Injection section apply live, except the hosting switch. Use **Save to ini** (bottom of the sidebar) to keep them for this game.

## Settings

All settings are in the `[fg-inject]` section of `jmod\jmod.ini`. Most are also in the Injection section.

### Switches

| Key | Default | Values | What it does | Applies |
|---|---|---|---|---|
| `InjectFrameGen` | `0` | `0`, `1` | `1` = add frame generation to a game that ships none. In the overlay: **Host Streamline for a game without it**. DLSS must be the game's upscaler. | Restart the game |
| `InjectGameStreamline` | `2` | `0` to `2` | What to do in a game that already runs Streamline. `0` = leave it alone. `1` = join: the game keeps its own Streamline and frame generation is added to it. `2` = replace: the Streamline and NGX files in `jmod\streamline` are used instead of the game's own. Anything the folder lacks still comes from the game, and a game that has its own frame generation keeps driving it. Works also for a game that loads NGX directly. | Restart the game |
| `InjectStreamlineLog` | `1` | `0` to `2` | Streamline's own logging. `0` = the game's logging untouched. `1` = warnings and errors in `jmod.log`. `2` = every message in `jmod.log` (large; for diagnosing frame generation input problems). | Restart the game |

### Camera

A game without Streamline gives frame generation no camera, so one is built from these values. A game with its own Streamline sends its own camera. If the generated frames look wrong, tune these.

| Key | Default | Values | What it does |
|---|---|---|---|
| `InjectCameraFovY` | `60` | above `0` | Vertical field of view, in degrees. |
| `InjectCameraFovX` | `0` | `0` or above | Horizontal field of view, in degrees, for a game that states that one. A value above `0` replaces `InjectCameraFovY`. `0` = off. |
| `InjectCameraNear` | `0.1` | above `0` | Near plane, in game units. |
| `InjectCameraFar` | `100000` | above `0` | Far plane, in game units. |
| `InjectCameraInfiniteFar` | `0` | `0`, `1` | `1` = the far plane is infinite. Overrides `InjectCameraFar`. |
| `InjectCameraAuto` | `1` | `0`, `1` | `1` = take the field of view, near and far plane from the game when it provides them. |
| `InjectCameraLeftHanded` | `0` | `0`, `1` | `1` = left-handed camera, as in Unity and DirectX-style engines. `0` = right-handed. |
| `InjectCameraSwapPlanes` | `-1` | `-1`, `0`, `1` | Swap near and far for inverted depth. `-1` = automatic, `0` = never, `1` = always. Use `1` for a game whose generated frames are garbled or inverted but look normal when standing still. |

The overlay's **Camera** section shows the same options. The log line "injection camera" shows what was used.

### Depth and motion vectors

| Key | Default | Values | What it does |
|---|---|---|---|
| `InjectDepthState` | `64` | a number | The state the game leaves its depth buffer in when it runs DLSS, as a number: `8` = unordered access, `64` = non-pixel shader resource, `128` = pixel shader resource, `192` = both shader-resource bits, `4` = render target, `16` = depth write, `32` = depth read. Add the values for a combined state. A wrong state shows as flicker or graphics-layer errors. |
| `InjectMotionState` | `64` | a number | The same for the motion vectors. |
| `InjectMotionScaleX` | `1` | any number except `0` | Multiplied into the motion vector scale in X. A negative value flips the sign. Use it when generated frames move against the camera or the objects. |
| `InjectMotionScaleY` | `1` | any number except `0` | The same in Y. |

The overlay shows these under **Depth and motion vectors**, with a **Flip both vector signs** button.

## Hotkeys

None.

## Needs

- A DirectX 12 game that uses DLSS Super Resolution as its upscaler. A game that runs Streamline itself is handled by `InjectGameStreamline`.
- NVIDIA's Streamline release files in `jmod\streamline` (create the folder next to the game `.exe`): `sl.interposer.dll`, `sl.common.dll`, `sl.dlss_g.dll`, `sl.reflex.dll`, `sl.pcl.dll` and `nvngx_dlssg.dll`. Take them from an NVIDIA Streamline SDK release, or from a game that ships them.
- For `InjectGameStreamline=1`: `sl.dlss_g.dll` must be the same Streamline major version as the game's own.
- Frame generation added to a game that has Streamline uses the game's own depth, motion vectors and Reflex markers, so the game must use Streamline Reflex.
- The [framegen](framegen.md) and ngx modules.

## Known limits

- The hosting switch and `InjectGameStreamline` apply only when the game starts.
- Frame generation added this way depends on the game giving DLSS good depth and motion vectors. If the generated frames look wrong, check the camera and the depth and motion vector settings above.
- If `InjectGameStreamline=2` cannot take over a game's Streamline files (for example because of a version mismatch), the game keeps its own, and `jmod.log` says what was kept and why.
