---
created: 2026-07-30 00:18
tags:
  - note
  - tip
  - gamedev
  - 3d
  - 2d
aliases:
---
Looping noise textures can be insanely useful when working on animations, especially baked ones, to achieve a seamless, infinite animation.
Whilst you can just animate the noise back to initial values at the end, this doesn't really give good looking results in most cases, as it causes the animation to play in reverse.
Instead, using 2 noise textures, animating them the same way, and slowly interpolating between them solves pretty much all the issues.

## Blender Example

In order to explain it better, lets use a [[Blender]] [[Geometry Nodes]] example.
The goal is to animate the Z offset of the noise texture from 0 to 25.

1. Add a `Noise Texture` node and animate its Z offset from 0(first frame) to 25(last frame+1).
   [[Looping_Noise_Texture_1.jxl]]
2. Duplicate these nodes, and animate the Z offset from -25(first frame) to 0(last frame+1).
   [[Looping_Noise_Texture_2.jxl]]
3. Use the `Mix` node to combine the noise `Factor` outputs, and animate the mix `Factor` from 0(first frame) to 1(last frame+1).
   [[Looping_Noise_Texture_3.jxl]]
4. Open the `Graph Editor`.
   [[Looping_Noise_Texture_4.jxl]]
5. Select all the geometry nodes, then select all the keyframes, and set their `Interpolation Mode` to `Linear`.
   [[Looping_Noise_Texture_5.jxl]]

This setup ensures a seamless, infinite loop.