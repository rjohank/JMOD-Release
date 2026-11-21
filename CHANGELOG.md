# Changelog

## 1.6.0.0

### Added
- **Sharpening (new `sharpen` module):** a small neural network sharpens the image right after DLSS, on every frame DLSS upscales, before frame generation reads it. Only brightness is sharpened; colour stays. Two models (standard and edge-enhanced), strength 0 to 2, edge response, optional extra strength on moving pixels, and two debug maps. Off by default. Turn it on from the **Sharpening** page (`[sharpen] Sharpen=1`); every setting applies at once. Works in DirectX 12, in DirectX 11 with `D3D11Bridge=1` and in Vulkan with `VulkanBridge=1`.
- **Overlay styles:** `OverlayStyle` Classic, Modern (new default) or Compact, with animated switches, sidebar icons and softer surfaces. **Follow Windows** takes light/dark mode and the accent colour from Windows.
- **Notifications:** short click-through notices in a corner of the screen for hotkey changes (multiplier, VSync, Neural Rendering toggle, on-screen display preset, Interpolation A/B) and for the first-launch hint. `Notifications`, `NotificationPosition` and `NotificationDuration` in `[core]`, on the Controls page. They are hidden while the panel is open.
- **Performance page:** the game's frame pipeline (CPU and GPU busy, frame times, GPU time per pass) from the driver's latency report, in DirectX 11, DirectX 12 and Vulkan. Nothing runs while the page is closed.
- **PC latency (experimental)** on the Performance page and as a row in the on-screen display (`OsdPcLatency`). DirectX only.
- **More system DLL names:** the core also runs as `d3d12.dll`, `dbghelp.dll` or `dinput8.dll`, for games where `winmm.dll`, `version.dll` and `dxgi.dll` are taken.
- **XeFG HUD interpolation, Efficient option:** with the Quality or Performance preset, only the parts of the HUD that moved are processed. It lowers the cost of those presets.
- **Neural Rendering:** saved presets on the page (a look is kept as `jmod\dlssnr\presets\<name>.ini`). Custom models: supply your own model files in `jmod\dlssnr\models`; the page shows a **Model** choice only when that folder exists.

### Changed
- **XeFG HUD choice:** the two HUD checkboxes are one choice, Off / Composition / Interpolate. The preset shows under Interpolate. The ini keys are unchanged.
- **Frame Generation page** regrouped: a card for the multiplier and method, a panel for the chosen method's options, and a "For every method" section with live summaries.
- **A module whose required module failed to start is skipped** with the reason in the log, and a module whose start failed no longer leaves its services behind.
- Less per-frame work in several modules; unused state removed.

### Fixed
- **XeFG: game hang with HUD interpolation** in games that run frame generation on a compute queue (a hang when the HUD appeared on a scene change or a letter opened). The HUD step now runs on every frame pair.
- **XeFG: shader pipelines created from a stream** were not recorded because of a wrong table index.
- **Borderless window:** a game that keeps its own window size no longer fights the layout. While the mode is on, the layout holds even if the game restyles or moves its window.

### Known limits
- **Sharpening** works on frames DLSS upscales. It does nothing in a game with no DLSS unless Injection is on.
- **PC latency** is experimental and needs a game that reports latency markers.

### Upgrade notes
- No required settings changes. Sharpening is off until you turn it on. The overlay opens in the new Modern style; set `OverlayStyle=0` for the old look.
- Replace `winmm.dll` (or the name you use) and the whole `jmod\modules` folder together. `sharpen.jmod` is new.

## 1.5.2.0

### Added
- **`[core] SkipProcesses` setting:** list other programs (comma-separated, for example `SkipProcesses=Reporter.exe,Updater.exe`) that should not load JMOD. Known crash reporters and helper programs are recognized already. Read once at launch.

### Changed
- **One Diagnostic page:** the overlay's Status and Diagnostics pages are now one page, **System > Diagnostic**, with a collapsible section per category. The bridge sections start collapsed.
- **Less work while a feature is off:** DLSS injection, Neural Rendering and the other optional features no longer do work every frame while they are turned off.

