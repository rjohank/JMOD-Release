# Installing JMOD

## What is in the zip

Extract `JMOD-MFG-RTX40-v<version>.zip` into the folder that holds the game's `.exe`:

| File | What it is |
|---|---|
| `winmm.dll` | The core. It loads every feature, the overlay and the hooks. |
| `jmod\modules\*.jmod` | One file per feature, including `kernels.jmod`. Keep the whole folder together. |
| `jmod\xefg\libxess_fg.dll` | Intel's XeSS Frame Generation library (version 1.3.4.95). |

There is no `jmod.ini` in the zip. JMOD writes it on first start, so updates keep your settings. The log is `jmod\jmod.log`.

`winmm.dll` also works renamed to `version.dll` or `dxgi.dll`. Use one of those instead if the game already ships its own `winmm.dll`, or another mod uses that name. Use only one of the three.

## Updating

Close the game, then replace `winmm.dll` (or the name you renamed it to) and the whole `jmod\modules` folder together. Your `jmod\jmod.ini` and `jmod\streamline` folder are kept.

## Files you supply

NVIDIA's files are not included. When a feature needs them, put them in `jmod\streamline` (create the folder if it does not exist):

| Feature | Files |
|---|---|
| Frame generation in games that do not have it | `sl.interposer.dll`, `sl.common.dll`, `sl.dlss_g.dll`, `sl.reflex.dll`, `sl.pcl.dll`, `nvngx_dlssg.dll` |
| DLSS in DirectX 11 games without DLSS | `nvngx_dlss.dll` (plus the files above for frame generation) |
| DLSS Neural Rendering | `nvngx_dlssnr.dll` (see [neural-rendering.md](neural-rendering.md)) |

Take them from an NVIDIA Streamline SDK release, or from a game that ships them.

## DirectX 11 games

Frame generation and DLSS Neural Rendering also work in DirectX 11 games. Turn on the DirectX 11 support in `jmod\jmod.ini`, then restart the game:

```ini
[dx11-bridge]
D3D11Bridge=1
```

- For frame generation, also set `InjectFrameGen=1` in `[fg-inject]` and put NVIDIA's Streamline files in `jmod\streamline`. See [fg-inject](modules/fg-inject.md).
- For a game that ships no DLSS, turn on `SrInject=1` in `[sr-inject]` and put `nvngx_dlss.dll` in `jmod\streamline`. See [sr-inject](modules/sr-inject.md).
- For Neural Rendering, put `nvngx_dlssnr.dll` in `jmod\streamline` ([neural-rendering.md](neural-rendering.md)).

Start the game once first, so that JMOD writes `jmod\jmod.ini`. See [dx11-bridge](modules/dx11-bridge.md).

## Vulkan games

JMOD can run XeSS Frame Generation (with its HUD options) and DLSS Neural Rendering in a Vulkan game that uses DLSS Frame Generation through Streamline. NVIDIA GPUs only.

1. Keep the whole `jmod\modules` folder as it came in the zip.
2. Turn the Vulkan support on in `jmod\jmod.ini`, then restart the game:

   ```ini
   [vk-bridge]
   VulkanBridge=1
   ```

3. Neural Rendering needs `nvngx_dlssnr.dll` in `jmod\streamline`, as in a DirectX 12 game ([neural-rendering.md](neural-rendering.md)).

If you have not started the game once with JMOD, there is no `jmod.ini` yet. Start the game once and close it, then edit the file.

The overlay's Diagnostic page (System > Diagnostic) has a "Vulkan bridge" section. It shows whether the support is ready ("Vulkan bridge: ready"), the graphics device it uses and how many shared textures are in use.

If it cannot work (the setting is off, the graphics driver lacks something it needs, or a frame cannot be handled), the game keeps running on its own path: DLSS Frame Generation stays NVIDIA's, Neural Rendering stays off, and `jmod.log` has one line saying why. DirectX 11 and DirectX 12 games are not affected by this setting.

## Safe mode, and turning modules off

- `jmod\modules.ini`: write `[Modules]`, then `disabled=window,dlssnr`, to keep the named modules from loading at all. The overlay's Modules page edits the same list.
- Safe mode loads no module: create an empty file `jmod\safe_mode`, or hold **Shift** while the game starts. Delete the file to leave safe mode. Use it when a game crashes at start-up, then turn modules off one by one with `disabled=`.
- A module that fails to load is switched off and listed with the reason. The others keep running.

## Uninstall

Close the game, then delete `winmm.dll` (or the `version.dll` / `dxgi.dll` you used) and the `jmod` folder. Nothing else is written, and no game file is modified.
