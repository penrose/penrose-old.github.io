---
title: Performance and Profiling
category: Penrose Development
order: 7
---

## Building, profiling, and running

To build the system: `stack build --profile` 

To run it: `stack exec -- <path-to-penrose-binary> +RTS -p -RTS <command-line flags>`

To view the report: `src/` will contain a file called `penrose.prof` 

## To get better Haskell reports

Try inlining functions, using `seq` to force evaluation, and setting cost centers. Also see the GHC guide on [profiling](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/profiling.html).

## Profiling the front-end

See this Chrome DevTools [writeup](https://developers.google.com/web/tools/chrome-devtools/rendering-tools/).

## Related GitHub issues

[Profiling optimization](https://github.com/penrose/penrose/issues/13)

[Debugging optimization](https://github.com/penrose/penrose/issues/116)

