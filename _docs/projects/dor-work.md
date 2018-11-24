---
title: Dor's Work
category: Projects
order: 1
---

This document covers my work progress through my working on Penrose through summer 2018.

### What I did so far?
* Wrote a visual spec for the linear algebra domain
* Added new shapes and visualisation options to the system (Issues 63, 64, 66)
* Wrote DSLL program for describing the set theory domain (book...)
* Implemented a DSLL parser
* Implemented a new substance Parser and integrated it to the system
* Implemented a DSLL typechecker
* Implemented a Substance typechecker
* Currently, Implement the linear algebra domain


### The DSLL Parser and Type Checking
The DSLL parser currently parse a DSLL program which describe a specific domain and translate it to an AST. Then, semantic checks according to the rules in the design paper are preformed. The DSLL parser is now integrated as part of the system and you can't run it without a given DSLL for the desired domain.

### The Substance Parser and Type Checking
The Substance parser parse any Substance Program which follow the Substance new grammar. Then, it uses the environment which is passed from the DSLL type checking (which contains all the declarations of types, operations and predicates of the specific domain) and preforms a type checking using that environment. <br/>
Note that parts of the Substance and DSLL AST, grammar and type checking are in fact common and you can find those parts in `Env.hs` together with the environment definition which is both for the DSLL and Substance type checking

### Running the System
In order to run the system now you should specify 3 parameters, for example:
`./runpenrose snap sub/continuousmap.sub sty/continuousmap.sty dsll/setTheory.dsl`
which are:
1. The Substance program
2. The Style program
3. The DSLL program for the particular domain

These instructions refer to the `Dor-DSLL` branch which has not merged to the master yet

### Near Future TODOs:
1. Add line numbers to the type checker errors.
2. Implement the linear algebra domain


### The new Substance Parser - Parity Check
I wrote a new substance parser which parse the substance core language grammar.
Running the system with the new parser is exactly the same as it was before

**Parity Check:**

|Program        | Old Substance  | New Substance |
| ------------- | -------------  | ------------- |
|`./runpenrose snap sub/tree.sub sty/venn.sty dsll/setTheory.dsl`               | <img src="assets/Examples-098cd.png" width=300px>   | <img width=300px alt="image" src="https://user-images.githubusercontent.com/19957447/40058509-56012b3a-581f-11e8-98da-1a0eda088c8d.png">|
| `./runpenrose snap sub/tree.sub sty/tree.sty dsll/setTheory.dsl` | <img src="assets/Examples-ef85b.png" width=300px>| <img width="141" alt="image" src="https://user-images.githubusercontent.com/19957447/40058630-bad5e7e4-581f-11e8-99ab-c8bcf4e37fe1.png">|
| `./runpenrose snap sub/continuousmap.sub sty/continuousmap.sty dsll/setTheory.dsl` | <img src="assets/Examples-266ff.png" width=300px>| <img width= 300px alt="image" src="https://user-images.githubusercontent.com/19957447/40058764-267b1fdc-5820-11e8-989d-a2bf9705a66b.png">|
| `./runpenrose snap sub/surjection_computed.sub sty/surjection_computed.sty dsll/setTheory.dsl` | <img src="assets/surjection.png" width=300px> | <img width= 300px alt="image" src="https://user-images.githubusercontent.com/19957447/40058869-7efc720a-5820-11e8-9aa1-295c95e3367d.png">|
| `./runpenrose snap sub/cart-test.sub sty/cart-test-pt.sty dsll/setTheory.dsl` | <img src="assets/Examples-8b931.png" width=300px>|<img width=300px alt="image" src="https://user-images.githubusercontent.com/19957447/40121754-4e5d423e-58f0-11e8-874a-894bb6ac9161.png">|
| `./runpenrose snap sub/cart-test-pt.sub sty/cart-test-pt.sty dsll/setTheory.dsl` | <img src="https://i.imgur.com/X9CvAod.png" width=250px>|<img width= 250px alt="image" src="https://user-images.githubusercontent.com/19957447/40121982-ccce590a-58f0-11e8-8145-3c74eb363ab1.png">|

**The Linear Algebra Domain**
After working implementing the DSLL and Substance parsers and type checkers and integrating them into the system I started to implement the linear algebra domain as a new domain for penrose.

**Known issues & important points:**
1. We do not have any syntactic sugars and the syntax is a little bit different from the old syntax, for example, `continuousmap.sub` syntax was:
```
Set A
Set B
Set R1
Set R2
Subset A R1
Subset B R2
NoIntersect R1 R2
f: A -> B
Set U
Subset U B
Set V
Subset V A
```

and now the syntax is:

```
Set A
Set B
Set R1
Set R2
Subset(A,R1)
Subset(B,R2)
NoIntersect(R1,R2)
Map(A,B) f
Set U
Subset(U,B)
Set V
Subset(V,A)
```

2. The new grammar does not support substance definitions and FOL operations, so we don't support programs like this:
```f: A -> B
Set A 
Set B
Definition Surjection(Map f, Set X, Set Y):
    forall y : Y | exists x : X | f(x) = y
Surjection(f, A, B)
```
3. The tests for the systems does not support the new mechanism of the DSLL and will not work now
