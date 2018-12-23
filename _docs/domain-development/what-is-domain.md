---
title: What is a domain?
category: Domain Development
order: 1
---

When you develop a domain, you might be interested to develop one or more of
the following things:

 - **Domain Specific Language Logic**
 - **Style**
 - **Substance**

For this introduction to development, we are going to first review a high
level understanding of the files, and then how they are related to one another.
Organization is important to understand so that you are able to develop
your code in a modular fashion.

## Domain Specific Language

The Domain Specific Language Logic (DSLL) is the core of Penrose. 
(These are also called Elements?). This might
be where we define different predicates and operators that are specific
to a kind of math. For example, here is a dsl file for linear algebra:

```
tconstructor Scalar : type
tconstructor VectorSpace : type
tconstructor Vector : type
tconstructor LinearMap : type

operator Neg (v : Vector) : Vector
operator Scale (c : Scalar, v : Vector) : Vector
operator AddV (v1 : Vector, v2 : Vector) : Vector
operator AddS(s1 : Scalar, s2 : Scalar) : Scalar
operator Norm (v1 : Vector) : Scalar
operator InnerProd (v1 : Vector, v2 : Vector) : Scalar
operator Determinant (v1 : Vector, v2 : Vector) : Scalar
operator Apply (f : LinearMap, v : Vector) : Vector

predicate In (v : Vector, V : VectorSpace) : Prop
predicate From (f : LinearMap, V : VectorSpace, W : VectorSpace) : Prop
predicate Not (p1 : Prop) : Prop

-- Prelude Example

-- Examples for prelude, just for reproducing
value T : VectorSpace
value T1 : VectorSpace
```

It will be the case that we have a smaller number of core domains.

## Style

Style you can think of like cascading style sheets. It's basically rules
for how to render different components (shapes, lines, etc.) that are defined
for a domain. These files are prefixed wit *.sty. It's typically the 
case that a style is created to describe a particular domain. So we can
make this assertion that a style might point back to its domain:

```
[style]  --- describes --> [domain specific language]
```

This means that many styles can exist to describe the same DSL:

```
                +-------------------------------+
[Style A] ----> |                               |
[Style B] ----> |   Domain Specific Language    |
[Style C] ----> |                               |
                +-------------------------------+
```

And this is logical! If you have a favorite pair of black pants, you might
have several sets of other things that you wear to style it up. The black
pants don't depend on the accessories, but the accessories might be
intended for use with the black pants.

## Substance

Let's say that we have a DSL, and we've also chosen a way to style it. The
substance is an instantation of these two things with a particular equation.
For example, here is a substance definition for adding two vectors. This
is a text file with extension *.sub. The  elements that we include are 
defined in the domain. They will be styled by the style that we choose.

```
VectorSpace U
Vector A, B, C

C := AddV(A,B)

In(A, U)
In(B, U)
In(C, U)

Label A $\vec{u_{1}}$
Label B $\vec{u_{2}}$
Label C $\vec{u_{1} + u_{2}}$
AutoLabel U
```

This also means that a substance file is intended for a particular domain, 
but again, we might have more than one style for it.

Now that you understand the components (and relevant files) involved, let's
[show you how we organize them]({{ site.baseurl }}/domain-development/organization/) 
in the [Penrose Library](https://www.github.com/penrose-lib)
organized under the Github organization "penrose-lib." 

