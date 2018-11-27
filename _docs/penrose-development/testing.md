---
title: Testing
category: Penrose Development
order: 5
---

### Testing the system

In the top-level directory (referred to as TOP), run either `stack test [--profile]` or `sudo cabal test`. 

You can also run the test suite partially by providing a string pattern to the test suite's executable (if you use `sudo cabal test`, the executable is located at `TOP/dist/build/penrose-testsuite/penrose-testsuite`. For example,

```
$ dist/build/penrose-testsuite/penrose-testsuite -p "Style"
```
will run all test groups with string identifiers that contain the pattern "Style". You can also use awk expressions, too.

Issue tracker: [#8](https://github.com/penrose/penrose/issues/8)

### Test structure

We use `tasty` for tests, which supports both property-based testing (QuickCheck and SmallCheck) and unit tests (HUnit). `tasty` also has more extensions that you can try out.

Tests are located in `TOP/test`. `TestSuite` contains the tests for the system. There is one test file for each file in the system, located in the corresponding directory. 

So we can test main-esque functions by importing main-esque modules for the testing libraries (which have their own Main module), the Penrose `Main` module has been split into `Main` and `ShadowMain`. Testable main-esque logic should go in `ShadowMain`.

Directory and dependency structure are tracked in `penrose.cabal`, which `stack` depends on to find the right modules (Note that the test suite has a separate set of dependency structure from the main system. Please make sure you update `build-depends` of the test suite if you want to include a new package in the tests).

Code coverage doesn't yet work with our system (`stack test --coverage`).

See [examples](https://github.com/penrose/penrose/wiki/Examples) for the end-to-end examples and regression tests that should work in our system.

### Adding regression tests

Important examples that should work, and not stop working, are listed on this [wiki page](https://github.com/penrose/penrose/wiki/Examples).

Regression tests can be found in `test/ShadowMain/Tests.hs`. To add a regression test, do two things:

* Add a `TestInfo` for your test, consisting of a test name, Substance file, Style file, and ground truth initial and final states (see `twopoints_computed` for an example). These initial and final states can be obtained by copying output from the system.
* Add the name of your test to the list `all_test_info`. 

Then the function `genTests` will generate two tests for each entry in `all_test_info`, one for the initial state and one for the final state. It does so by running a serverless version of the main in `ShadowMain`, comprising `mainRetInit` (which returns the initial state) and `mainRetFinal` (which returns the final state).

NOTE: the function `mainRetInit` reads files given their filenames, so its output state is in the IO monad. `genTests` uses `unsafePerformIO` to extract the state from the IO monad. I think this is okay because we're only using it for test purposes.

### Writing good tests

You can see some of the existing test cases for examples of how to write property-based tests and unit tests. Try to prefer property-based testing over unit tests, and try to write tests as documentation. You can even start by writing tests before you write your code!

Note that QuickCheck is randomized property-based testing and SmallCheck is exhaustive, so the latter may not terminate if you give it a type like `Float`.

Test style is TBD as we write more tests.
