---
title: Docstrings with Haddock
category: Penrose Development
order: 8
---

## Documenting Penrose using Haddock
Penrose uses [haddock](https://www.haskell.org/haddock/doc/html/index.html) to automatically generate documentation. We follow Haddock's specific [markup language](https://www.haskell.org/haddock/doc/html/ch03s08.html#idm140354810796304) when documenting our code.

## Generating HTML Documentation Site
- If you use `cabal`: 
```shell
cabal haddock --hyperlink-source --executables  
              --html-location='http://hackage.haskell.org/package/penrose/docs'  
              --haddock-options=--no-print-missing-docs
```
  - If you use `stack`: Note that you have to make sure `src` is clean and in sync with `upstream` because it will parse all `.hs` files in `src` even if they are untracked. You can run the following command or execute [`script/mkdoc.sh`](https://github.com/penrose/penrose/blob/master/scripts/mkdoc.sh)
```shell
stack exec -- haddock --html src/*.hs  --hyperlinked-source --odir=dist/docs
```
- After the generation, `haddock` would report a path to `index.html` of the documentation site relative to `TOP`
- By default, all top-level functions will appear in the documentation page. Adding `{-# OPTIONS_HADDOCK prune #-}` will hide undocumented ones and only show documented ones.


