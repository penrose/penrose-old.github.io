---
title: Extending the System
category: Domain Development
order: 4
---

**Note: this page needs to be updated**

## Adding a new shape
Let's say the datatype name is `Shape` (e.g. `Circ`). Here's how you add a new shape to the system.

**To be updated.**

## Writing new objective functions and constraints

* In Functions, write your function as a Haskell function of type `ObjFn`.
* Register it in either `objFuncDict` (if an objective) or `constrFuncDict` (if a constraint).

## Writing new computations

* In Computation, write a wrapper function as a Haskell function of type `CompFn a`. Inside the wrapper function, annotate your real function with its expected input argument number, types, and order, and its expected output type. (There is a limited number of allowable types.)
* Call your real computation from the wrapper function.
* Register your wrapper function in `computationDict`.

## Adding new object properties

* Add a getter and setter in `Shapes`, or look up the result in the object's configuration in `init{SHAPETYPE}` in Runtime.
* More standard interface coming soon.

## Extending Substance and Style

Coming soon.

## Writing a custom style

Coming soon.

## Adding a new renderer

Coming soon.
