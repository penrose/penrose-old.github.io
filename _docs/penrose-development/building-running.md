---
title: Building and Running
category: Penrose Development
order: 3
---

Quick start: (tested on recent Macs with the latest GHC)

* Navigate to `src/`
* [First time] For Alloy support: execute `make` in `src/`
* Compile the system by running `stack build` in `TOP`: <br />
The stack output will tell you the location `X` of the Penrose binary in the following format: 
"...Installing executable(s) in `X`".
The binary name is `penrose` and it is located in path `X`.
* [First time] Using Alloy requires running Penrose from `src`, so I recommend aliasing the command `runpenrose` to the location `X/penrose` of the binary. 
* In a separate tab, start a server within `src/`: `python -m SimpleHTTPServer` (or in case you use python 3: `python -m http.server`)
* Within `src/`, start the Penrose runtime, providing a pair of Substance/Style programs: `runpenrose linear-algebra-domain/twoVectors.sub linear-algebra-domain/linear-algebra.sty linear-algebra-domain/linear-algebra.dsl`
* View the visualization with interactive optimization: in your browser, navigate to http://localhost:8000/front-end/client.html
* You should get a diagram that looks similar to the first one [here](https://github.com/penrose/penrose/wiki/The-LA-Domain-Full-Spec).
* In the UI, try stepping, autostepping, and resampling the state. You can also drag objects to set a new initial state for the optimization. Output will appear in the Terminal.
* Terminate the servers with `Ctrl-C` when finished.
* Note: __Chrome is recommended as the front-end browser__

If you install libraries or change the project structure, you may need to update `penrose.cabal` and `stack.yaml`.

If you use emacs, syntax highlighting is available for DSL, Substance, and Style files [here](https://github.com/penrose/penrose/tree/master/src/penrose-modes).

NOTE: `stack` will sometimes unregister packages without warning you, resulting in having to rebuild the whole project, which takes 10-20 minutes. To check what `stack` will do beforehand, run `stack build --dry-run` or `stack build --dry-run --profile` (depending on which flags you used for the original build).
