---
title: Parameters
category: Penrose Development
order: 5
---

Possibly outdated, since we've switched to snap and added new parameters.

* `stepsPerSecond`: number of simulation steps for `gloss` to take for each second of real time
* `picWidth`, `picHeight`: canvas dimensions
* `stepFlag`: turns stepping the simulation on and off for debugging (no stepping = objects don't move)
* `clampFlag`: turns clamping gradient values on and off for debugging
* `debug`: turns on/off the debug print functions
* `constraintFlag`: turns constraint satisfaction on/off (currently off because we're doing unconstrained optimization)
* Default ambient objective functions are specified in `ambientObjFns`, and analogously for `ambientConstrFns`.
* Default objective functions are specified in `genObjsAndFns`.
* `btls`: turn on/off the backtracking line search for debugging (off = use a fixed timestep specified in the code)
* `alpha` and `beta`: parameters for the backtracking line search (see code for a more detailed description)
* `stopEps`: stopping condition sensitivity for gradient descent. Stop when magnitude of gradient is less than `stopEps`.
