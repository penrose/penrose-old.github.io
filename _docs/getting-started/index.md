---
title: About
category: Getting Started
permalink: /getting-started/index.html
order: 1
---

## Is Visualization Good Enough?

Let's say you are writing a paper. It has a function that you are including as LaTex,
and it renders to this (the graphic is from the 
<a href="http://penrose.ink/Penrose_DSLDI_slides.pdf" target="_blank">original slides</a> 
by one of the authors):


Now let's say you are feeling adventurous, and want to help your readers by including
a diagram. This should be fairly simple, right? Here is how it looks to generate
with <a href="http://www.texample.net/tikz/" target="_blank">tikz</a>:

```
\documentclass{article}
\usepackage{tikz}
\begin{document}
\pagestyle{empty}
\begin{tikzpicture}
 \begin{scope}[shift={(3cm,-5cm)}, fill opacity=0.5]
 \draw[fill=red, draw = black] (0,0) circle (5);
 \draw[fill=green, draw = black] (-1.5,0) circle (3);
 \draw[fill=blue, draw = black] (1.5,0) circle (3);
 \node at (0,4) (A) {\large\textbf{A}};
 \node at (-2,1) (B) {\large\textbf{B}};
 \node at (2,1) (C) {\large\textbf{C}};
 \node at (0,0) (D) {\large\textbf{D}};
 \end{scope}
\end{tikzpicture}
\end{document}
```

It makes a pretty picture...

![img/about-tikz.png](img/about-tikz.png)

But look at the code above! Yuck. We aren't actually understanding any of the notation,
we are just brute force drawing circles and adjusting the position and coloring.
If you did this for a paper, after figuring out all of the above you'd be
exhausted. The problem is that <b>our visualization software doesn't understand the math</b>.

Let's show another example from the slides, this one with <a href="http://www.graphviz.org/" target="_blank">GraphViz</a>.
Here is the code and resulting visualization:

![img/about-graphviz.png](img/about-graphviz.png)

Despite the fact that we learn enormously from visualization, because it isn't an
essential requirement in training or education, it comes as an afterthought. It's
also very hard to do, so it tends to be the case that visualization technology
lags behind methods to understand or produce knowledge. Penr

## Penrose

Now imagine that we had a language for these designs. We would want to be able to
say "This is set A, and this is set B, and here is the Intersect between sets A and B."
We would want this represented not only in the visual output, but also the 
code that we type to produce it. Penrose is software that provides two design specific
languages, or DSLs.

### Substance

The substance of a car might be frame and wheels. We don't care that much about
the color, seat material, or other aesthetics. Thus, you can think of a substance
domain as the core elements of what you want to describe. For Penrose, this
means objects in math.

> The Substance Domain Specific Language include high level declarations of mathematical objects.

### Style

But we *do* care about the color of the car, and whether it has cloth or leather seats! This is
where the Style domain comes in.

> The Style Domain Specific Language includes the visual presentation of the objects.


### An Example

**note: still working on this, maybe some parts wrong**

To go back to the simple sentence _This is set A, and this is set B, and here is the Intersect between sets A and B._
let's now look at how we might define this using Penrose Substance and Style languages. First let's
define Sets A and B, and their Intersects.

```
Set A, B
Intersect A B

```

Yep, Penrose understands what these things mean mathematically! And importantly,
so do you, just by looking at the text above. Now let's add our style preferences.
Here are some expected conditions for our objects:

```
Set A {
 shape = Circle{ }
}
Intersect X Y {
 ensure X overlapping Y
}
```

The above says that A should be a circle, and when we define an intersect between
two variables (such as A and B) we want to ensure the two are overlapping.

**still writing / learning**
