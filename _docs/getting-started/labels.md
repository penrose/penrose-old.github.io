---
title: Labels
category: Getting Started
order: 3
---

Related issues: #59 

This page describes a labeling language, so Substance variables can be named in ASCII and given TeX labels, e.g.
```
Set A, B
f: A -> B
AutoLabel A, B
Label f $\sum_i^n \frac{x^2}{10}$
```
## Label DSL spec

* We support three kinds of statements in our labeling DSL: 
  * `Nolabel xs`: forbids Substance objects from having labels. `xs` is a comma separated list of Substance identifiers.
  * `Label x s`: normal declaration of labels. Gives a Substance object with identifier `x` a label, where the content of the label is specified by `s`. `s` is a TeX expression in math environment (`"$f$"`) 
  * `Autolabel xs`: automatically generates labels for all identifiers in `xs`. By default, a Substance id `a` will be translated into TeX math environment as `$a$`. 
    * Special keyword `All` can replace `xs`, which just refers to all Substance ids. 
* The semantics of label statements is determined by the ordering of them in the source file. 
  * If we have the following, `A` will end up not having any label. 
  ```
  Label A $AA$
  AutoLabel A
  NoLabel A
  ```
  
* In the system, we would add the two kinds of label declarations (manual vs. auto labeling) to the options of Substance statements.
```haskell
data SubStmt = {- all other Substance stmts -} | LabelDecl String String | AutoLabel [String]
```
*  __As a result, label statements can be interleaved with other Substance statements__. Thus, the following style is also supported:
```
Set A, B
AutoLabel A, B

f: A -> B
Label f $\sum_i^n \frac{x^2}{10}$
```
* In Style, a labeled Substance object, `X`,  will have a __field__ `X.label`, which contains the label string _without the enclosing `$$`_. The Style writer can subsequently declare new GPIs that use the `label` field as their contents:
```
Set A {
  A.test = Text {
      string = concat(A.label, "_1")
  }
}
```
* The string in `string` property of Text GPI get rendered in TeX's math environment. If normal text is needed, please use `\textrm` to escape from the math environment. For instance, `Label a $\textrm{hello world }\Delta$ is rendered to:

![](https://user-images.githubusercontent.com/11740102/42915181-4f61a5ae-8acc-11e8-894a-aae3ffa6b9ed.png)
* Label statements can be interleaved with other Substance statements

## Implementation strategy and design decisions

- On the highest level, all labels are converted to SVG strings by MathJax __the first time the scene is rendered__ and will then be kept inside of a global dictionary called `label_svgs` indexed by `uniqueShapeName`
- Everytime `Render.scene` is called, `_renderLabel` will parse the strings into SVG fragments using Snap and place them onto canvas
- MathJax has an asynchronous API and requires queuing its "Queue" with typesetting requests. Therefore, I used Promises, `await` and `async` for all related functions (see `tex2svg` and `_collectLabels` for details)

# Examples
- `penrose sub/twosets.sub  sty/venn.sty  dsll/setTheory.dsl`
![latex](https://user-images.githubusercontent.com/11740102/41574120-104c10ec-734e-11e8-9738-1ec6cf6a4e5e.gif)
- `penrose sub/continuousmap.sub  sty/continuousmap.sty  dsll/setTheory.dsl`
![image](https://user-images.githubusercontent.com/11740102/41693128-e644e32a-74d1-11e8-9cdd-6102fe2f9d36.png)
- `penrose sub/surjection_computed.sub  sty/surjection_computed.sty dsll/setTheory.dsl`
![image](https://user-images.githubusercontent.com/11740102/41693539-c230aeea-74d3-11e8-9c07-dfe312cf8f98.png)

# Open questions
Questions that require more discussion or to be addressed in future development:
- Gaining access to Bezier curves
- To solve this, we first have to know the best way to represent Bezier curves in our Runtime? Currently, we only represent them as a list of point, which could be insufficient for optimization purposes 
- We still need to parse the SVGs of TeX labels back into our Runtime representation in order to optimize them. Here's an SVG parser library I found: [svg-parser](https://github.com/elaye/svg-parser).

* In Style, how to design a mechanism for “don’t make a labeling objective in Style if A does not have a label”? Below are possible solutions:
  * `if x.labeled`, where `labeled` is a built-in field in a Substance object
  ```
  Set A {
    if A.labeled: labelFn = encourage -- some objective
  }
  ```
  * A built-in unary predicate `Labeled` in the core Substance language, so that we can write selectors for them:
  ```
  Set A 
  where Labeled(A) {    labelFn = encourage -- some objective }
  ```
