# Changelog

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
