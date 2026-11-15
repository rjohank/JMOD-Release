# DLSS feature information (`ngx`)

## What it does

This module keeps track of the DLSS features a game creates and uses: Super Resolution and Ray Reconstruction, with the sizes and the mode they were created with. Other modules use this information:

- [presets](presets.md): the resolution shown on the Upscaler page, and the quality mode and ratio overrides;
- [framegen](framegen.md) and [xefg](xefg.md): frame generation;
- [fg-inject](fg-inject.md) and [sr-inject](sr-inject.md): frame generation and DLSS for games that do not have them;
- [dlssnr](dlssnr.md): Neural Rendering;
- [perf](perf.md): the upscaler and frame generation timings;
- [osd](osd.md): the upscale ratio.

It has no page and no settings of its own. It does nothing noticeable by itself. It only acts for a feature that another module asked for.

## Where to find it

There is nothing to find in the overlay. The results show on the pages of the modules above.

## Settings

None.

## Hotkeys

None.

## Needs

Keep it in `jmod\modules` together with the other modules. Several of them do not work without it.

## Known limits

- It sees only games that use NVIDIA's DLSS, through Streamline or directly.
