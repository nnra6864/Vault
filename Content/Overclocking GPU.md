---
created: 2026-11-06 16:54:50
tags:
  - note
  - guide
aliases:
---

Overclocking your [[GPU]] is a great way to squeeze out some extra performance for free.
For example, I went from 50FPS to 60FPS in [[Minecraft]] with shaders, and in some regions from 80FPS to over 100FPS.
The tool I recommend using is [LACT](https://github.com/ilya-zlobintsev/LACT).

## Configuration

- Power Usage Limit
    I would recommend increasing it to the max.
    There is no reason not to do this unless you are trying to power save.
- GPU P-State 0 Clock Offset (MHz)
    Try pushing this as high as you can without any artifacts or stability issue.
    For my rtx 3060, the value of 150 worked the best.
- GPU Voltage Boost (%)
    This is the risky one and I would just leave it alone.
- VRAM P-State 0 Clock Offset (MHz)
    Try to push this as far as you can without any artifacting or issues.
    For me, the value of 1500 worked the best.

![[../Assets/Overclocking_GPU.jxl]]
