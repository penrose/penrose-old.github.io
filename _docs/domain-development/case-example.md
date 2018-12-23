---
title: "The Real Analysis Domain: A Case Example"
category: Domain Development
order: 4
---

## Notes on `parallel-reals.sty` and `parallel-reals2.sty`

Domain programs can be found [here](https://github.com/penrose/penrose/tree/real-analysis/src/real-analysis-domain).

We can write a Style reasonably generically and concisely to cover most of the likely RA programs and their variations. This Style works for the "parallel reals" visual representation. This is good news!

We are in fact able to solve the problem of `f : R -> R` and `x \in Dom(f); y \in Cod(f)` using most of the existing language features. We make a new shape attached to `f` for the former, and for the latter we amend the selector and attach the Real to the correct y value for f's codomain.

Some parts of the Style don't scale / break down compositionally (e.g. composing arbitrary functions and applying some of them). Some parts of the Style do scale up compositionally (e.g. applying arbitrary numbers of functions). It feels quite like a proof by induction: first, write a selector for the base case (e.g. applying a function to an argument). Then, write a selector for the inductive case (e.g. applying a function to an argument, then applying another function to the result). Then that works for any number of functions applied.

It feels difficult to reason about programs written in Substance that use multiple Style selectors (e.g. function application and composition) and how the combination of an arbitrary combination and number or Style selectors will cascade. I had to write out a lot of examples by hand. There were some tricky examples where for one special case (e.g.` R -> R`) we had to do a special delete, or something was deleted twice, or some field didn't exist.

We need to be able to abstract across a Substance object's fields in Style. That is, we need to be able to say: a `Real`'s fields in Style include `val` and `yval`, and any other selector can depend on that, and no selector will delete those fields. And a "special" `Real` might have some additional fields, shapes, objectives, and constraints that we can depend on. And we must be able to depend on these fields for subtypes: if an `Interval` has fields for `left` and `right`, then `Reals` (as a subtype of `Interval`) should also have a `left` and `right`, and so should `ClosedInterval`, etc. 

JA's comments: Match on existence of field? Check to see if fields exist?

It feels cognitively challenging to write a Style program. This was a phenomenon that Jenna and I noticed early on. Since you're really writing the "minimum" code needed to wire up the spatial relationships just right (not overconstrained and not underconstrained), you tend to think for a long time about each line of Style code needed (e.g. `x.left = y.right`). But the upside is that that's all the code you need to write!

## Style-writing style

It feels like a good "Style-writing style" to break down selectors as much as possible: start with just `Real`, then `Real` and `In`, and `Real` and `Function` and `In`. Or `Interval`, then various subtypes of `Interval`s, then `Interval`s with various kinds of endpoints, then `Interval`s with `Subset`. This makes each one more modular/generic, less specialized to a specific Substance program, and more likely to generalize.

## Next steps

We need to implement new language features, most urgently the programmatic number of shapes, matching WRT subtyping, matching of value deconstructors, the `?=` in selectors, and the custom shape API. I'm using all of them in this Style program and it feels pretty natural. 
