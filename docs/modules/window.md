# Borderless window (`window`)

## What it does

This module removes the game window's frame and places the window on its monitor: centered, snapped to an edge or a corner, sized to the whole monitor, or sized and offset by hand. It changes only the window. Whether the game resizes its picture to the new window is up to the game.

It is off by default. It is not for exclusive fullscreen. Turning it off restores the window as the game had it.

## Where to find it

- Overlay, **Window** page. Use **Reset to defaults** for the page to go back to the default settings.
- Overlay, **Home** page: a **Borderless window** switch.

## Settings

All settings are in the `[window]` section of `jmod\jmod.ini`. They apply live.

| Key | Default | Values | What it does |
|---|---|---|---|
| `Borderless` | `0` | `0`, `1` | `1` = remove the window frame and place the window from the settings below. In the overlay: **Borderless window**. |
| `WindowSnap` | `0` | `0` to `8` | Where the window sits on its monitor. `0` = center, `1` = left, `2` = right, `3` = top, `4` = bottom, `5` = top-left, `6` = top-right, `7` = bottom-left, `8` = bottom-right. Snapping moves the window only. It keeps the size the game gave it. |
| `WindowFill` | `0` | `0`, `1` | `1` = size the window to the whole monitor. `WindowWidth` and `WindowHeight` are then ignored, and the snap makes no visible difference. In the overlay: **Fill monitor**. |
| `WindowWidth` | `0` | `0` or above | Window width in pixels. `0` = the game's own width. The window stays anchored to the snap position at the new size. |
| `WindowHeight` | `0` | `0` or above | Window height in pixels. `0` = the game's own height. |
| `WindowOffsetX` | `0` | any number | Offset from the snapped position, in pixels. A negative value moves the window left. |
| `WindowOffsetY` | `0` | any number | The same, vertically. A negative value moves the window up. |

In the overlay, **Ctrl+click** a value to type it. **Reset size and offset** sets the width, height and offsets back to `0`.

## Hotkeys

None.

## Needs

- A windowed or borderless game. It does not work with exclusive fullscreen.

## Known limits

- Only the window changes. A game that does not resize its render buffers to the new window size shows a stretched or cropped picture.
- If the game has its own window mode setting, set it to windowed first.
