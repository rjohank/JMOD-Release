# Multi Frame Generation unlock (`unlock`)

## What it does

This module makes DLSS Multi Frame Generation available on GeForce RTX 40 series cards, with multipliers above 2x, in games that use DLSS Frame Generation through NVIDIA Streamline. The other frame generation modules ([framegen](framegen.md), [xefg](xefg.md)) rely on it for multipliers above what the game offers on its own.

There is nothing to switch on: the module works as soon as the game starts frame generation. You choose the multiplier on the Frame Generation page.

## Where to find it

- Overlay, **Frame Generation** page, in the **Multiplier** section: **Max generated frames (multiplier hotkeys)**.
- Overlay, **Frame Generation** page, in the **Plugin workarounds** section: the two compatibility options below.
- Overlay, **Status**: the line "Multi frame generation unlock" shows yes or no.
- Overlay, **Diagnostics** page, **Frame generation**, **Unlock**: "Multi frame generation unlock" shows active or not active.

## Settings

All settings are in the `[unlock]` section of `jmod\jmod.ini`.

| Key | Default | Values | What it does | Applies |
|---|---|---|---|---|
| `MaxGeneratedFrames` | `4` | `2` to `5` | The most generated frames per real frame that the multiplier hotkeys step up to: `2` to `5` generated frames = 3x to 6x. It does not limit what the game or DLSS Frame Generation does on its own. | Live |
| `ForceFlipMeteringOff` | `0` | `0`, `1` | A compatibility option for older games that freeze at 3x and above. Current games pace correctly on their own: turn it on only if you hit that freeze. DirectX 12 only. | Restart the game |
| `RaiseFrameCeiling` | `0` | `0`, `1` | Lets a game's own multiplier limit stand instead of being lowered. Off by default, because raising the limit of an older game is not guaranteed to be safe in every game. Updating the game's DLSS Frame Generation files is the sound fix. | Restart the game |

## Hotkeys

None. The multiplier hotkeys are on the [framegen](framegen.md) page, and they use `MaxGeneratedFrames` as their upper limit.

## Needs

- An NVIDIA GeForce RTX 40 series GPU.
- A game that uses DLSS Frame Generation through Streamline.
- The `[core] Enabled` master switch on. With it off, JMOD changes nothing.

## Known limits

- The multiplier a game or its Streamline files allow can still be lower than what you ask for. The Diagnostics page shows the multiplier the game requested and the one in force.
- The compatibility options are for specific problems. Leave them off otherwise.
