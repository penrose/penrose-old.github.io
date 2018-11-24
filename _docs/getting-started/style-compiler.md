---
title: How the Style Compiler Works
category: Getting Started
order: 5
---

## Compiler phases

The compiler has four phases:
1. Selector static semantics (checking selector well-formedness and generating environments of the variables in each selector)
2. Selector matching semantics (finding the Substance variables that match each selector)
3. Translation semantics (applying each selector in turn to a matched Substance variable, accumulating its shapes and other information)
4. Analyzing the translation to generate the initial state and the objective function for the optimization problem

Phases 1, 2, and 3 (as well as the Style program parsing) are performed in `Style`. Phase 4 is performed in `GenOptProblem`. After phase 4, the initial state and the objective function are given to our optimizer, which interacts with the server and frontend to display the results.

See the design paper for the precise semantics of phases 1, 2, and 3.

NOTE: we will sometimes refer to the "optimization functions," which denotes the collection of all user-specified objective and constraint functions as well as all default-generated system objective and constraint functions (e.g. `minSize` and `maxSize`).

## Generating the initial state and objective function

A pair of a Substance and Style program is turned into a translation with the following type:

```
data Translation a = Trans { trMap     :: M.Map Name (FieldDict a),
                              warnings :: [Warning] }
```

Again, see the design paper for a more full description of the translation.

Here are the rest of the relevant types:

```
type Field = String
data FieldDict a = M.Map Field (FieldExpr a)

type ShapeTypeStr = String
data FieldExpr a = FExpr (TagExpr a)
                 | FGPI ShapeTypeStr (PropertyDict a)

type Property = String
type PropertyDict a = M.Map Property (TagExpr a)

-- For use in later translation evaluation at runtime
data TagExpr a = OptEval Expr      -- Thunk evaluated at each step of optimization-time
               | Done (Value a)    -- A value in the host language, fully evaluated

data Expr = ... -- Defined in Style: expressions in Style programs

data Value a = ... -- Defined in Shapes: a fully-evaluated Style expression, which is a value
```

To generate the initial state, the following steps are taken in `GenOptProblem`:

