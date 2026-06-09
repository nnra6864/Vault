---
created: 2026-06-07 01:18
tags:
  - note
  - tip
  - gamedev
aliases:
---
Pretty cool [[Pathfinding]] concept I randomly encountered whilst watching [this video](https://www.youtube.com/watch?v=H29dADzyuQo).
Instead of a single forward [[Raycast]], you use the [[Normal]] of the the entity to cast a ray up, then forward, then down, basically creating a hook.
Video showcases it being used for a centipede, but it can be used for any insect, or any other creature that can climb walls.
Combined with [[A Star]] [[Algorithm]], it can be pretty powerful. 