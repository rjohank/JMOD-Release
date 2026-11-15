# UI Layer (`ui-layer`)

## What it does

Frame generation works on the finished picture, so a HUD drawn on top of it can smear or distort in the generated frames. The UI Layer gives frame generation a separate view of the HUD, so the HUD stays sharp. It works with DLSS-G and with the XeSS methods of the [xefg](xefg.md) module. It handles the HUD in the way that fits the game:

- **The game provides its own UI layer:** it is passed through as it is.
- **The game provides a HUD-less image:** the UI Layer builds the HUD layer from it.
- **The game provides neither:** **HUD assist** finds a HUD-less image for the game.

With the default mode, Automatic, JMOD picks the best of these for each game. It also hands a HUD-less image to HUD interpolation in [xefg](xefg.md).

## Where to find it

- Overlay, **Frame Generation** page, in the **UI layer** section. A short Mode choice is also on the **Home** page.
- The **Status** part shows the state of the check ("Gate") and whether HUD assist is on.
- Under **Advanced**: the HUD detection strictness, HUD assist, and the debug views.
- Overlay, **Debug** page, **UI layer tools**: the debug views and tools, with no key binding needed.

## Settings

All settings are in the `[ui-layer]` section of `jmod\jmod.ini`.

| Key | Default | Values | What it does | Applies |
|---|---|---|---|---|
| `UiLayer` | `1` | `0`, `1` | The main switch. `1` = automatic: the best source for the HUD is chosen for each game, and HUD assist is used when the game offers nothing usable. | HUD assist may need a restart of the game |
| `UiLayerFromHudless` | `0` | `0`, `1` | Builds the HUD layer from the game's own HUD-less image. Automatic mode turns it on by itself. | Live |
| `HudAssist` | `0` | `0`, `1` | HUD assist: for games without a usable HUD source. Automatic mode turns it on when needed. With `UiLayer=0` this key alone decides. | May need a restart of the game |
| `UiLayerDeadband` | `0` | `0` to `8` | How strict the HUD detection is. `0` = exact, right for most games. Raise it to `1` when the whole frame is detected as HUD. DirectX 12 only. | Live, from the next frame |
| `HudAssistDraw` | `0` | `0` to `64` | Manual step for HUD assist. `0` = automatic. If the debug views show the HUD handled wrongly, try values from `1` upward and keep the smallest one that handles the whole HUD. DirectX 12 only. | Live |

In the overlay, the three switches are shown as one **Mode** choice:

| Mode | What it does |
|---|---|
| Automatic (recommended) | Picks the best HUD handling for the game. |
| Game-provided only | Uses only what the game provides. HUD assist is never used. |
| Always assist | HUD assist is used whenever it applies. |
| Off | Leaves the game's UI options alone. |

If the ini holds another combination of the three keys, the overlay shows **Custom**.

### Debug views and tools

These help to check why a HUD looks wrong, and for bug reports.

| Key | Default | Values | What it does |
|---|---|---|---|
| `UiDetectView` | `0` | `0`, `1` | Start with the detected-UI view on. It is read from the ini and never saved. |
| `UiDetectMode` | `0` | `0` to `2` | What the view paints: `0` = tint by coverage, `1` = tint by alpha, `2` = the layer only. |
| `UiDetectMinAlpha` | `0` | `0` to `255` | The alpha a pixel needs to count in the view. `0` = any alpha above zero. |
| `UiDetectColor` | `FF66CC` | `RRGGBB` | The paint colour, in hex. |
| `UiDetectStrength` | `60` | `0` to `100` | How strongly the tint replaces the picture, in percent (modes `0` and `1`). |
| `UiLayerDiagnostics` | `0` | `0`, `1` | Extra checks and more detailed log lines. Use it on a new game. `0` logs one line with the verdict. |

The overlay has the same controls under **Advanced**, **Debugging**: **Show detected UI**, Mode, Colour, Strength, Minimum UI alpha, and **UI layer diagnostics (verbose logging)**. **Show detected UI** paints the detected HUD over the game image for checking. Generated frames show the same pixels where that is supported. The **HUD-less view** hides the HUD instead. The two views replace each other.

On the Debug page, **UI layer tools** also has **Arm trace (next 3 frames)** and **Take difference snapshot**, for bug reports.

## Hotkeys

The debug hotkeys are not bound by default. They need `DebugHotkeys=1` in `[core]`, and a restart after you change it. Each is a decimal key code (for example `119` = F8). `0` = none. You can also set them on the Controls page.

| Key | What it does |
|---|---|
| `HudlessViewHotkey` | Toggles the HUD-less view. |
| `UiDetectViewHotkey` | Toggles the detected-UI view (for example `121` = F10). |
| `HudAssistDrawNextHotkey` / `HudAssistDrawPrevHotkey` | Step `HudAssistDraw` up or down by 1. |
| `FrameFlowTraceHotkey` | Logs the next 3 frames. |
| `DiffSnapshotHotkey` | Writes three comparison images and a summary to the log. |

## Needs

- A DirectX 12 or Vulkan game that uses DLSS Frame Generation through NVIDIA Streamline, and the [framegen](framegen.md) module. Without framegen, no UI layer is built.
- In a game that runs neither DirectX 12 nor Vulkan, the page shows "Inactive". The settings are kept but not used.

## Known limits

- The detection strictness, the HUD assist step and the HUD assist rules are DirectX 12 only. In Vulkan games they are greyed out, and Vulkan games use their own handling.
- Turning HUD assist on can need a restart of the game. The page shows "Restart the game to apply" when it does.
- While a debug view is on, generated frames show the debug picture, and the UI layer is not handed to frame generation for those frames. HUD interpolation falls back to the game's own HUD-less image.
- A game whose HUD-less image is rejected is checked again after about 30 frames.