1. Find the varying paths in the translation (all uninitialized floats)
2. Find the uninitialized paths in the translation (all uninitialized non-floats **and** label dimensions, determined by `pendingProperties` in `Shape` module).
3. Find all the paths corresponding to all properties of all GPIs
4. Modify the translation so that all the varying fields (floats) are sampled, and no longer marked as `OPTIMIZED` (fields can't be categorical and sampled)
5. Modify the translation so that all the GPIs' properties are sampled or instantiated with default values (including those that are not set by the user in the Style program). Sample varying properties (floats) and categorical/product properties (style, tuples, etc.) The resulting instantiated translation, after steps 4 and 5, is called `transInit`.
6. To generate the initial GPIs that are drawn on the screen, we evaluate the translation, meaning that we evaluate every GPI in `transInit`. (For example, we might evaluate `[X.shape, Y.shape]`.)
7. To get the initial state (a list of floats for the optimization), we look up all the varying paths in the **evaluated** translation. This is the initial value of `varyingState`, which is updated after each step by the optimization.

To generate the objective function, the following steps are taken in `GenOptProblem`:

1. Find all the objective and constraint functions in the translation
2. Create all the default objective and constraint functions
3. Generate the objective function. It is a function partially applied with `transInit`, the optimization functions, and the varying paths. When fully applied with the penalty weight and the varying values (a list of floats), it creates a `varyMap` that maps from the varying path to its current value. Then it evaluates all arguments of all optimization functions, in order. [1] Then each optimization function is applied to its evaluated arguments (which are now values of the `Value a` type), and each of the resulting energies is weighted (with an extra weight if it's a constraint) and summed to yield the overall energy. [2]

[1] For example, if one of the functions was `center(x.shape, y.shape.y, 1 + 2)`, it will evaluate `x.shape`, then `y.shape.y`, then `1 + 2`, recursively caching the result of each evaluation in the translation for this optimization step.

[2] The gradient is taken of this overall objective function with respect to `varyingState`. Because all floats are polymorphic, autodiff is automatically taken with respect to the intermediate expressions/computations. See internal documents on the computational graph for more about how this works.

## Representations of the state

It is important to know that there are **three** representations of the state stored in `State`. It is also important to understand the differences between them, and to know when each representation is produced and modified (and its changes propagated to other representations).

The representations are:

1. The initialized translation
2. The varying state
3. The GPIs (list of shapes)

The **initialized translation** is produced in the compiler analysis phase. It is used by the objective function, which refers to it and locally updates it in order to evaluate the arguments to the optimization functions. It is not modified by the optimizer, but may be modified by the server and front-end to update the label dimensions (stored in the uninitialized paths), since those are only known at render-time.

The **varying state** is initialized in the compiler analysis phase after evaluating the translation. It is modified by the optimizer after each step. If the front-end receives an update to the GPIs, then it updates the varying state to match (e.g. in `updateShapes` and `dragUpdate`). NOTE: The ordering of the varying paths list must stay the same as the ordering of the varying state. In fact, neither one's order should need to change at all. This is important so that `varyMap` is created correctly. For example, anytime `varyState` is constructed using a `foldl`, check if you need to reverse the order of the list (as in `shapes2floats`).

The **list of shapes** is initialized in the compiler analysis phase after evaluating the translation. After each step of the optimizer, after the varying state is updated, the translation is evaluated and the list of shapes is updated for the front-end to render. [1] The list of shapes may be updated by the front-end if the user performs a UI action (e.g. dragging or resampling), in which case the edits are propagated to the varying state and initialized translation.

[1] We don't have to re-evaluate the translation after each step of the optimization. This is just for the benefit of the user so they can see each step live.

## How the translation is initialized

### Fields

All varying fields (floats) are sampled from the `canvasDims` interval defined in `Shapes`. No non-varying fields are initialized. The logic can be found in `initField` in `GenOptProblem`.

Labels are treated as a special case, where for a Substance object `X`, `X.label` is automatically set to its label in Substance if it exists (e.g. `Label X $\frac{1}{x^2}$`) and to the empty string if not.

### Shape properties

Shape properties are initialized as follows:

* Any property explicitly set to `OPTIMIZED` is sampled (according to the sampler defined for that property for the shape).
* Any property that a shape must have, that is **not** specified by the Style writer, is sampled (again according to the sampler defined for that property for the shape). (For example, if the Style writer doesn't specify the radius of a circle, it's sampled according to the canvas size.) This applies to both floating and non-floating properties.
* Any property explicitly set by the Style writer to a constant value is set to the constant value.
* Any property explicitly set by the Style writer to an expression may be partially evaluated at compile-time as much as possible (e.g. `r = 300 / 50 + x.shape.r` may be evaluated to `r = 60 + x.shape.r`), then set to the resulting expression. (Currently we don't do compile-time evaluation.)
* The "name" property of a shape is automatically set to its name (e.g. `X.shape`).

The logic can be found in `initProperty` in `GenOptProblem`.

## How expressions are evaluated

The function `evalExpr` contains all the logic for evaluating expressions. In short, `evalExpr` recursively evaluates expressions, subject to an iteration depth (in case of cycles in the computational graph). It caches results of evaluations in the translation, and it only evaluates those sub-expressions that are needed for the overall result. The two kinds of top-level expressions that might be evaluated are either optimization function arguments (in `genObjFn`) or shape paths (in `evalShape`).

The base case of `evalExpr` is if an expression is a value or evaluates to a value, in which case it returns the value.

To improve lookup speed in `genObjFn`, `evalExpr` uses an optional parameter `varyMap` that maps from a varying path to its current value in the optimization. Whenever a path needs to be looked up, `lookupPropertyWithVarying` will first try to find it in `varyMap`, and default to the translation otherwise.

`evalExpr` deals with the following main cases:
1. Evaluating an expression (inline computation or calling a system-defined computation). The expression's arguments are recursively evaluated, then the computation is performed and the result returned.

2. Evaluating a path. Since the function has the path, it can cache the result of evaluating the expression that the path points to.
    1. Evaluating a field that is not a GPI. The expression that the field refers to is evaluated, cached, and returned.

    2. Evaluating a field that is a GPI. Each property path of the GPI is evaluated in turn, and each value is stored in a new property dictionary, which is returned with the updated translation.

    3. Evaluating a GPI property. The expression that the property refers to is evaluated, cached, and returned.

## How the shapes are rendered

* Organization of the code: `main.js` and `graphics.js` are imported by the frontend HTML page.
    * `main` contains:
        * common parameters: DEBUG flag, sample interval for rendering (affects the FPS), size of the SVG canvas, and port number for the WebSocket connection
        * server communication logic
        * `Utils` module
    * `graphics` contains the `Render` module
* The JS codebase employs the [module pattern](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html):
    * `Utils` contains miscellaneous helper functions and exposes the ones that are most commonly used functions such as `scr` to translate from math coordinates to screen coordinates
    * `Render` contains all rendering functions that uses `Snap` and exposed one function `renderScene` (TODO: `collectLabels` is also there, which calls `tex2svg` to render the LaTeX labels using MathJax)
* The JSON format for shapes
    * the server and frontend communicate to each other using JSON packets over a WebSocket connection
    * the JSON format is automatically generated by a Haskell library, [Aeson](http://hackage.haskell.org/package/aeson-1.4.0.0/docs/Data-Aeson.html), which translates from GPIs in Haskell to JSONs. Refer to the Aeson documentation for details.
* Transforming coordinates to screen space
    * The coordinates in Style assume the center of the canvas to be the origin, whereas Snap does not.
    * Therefore, all positional properties, such as `x`, `y`, and `path`, of GPIs passed from the server need to be translated by some amount to reflect Style's specification precisely. This is done by calling `Utils.scr` on such properties before rendering the GPIs.
* Debugging the front-end (debug flags, what is logged to console, etc.)
    * When `DEBUG` is on, all debug messages will be printed in the console, which often include the JSON received, SVG elements generated for GPIs and labels, and optionally other information.

## Debugging Style programs and the compiler

(TODO: write this out)

* Look at the compiler output
* Look at the warnings in the translation

## Areas of improvement

(TODO: write this out)

* Clarify if we allow GPI properties that refer to other GPIs, e.g. `start = A.shape`
* Evaluating expressions at compile-time
* Evaluating GPIs may not be cached?
* Clarifying matching semantics for multiple Substance objects of the same type (lexicographic order?)
* Internal namespace name handling
* See [PR README](https://github.com/penrose/penrose/pull/121)
