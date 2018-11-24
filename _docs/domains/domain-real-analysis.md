---
title: The Real Analysis Domain
category: Domains
order: 3
---

_This domain is being developed. These are very preliminary diagrams, not final ones!_

You can find the real analysis DSL, Substance, and Style programs [here](https://github.com/penrose/penrose/tree/real-analysis/src/real-analysis-domain).

Here are the [example spec diagrams](https://github.com/penrose/penrose/issues/90).

## Style 1: perpendicular reals

Here is the [Style program](https://github.com/penrose/penrose/blob/real-analysis/src/real-analysis-domain/real-analysis.sty).

1. Two reals

Note: need to comment out the interval selectors for matching to work

`runpenrose real-analysis-domain/reals.sub real-analysis-domain/real-analysis.sty real-analysis-domain/real-analysis.dsl`

<p align="center"> <img src="assets/reals.png" width=500px> </p>

2. Function on interval

`runpenrose real-analysis-domain/interval.sub real-analysis-domain/real-analysis.sty real-analysis-domain/real-analysis.dsl`

<p align="center"> <img src="assets/func-interval-1.png" width=500px> </p>
<p align="center"> <img src="assets/func-interval-2.png" width=500px> </p>
<p align="center"> <img src="assets/func-interval-3.png" width=500px> </p>
<p align="center"> <img src="assets/func-interval.gif" width=500px> </p>

3. Point on function with derivative and integral

[Substance](https://github.com/penrose/penrose/blob/real-analysis/src/real-analysis-domain/point-perp.sub), [Style](https://github.com/penrose/penrose/blob/real-analysis/src/real-analysis-domain/real-analysis.sty)

Note: the derivative is currently not computed to lie tangent to the curve, since the internal representation of curves doesn't allow us to do that. Will be fixed when integrated with the new frontend. Also, the integral will be drawn as a path in the future.

`runpenrose real-analysis-domain/point-perp.sub real-analysis-domain/real-analysis.sty real-analysis-domain/real-analysis.dsl`

<p align="center"> <img src="assets/pt-deriv-int.png" width=500px> </p>
<p align="center"> <img src="assets/pt-deriv-int.gif" width=500px> </p>

## Style 2: parallel reals

Here is the [Style program](https://github.com/penrose/penrose/blob/real-analysis/src/real-analysis-domain/parallel-reals2.sty).

1. Function composition

Aiming to get close to the example in the Barthe book:

<p align="center"> <img src="assets/func-comp-barthe.png" width=500px> </p>

`runpenrose real-analysis-domain/interval-par.sub real-analysis-domain/parallel-reals2.sty real-analysis-domain/real-analysis.dsl`

These are three separate samples of the same Substance program.

<p align="center"> <img src="assets/func-comp-par-1.png" width=500px> </p>
<p align="center"> <img src="assets/func-comp-par-2.png" width=500px> </p>
<p align="center"> <img src="assets/func-comp-par-3.png" width=500px> </p>
