---
created: 2026-10-28 01:16
tags:
  - note
  - gamedev
  - tip
  - physics
aliases:
---
These are some cool ways ragdolls can be improved.

## Soft Limits

Usually, ragdolls just use hard limits, meaning the join can't rotate past a certain point.
But adding soft limits on top of that leads to a much more natural motion.
This is due to all the muscles and ligaments preventing a linear motion.

## Returning to neutral body posture

This leads to much more natural ragdolls, and it's insanely easy to implement too.
The joint just tries to go back to its natural position.
The natural position I am referring to is the one we see a human body default to when in 0 gravity.
You can learn more about it under [[Neutral body posture]].

[source](https://www.youtube.com/watch?v=yY6vck8SuCY)