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

## Layering animations

This follows the same idea of [[#Returning to neutral body posture]], but applied to other cases too.
For example, you can layer a falling animation while the ragdoll is affected by an explosion.
Or constantly try to reach the protective pose while falling down the stairs.
Another cool example would be hands reaching for a gunshot wound while dying.

## Different states

This is yet another idea we can add on top of [[#Layering animations]].
For example, an alive character might reach out to stop the fall, whilst a dead character will just fall flat to the ground.

## Dislocated joints and broken bones

If enough force is applied to a joint, you can break it by having the motion range move over the limit, and become slightly smaller.
On top of this, it may be possible to add completely stiff joints along the bones that have an even higher breaking point, and this could simulate actual broken bones.

[Improving ragdolls forever](https://www.youtube.com/watch?v=yY6vck8SuCY)
[Ragdolls improved by YOUR suggestions.. you weirdos](https://www.youtube.com/watch?v=X1V_VeR0h4E)