# DLSS Neural Rendering

JMOD can run NVIDIA's DLSS Neural Rendering on top of the image the game's own DLSS Super Resolution or Ray Reconstruction produces. It works in DirectX 11, DirectX 12 and Vulkan games.

## Install

Put `nvngx_dlssnr.dll` in the `jmod\streamline` folder next to the game `.exe` (create it if it does not exist). This file is **not included** in JMOD: you supply it.

## Turn it on

Open the overlay and go to the **Neural Rendering** page, then switch it on. Or set it in `jmod\jmod.ini`:

```ini
[DlssNr]
Enabled=1
RunBeforeSR=1
Passes=1
```

| Setting | What it does |
|---|---|
| `Enabled` | `1` turns Neural Rendering on, `0` turns it off. |
| `RunBeforeSR` | `1` runs it before Super Resolution, `0` runs it after. |
| `Passes` | How many passes it runs. Start with `1`. |

## Hotkey

You can toggle `Enabled` while you play. Bind a key on the Neural Rendering page (or on the Controls page, under "Neural Rendering"), then use "Save to ini" to keep it. The hotkey works while the game window has focus.

## Status box and Retry

The page has a status box. If Neural Rendering is not running, it says why: for example a missing file, or a game that supplies no usable depth or motion data. After you fix the cause, press **Retry** to try again without restarting the game.
