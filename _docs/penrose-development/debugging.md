---
title: Debugging
category: Penrose Development
order: 4
---

### General tips

* Prevent bugs in the first place by writing property-based tests and unit tests before you start your code!
* Don't forget that Haskell is lazily evaluated, so the stepping and tracing output might not be what you expect.
* Check if all tests pass.
* In `Utils`, turn on the debug flags to see additional output.
* Try removing different lines from Substance and Style programs to find a minimal example, minimal objective function, or minimal combination of functions that fails.
* Print intermediate values with [Debug.Trace](https://hackage.haskell.org/package/base-4.9.0.0/docs/Debug-Trace.html)
 (use `tr` or the appropriate variant so we can turn off debug output easily).
* Use `error` to stop the code when an assertion is violated (e.g. a NaN appears as an argument to an objective function).
* ghci should be able to load Penrose if you run `stack ghci` in `TOP`. You can [step through code in the ghci debugger](https://github.com/penrose/penrose/commit/c368dcd94c83de80da85265d809e8e846a85e93b#comments). For example,
```
stack ghci -- load project
:set args snap src/sub/twosets.sub src/sty/venn_comp.sty -- set input arguments
:set stop :list -- display code at breakpoints
:break timeAndGrad -- set breakpoint
Main.main -- need to wait a bit, then open in browser as usual
:step -- repeat until state is in scope
let gradf = appGrad f
gradf state -- see the NaNs
```
* Atom comes with a graphical Haskell debugger, which shows line numbers/locations inline. It's very useful!
* Did you swap or mistype `x` and `y` or a coordinate index?
* **WARNING: may rebuild all dependencies**: Try compiling with `stack build --profile`, which will give stack traces when errors are thrown
* Try asking GHC to show type defaults: `stack build --ghc-options -Wtype-defaults`

### Debugging optimization

* Optimization slow? Try turning off all the debug flags.
* [Test autodiff in ghci by taking gradients of small functions](https://gist.github.com/hypotext/1a442ca736556a2fc989edeb7458166c).
* Look at the graph of the analytic gradient in Wolfram Alpha. Check that it has local minima where you want them.
* Check if there are NaNs because a gradient has variables in the denominator, possibly resulting in a divide-by-zero.
* Check which fields are set to NaNs, and take a closer look at objective functions that vary as a function of those fields.
* Check if objects have disappeared (sizes are set to <= 0) or gone offscreen (coordinates are not on canvas).
* Check if the line search is converging (or at least terminating).
* Check if the optimization is converging (or at least terminating), e.g. check that the norm of the gradient is decreasing.
* If the optimization is returning NaNs or Infinity: go step by step, and check when the NaNs/Infinity are starting to happen, and which inputs are causing it. If the state/energy looks like it's blowing up (e.g. 1e16, 1e23...) then it could be that **you have written a Style program that is unsatisfiable**. For example, you might write a constraint that a point needs to be in a circle, but you've written another computation that sets the point to lie outside the circle. Try removing/loosening constraints and computations until the optimization can converge.

### Debugging laziness

* `seq :: a -> b -> b` enforces evaluation of the first argument while ignoring the value of it, which is useful for debugging. For instance, `y` is evaluated but _not_ used in the following code snippet
```haskell
f x = let y = last [1..10000000000] in y `seq` x

main :: IO ()
main = do
    let z = f 10
    print z
```

### TypeScript Gotchas

#### Adding npm modules

This might be the most difficult process in TypeScript. If the module doesn't already contain type definitions, someone probably wrote them in the `DefinitelyTyped` repository (the types for `my-module` might be available if you `npm install @types/my-module`).

If you still run into errors with importing afterwards (or if a `DefinitelyTyped` module doesn't exist), add the following line to your `index.d.ts`:

```ts
declare module "my-other-module";
```

If necessary, restart the hot reloading process (`ctrl-c` and then `npm start` again).

#### npm module has no default export

Instead of using `import myModule from "my-module`,  try `import * as myModule from "my-module"`.

#### Adding files

If you get a TypeScript compilation error after adding a new file and you have no idea why, restarting the hot reloading process usually works.

### Other tips

* Make sure the Substance and Style programs were parsed correctly.
* Look at the frames and objects in the HTML and the Chrome inspector. Check that the objects are present in the SVG and that the server has received the frames.
* Check that objects are being packed and unpacked correctly, and that their order in the state has not changed.
* In the browser, use `console.log` (making sure the console displays all debug output).
* Try `git bisect` or `git blame` to see how far back the bug goes or when it was introduced.
* Did you forget a `break` statement in Javascript?
