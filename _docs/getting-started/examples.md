---
title: Examples
category: Getting Started
order: 2
---

Here are the main example program pairs that should work together: (written in this format so they can be copied as arguments)

## Simpler examples

* One subset relationship: `runpenrose sub/twosets.sub sty/venn_comp.sty dsll/setTheory.dsl`

<p align="center"> <img src="assets/one-subset.png" width=200px> </p>

* Two points, one with computed location: `runpenrose sub/twopoints.sub sty/twopoints.sty dsll/setTheory.dsl`

<p align="center"> <img src="assets/two-pts-set.png" width=200px> </p>

* Point in set relationship: `runpenrose sub/venn_comp_pt.sub sty/venn_comp_simple.sty dsll/setTheory.dsl`

<p align="center"> <img src="assets/pt-in-set.png" width=200px> </p>

* Sets with each pair linked: `runpenrose sub/threesets.sub sty/test-local.sty dsll/setTheory.dsl`

Note that there are **six** arrows: one between each pair of sets.

<p align="center"> <img src="assets/test-local.png" width=200px> </p>

* Optimized fields and properties: (here the padding and radii are optimized to lie within ranges) `runpenrose sub/threesets.sub sty/test-optimized.sty dsll/setTheory.dsl `

<p align="center"> <img src="assets/optimized-field.png" width=200px> </p>

## More complex examples

* Subset relationships in Venn diagram style: `runpenrose sub/tree.sub sty/venn.sty dsll/setTheory.dsl`

<p align="center"> <img src="assets/subset-venn-port.png" width=200px> </p>

* Subset relationships in tree diagram style: `runpenrose sub/tree.sub sty/tree.sty dsll/setTheory.dsl`

Without the same-y-coordinate objective, we get a radial layout:

<p align="center"> <img src="assets/tree-radial.png" width=200px> </p>

With the same-y-coordinate objective, we get a linear layout: (takes much longer to converge)

<p align="center"> <img src="assets/tree-linear.png" width=200px> </p>

* Continuous map in set theory style: `runpenrose sub/continuousmap.sub sty/continuousmap.sty dsll/setTheory.dsl`

The first few samples have trouble converging:

<p align="center"> <img src="assets/continuousmap-sample2.png" width=600px> </p>

On the fourth or fifth resample, we get a nicely converged diagram:

<p align="center"> <img src="assets/continuousmap-sample4.png" width=600px> </p>

* Surjection in Cartesian style: `runpenrose sub/cart-test.sub sty/cart-test-pt.sty dsll/setTheory.dsl`

<p align="center"> <img src="assets/surjection-computed.png" width=300px> </p>

* Surjection in Cartesian style with special point: `runpenrose sub/cart-test-pt.sub sty/cart-test-pt.sty dsll/setTheory.dsl`

<p align="center"> <img src="assets/surjection-computed-point.png" width=300px> </p>

* Nicer computed surjection (using `Map` type only): `runpenrose sub/surjection_computed.sub sty/surjection_computed.sty dsll/setTheory.dsl`

<p align="center"> <img src="assets/surjection-nice.png" width=400px> </p>

### Libraries

In general, we want to build up Style libraries that work for domains and are not tied to particular Substance programs. So far we have two examples for set theory.

* Any set theory programs should work with `venn.sty` or `tree.sty`.

### In progress

* Upcoming linear algebra diagrams

### Currently not working

* None!

### Deprecated

Pending a port using Alloy:

* Surjection in abstract style: `runpenrose sub/surjection.sub sty/surjection-abstract.sty dsll/setTheory.dsl`
    - NOTE: Due to optimization parameters, this example might converge prematurely. Drag the geometries around with `Autostep` on to break out of local minima and get better results.

<p align="center"> <img src="assets/Examples-f2376.png" width=300px> </p>

* Bijection in set theory style: `runpenrose sub/bijection.sub sty/bijection.sty dsll/setTheory.dsl`

<p align="center"> <img src="assets/Examples-1d778.png" width=300px> </p>

* Function composition in set theory style: `runpenrose sub/composition.sub sty/composition.sty` (slow to optimize)

<p align="center"> <img src="assets/Examples-88e38.png" width=500px> </p>

* Surjection in set theory style: `runpenrose sub/surjection.sub sty/surjection.sty dsll/setTheory.dsl`

<p align="center"> <img src="assets/Examples-7d0a3.png" width=300px> </p>

* Injection in set theory style: `runpenrose sub/injection.sub sty/injection.sty dsll/setTheory.dsl`

<p align="center"> <img src="assets/Examples-a3a2e.png" width=300px> </p>

Pending the addition of `Surjection(f)`, `Bijection(f)`, `Injection(f)` predicates:

* Computed bijection: `runpenrose sub/bijection_computed.sub sty/bijection_computed.sty dsll/setTheory.dsl`

<p align="center"> <img width="300px" alt="image" src="https://user-images.githubusercontent.com/19957447/39818332-a27401b4-536e-11e8-935a-2fe419bcc33a.png"> </p>

* Computed injection: `runpenrose sub/injection_computed.sub sty/injection_computed.sty dsll/setTheory.dsl`

<p align="center"> <img width="300px" alt="image" src="https://user-images.githubusercontent.com/19957447/39818434-e6666736-536e-11e8-982b-01d85835efef.png">
</p>