### Fixed
- **XeFG: the game could lose its graphics device and close** with UI Composition or HUD Interpolation on, when it changed the format of its HUD-less image (for example when leaving a dialogue or a menu).
- **Crash reporters and other helper programs a game starts from its folder** no longer run a second copy of JMOD, which doubled log lines and could clear the game's `jmod.log`.

### Known limits
- A HUD-less image that differs too much from the game image is not used; HUD interpolation then works without it.
- Vulkan support needs a game that uses DLSS Frame Generation, and NVIDIA GPUs. A change to `VulkanBridge` needs a restart.

### Upgrade notes
- No required settings changes.
- The overlay's Status page is now part of **System > Diagnostic**.
- Replace `winmm.dll` and the `jmod\modules` folder together.

## 1.4.0.0

### Added
- **XeSS Frame Generation in Vulkan games:** XeMFG and XeMFG XMX (up to 6x), HUD interpolation with its presets, and live switching between XeMFG, XeMFG XMX and DLSS Frame Generation while the game runs. The HUD interpolation setup runs in the background on first use, so it starts a moment after frame generation.
- **DLSS Neural Rendering in Vulkan games**, before and after Super Resolution, with the on/off toggle.
- **UI Layer in Vulkan games:** the HUD-less image is handed to frame generation for HUD interpolation, and the "Show detected UI" and HUD-less debug views now work in Vulkan games.
- **Vulkan support setting:** `[vk-bridge] VulkanBridge=1` (default 0), then restart the game. It works for Vulkan games that use DLSS Frame Generation through Streamline, on NVIDIA GPUs. If it cannot work, the game keeps running on its own path and `jmod.log` has one line saying why. The Status page of the overlay has a "Vulkan bridge" section. DirectX 11 and DirectX 12 games are not affected by the setting.

### Changed
- XeFG accepts a HUD-less image in another colour format, after a quick content check. An image that differs too much from the back buffer is not used.
- Frame generation live switching also works in Vulkan games. A switch that cannot start logs one line per reason.
- Performance graphs measure each graphics API separately.
- Less idle work while DLSS injection is off.

### Known limits
- Vulkan support needs a game that uses DLSS Frame Generation, and NVIDIA GPUs. A change to `VulkanBridge` needs a restart.
- Vulkan support uses extra VRAM (freed when idle) and a small per-frame cost while a feature uses it.
- The HUD-less debug view on Vulkan shows only once the UI Layer is active for the game.
- The overlay runs in layered mode in Vulkan games.

### Upgrade notes
- No required settings changes. Vulkan support does nothing unless you set `VulkanBridge=1`.
- Replace `winmm.dll` and the `jmod\modules` folder together.

## 1.3.0.0

### Added
- **DLSS for DirectX 11 games that ship without DLSS** (DLAA, at native resolution). Frame generation and Neural Rendering then work in those games too. Turn on **Injection** on the Upscaler page of the overlay, and restart the game. Needs `nvngx_dlss.dll` in `jmod\streamline`, and for frame generation NVIDIA's Streamline files in the same folder. Use it only in games without their own DLSS: if the game starts its own DLSS, JMOD steps aside.
- The depth buffer is found automatically. Motion vectors are built from the image, with a camera model for static scenery. The field of view is measured and remembered for the next launch.
- `LogLevel=4` for debug output. The default log is now quieter.
- A **Retry** button for the XeFG HUD preset when it could not start.

### Fixed
- A hang and error storm after resizing the window with the overlay open.
- Rare hangs and HUD preset fallbacks while the game was starting up.
- The overlay's bottom strip flickering when the overlay runs off-screen.

### Changed
- XeFG uses less CPU, and its first flow layer runs on tensor cores.

### Known limits
- DLSS injection is DirectX 11 only.
- Very fast camera turns can show more ghosting than native DLSS, because the motion comes from the image itself.

### Upgrade notes
- No required settings changes. DLSS injection is off unless you turn it on in a game without DLSS.
- If you rely on the periodic statistics in `jmod.log`, set `LogLevel=4`.
- Replace `winmm.dll` and the `jmod\modules` folder together.
