# Frame Generation (`framegen`)

## What it does

This module controls how the game's DLSS Frame Generation runs. It lets you:

- force the **multiplier** (2x to 6x), or turn frame generation **off**, while you play, without touching the game's own settings;
- choose the **method** that makes the generated frames: DLSS-G (NVIDIA's own) or one of the XeSS methods from the [xefg](xefg.md) module;
- use NVIDIA's **Dynamic MFG**, which picks the number of generated frames every frame to reach a target frame rate;
- override **VSync** and the **Reflex** mode, and cap the frame rate through Reflex;
- pick which **Streamline runtime** the game loads.

A change from the overlay or from a hotkey applies live, within a frame: JMOD re-sends the game's last frame generation request with your choice. The game does not notice.

## Where to find it

- Overlay, **Frame Generation** page. It has a card with the multiplier and the method choice, a panel with the chosen method's own options, and a "For every method" part with live summaries (Dynamic MFG, VSync and Reflex pacing, plugin workarounds, Streamline runtime). The **Frame generation preset** section shows when the presets module is loaded.
- Overlay, **Home** page: one button per multiplier choice (Game's, Off, 2x to 6x). The current one is lit.
- Overlay, **FG Health** page: live graphs of the output frame rate, presents per real frame and VRAM, with a "healthy" or "short" mark for the multiplier in force. Shaded areas are the warm-up after frame generation turns on, when a shortfall is expected. It records nothing while Dynamic MFG is on.
- Overlay, **Diagnostics** page: Mode (fixed or Dynamic, the multiplier the game asked for and the multiplier in force), Dynamic MFG support, the Streamline version and DLSS-G version in use, and the preset in use.

## Settings

All settings are in the `[framegen]` section of `jmod\jmod.ini`, except where noted. Settings marked "live" apply while the game runs.

| Key | Default | Values | What it does | Applies |
|---|---|---|---|---|
| `ForceMultiplier` | `0` | `0` to `6` | `0` = the game decides. `1` = Off: frame generation is turned off whatever the game asks (this also wins over Dynamic MFG). `2` to `6` = force that exact multiplier. Turn off any other multiplier override so it does not replace this one. | Live |
| `DynamicMFG` | `0` | `0`, `1` | NVIDIA's Dynamic MFG: the runtime picks the number of generated frames each frame to reach the target. It replaces a forced multiplier while active and turns VSync off. Needs a Streamline 2.14 or newer wrapper, DirectX 12 and NVIDIA driver 595.41 or newer. The Diagnostic page shows whether it engaged. | Live |
| `DynamicTargetFPS` | `0` | `0` to `1000` | The displayed frame rate Dynamic MFG aims for. `0` = the display's refresh rate. The overlay slider goes to 500. | Live |
| `DynamicReflexSourceCap` | `0` | `0`, `1` | Also limits the game's rendered frames to the target through Reflex. Needs a non-zero `DynamicTargetFPS`. The game's own Reflex limit is restored when off. | Live |
| `ReflexFpsCap` | `0` | `0` to `1000` | Limits the frame rate through Reflex's frame limiter and overrides the game's own limit. `0` = off. Needs a game that uses Streamline Reflex. In the overlay, nothing changes until you click **Apply**; **Reset** removes the cap. | Live |
| `ReflexCapIsBaseFps` | `0` | `0`, `1` | `1` = `ReflexFpsCap` is the game's own (pre-generation) frame rate. It is multiplied by the multiplier in force before it goes to Reflex, so 45 stays 45 at 4x (180 displayed), and also when frame generation is off (menus). `0` = the value is the displayed frame rate. With Dynamic MFG the multiplier varies, so the base rate is approximate. | Live |
| `ReflexModeOverride` | `-1` | `-1` to `2` | Replaces the Reflex mode the game asks for. `-1` = the game's own, `0` = off, `1` = low latency, `2` = low latency + boost. Needs a game that uses Streamline Reflex. Frame generation wants Reflex on, so `0` can make DLSS-G report Reflex as not active. | Live |
| `VSyncOverride` | `-1` | `-1`, `0`, `1` | `-1` = the game's own, `0` = force VSync off (and allow tearing when the swap chain supports it), `1` = force it on. | Live |
| `SwitchBackendLive` | `1` | `0`, `1` | `1` = the chosen frame generation method takes over right away, by restarting frame generation for a moment. `0` = it applies the next time the game creates frame generation (after a toggle in its menu, or a restart). | Live |
| `RuntimeSelectionMode` | `0` | `0` to `2` | Which Streamline runtime the game loads: `0` = the game's default, `1` = prefer the copy in the game folder, `2` = force the one the NVIDIA driver downloaded. | Restart the game |
| `MultiplierUpHotkey` | `0` | a key code | Steps `ForceMultiplier` up. | Live |
| `MultiplierDownHotkey` | `0` | a key code | Steps `ForceMultiplier` down. | Live |
| `VSyncHotkey` | `0` | a key code | Cycles the VSync override: game's own, off, on. | Live |

The selected method is saved by the overlay (`FrameGenerationBackend`). Choose it on the Frame Generation page rather than editing it.

Related settings that this page shows, owned by other modules:

| Key | Section | What it does |
|---|---|---|
| `Enabled` | `[core]` | The master switch. `0` = JMOD changes nothing, and the game's requests go out unchanged. |
| `MaxGeneratedFrames` | `[unlock]` | `2` to `5` generated frames (up to 3x to 6x). It caps only the multiplier hotkeys. It does not limit what the game or DLSS-G does. |
| `ForceFlipMeteringOff` | `[unlock]` | Turns off the frame-pacing meter of the DLSS-G plugin. Needed only on games that freeze at 3x and above. DirectX 12 only. |
| `RaiseFrameCeiling` | `[unlock]` | Raises the plugin's own frame limit. Off by default. Updating the plugin is the sound fix. |
| `DlssgPresetOverride` | `[presets]` | The DLSS-G preset: driver default, A (UI recomposition off), B (UI recomposition on) or C (FG Lite, needs `nvngx_dlssg.dll` 310.9.16 or newer). Needs a restart of the game. |

## Hotkeys

Set these on the overlay's **Controls** page, or enter the decimal key code in `jmod.ini` (for example `33` = Page Up, `34` = Page Down). They need the overlay hotkey to be set, and they are ignored while the overlay panel is open.

| Hotkey | What it does |
|---|---|
| Multiplier up | Steps `ForceMultiplier` through the game's choice, then 2x up to the highest multiplier allowed. It stops at both ends. From Off, up goes to 2x. |
| Multiplier down | Steps the other way. From 2x it goes back to the game's choice. From Off it goes to the game's choice. |
| Cycle VSync | Cycles game's own, off, on. DirectX 12 only. |

Each step is shown in the on-screen message.

## Needs

- A game that uses DLSS Frame Generation through NVIDIA Streamline, on an NVIDIA RTX 40 series GPU.
- The `unlock` module for forced multipliers above what the game offers. Without it, only Off is applied, and otherwise the game's own multiplier is used.
- DirectX 12 for Dynamic MFG and VSync control. In a Vulkan game these controls are greyed out, and the game keeps its own.
- A game that uses Streamline Reflex, for the Reflex mode and cap.

## Known limits

- **Dynamic MFG is experimental.** It needs Streamline 2.14 or newer, DirectX 12 and driver 595.41 or newer.
- **Vulkan:** Dynamic MFG, VSync control and the flip-metering workaround are DirectX 12 only. The VSync hotkey is ignored.
- **Reflex mode** has an effect only after the game has set its Reflex options at least once.
- **Runtime selection and the DLSS-G preset** are read when the game starts, so a change needs a restart.
