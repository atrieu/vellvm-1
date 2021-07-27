
[comment]: # (this is a markdown comment! I think it must be after a blank line)

# FOR ARTIFACT EVALUATION

The description below has been augmented with comments that indicate the
relevant parts of the ICFP paper.  We've called them out using by marking them
with **ARTIFACT** so they are easier to find.  We also list the specific claims
in the paper and their locations in the development below:

### Definitions / Lemmas 

- [PropT laws from Figure 7](src/coq/Utils/PropT.v) search for comments including "Figure 7"
- [laws from Figure 8](src/coq/Utils/PropT.v) search for comments including "Figure 8"
- [Definition 5.1](src/coq/Utils/PropT.v) search for "Definition 5.1"
- [Definition 5.2](src/coq/Utils/PropT.v) search for "Definition 5.2"
- [Lemma 5.4](src/coq/Utils/PropT.v) search for "Lemma 5.4"
- [Lemma 5.5](src/coq/Utils/PropT.v) search for "Lemma 5.5"
- [Definition 5.6](src/coq/Theory/Refinement.v) search for "Definition 5.6"
- [Lemma 5.7](src/coq/Theory/TopLevelRefinements.v) search for "Lemma 5.7" (see related definitions in [Refinement.v](src/coq/Theory/Refinement.v)
- [Lemma 5.8](src/coq/Theory/TopLevelRefinements.v) search for "Lemma 5.8"

### Unit Tests

After compiling vellvm the development, running `src/vellvm --test` should successfully run 145 unit tests.  Running
`src/vellvm --test-file tests/helix/test_dynwin64.ll` should successfully run the HELIX test case mentioned in Section 7 of the paper.

### QuickChick Tests

The QuickChick tests can be run using the command `make qc-tests` in
the source directory. This test generates random LLVM programs that
are then run with the Vellvm interpreter and the `clang` compiler
(which must also be installed).

[comment]: # (TODO: Calvin add something here)


### HELIX Case Study

The paper's Section 6 refers to our use of this library for verifying a
front-end for the Helix tool chain.  That development is available in a separate
repository [Helix](https://github.com/vzaliva/helix) and it relies on a slightly
older version of Vellvm (Helix requires MetaCoq which isn't compatible yet with
coq v. 8.13) that was current at the time the paper was submitted. 
Since the Helix development points to another specific version of this repository and
is still under active development, we chose to not include it in this artifact. 
The interested reviewer can however get it and run it directly from its Github.

## Getting started

There are two options: build Vellvm from scratch in your own environment, or
use the virtual machine image we provide.

### Building in your own environment
See [Installing and Compiling Vellvm](#installing-and-compiling-vellvm).

### Using the virtual machine image
The Debian QEmu image has been packaged into `vellvm.tar.gz`. 
Run the image with `start.sh` for Unix-like systems (you might need `sudo` for 
Linux) and `start.bat` for Windows.

 * Username: artifact
 * Password: password

If the VM does not start, check `Debugging.md` provided in the directory.

See [Installing and Compiling Vellvm](#installing-and-compiling-vellvm) to 
compile and run the project.

# Vellvm
[![Build Status](https://travis-ci.com/vellvm/vellvm.svg?branch=master)](https://travis-ci.com/vellvm/vellvm)

Vellvm is an ongoing project aiming at the formal verification in the Coq proof
assistant of a compilation infrastructure inspired by the LLVM compiler.

The central piece of Vellvm is the Verified IR (VIR), a Coq formalization of the
semantics of (a subset of) the LLVM IR that is intended for _formal
verification_ of LLVM-based software.
It is being developed at the University of Pennsylvania as part of the DeepSpec project.

After VIR, the second component whose development is reaching maturity is the verification of 
a verified front-end for the [Helix](https://github.com/vzaliva/helix) chain of compilation.

[comment]: # (TODO: delete names for artifact evaluation)


### See also:
 - [Vellvm](http://www.cis.upenn.edu/~stevez/vellvm/) (somewhat out of date)
 - [DeepSpec](http://deepspec.org)
 - [LLVM](http://llvm.org)

# Participants
 - Steve Zdancewic
 - Yannick Zakowski
 - Calvin Beck
 - Irene Yoon
 
## Past Contributors
 - Vivien Durey 
 - Dmitri Garbuzov 
 - Olek Gierczak
 - William Mansky
 - Milo Martin
 - Santosh Nagarakatte 
 - Emmett Neyman 
 - Christine Rizkallah 
 - Robert Zajac
 - Richard Zhang 
 - Jianzhou Zhao

---

# Structure of the development

The development is organized as follows.

## Local libraries

Stored in the `lib` folder. Currently, the only dependency that needs to be installed locally is the QuickChick one:
- `lib/QuickChick` points to a branch of the QuickChick library that we have used in our testing

The library will be built as the same time as the Vellvm development via the `Makefile`.

## Interaction Trees

**ARTIFACT**
Although we have made significant contributions to the itree library for the sake of this project, most of it
is orthogonal to the material described in this paper, and can be treated entirely as an external library to understand this project.

The content of Section 5.1 that gives a taste of how the underlying equational theory works can be found in more details in
the files [Eq.v](https://github.com/DeepSpec/InteractionTrees/blob/master/theories/Eq/Eq.v) and [UpToTaus.v](https://github.com/DeepSpec/InteractionTrees/blob/master/theories/Eq/UpToTaus.v) in the [Interaction Trees github repo](https://github.com/DeepSpec/InteractionTrees)

## Coq formalization

The core of the project is encompassed by the Coq formalization of LLVM IR and the proof of its metatheory. 
This formalization is entirely contained in the `src/coq` folder. 

More specifically, the following selection of files are among the most important to understand the development:

Syntax, in `src/coq/Syntax/`
- `LLVMAst.v` the front VIR internal AST. Our parser of native llvm syntax returns an element of this AST.
- `CFG.v`     the VIR internal AST as used for the semantics. 

Semantics, in `src/coq/Semantics/`:
- `DynamicValues.v` definition of the dynamic values and underdefined values **ARTIFACT** discussed in Section 2.2.
- `LLVMEvents.v`    inventory of all events **ARTIFACT** as described in Section 4.1.
- `Denotation.v`    definitions of the representation of VIR programs as ITrees **ARTIFACT** as described in Section 4.2.
- `Handlers/`       includes the code for all of the handlers **ARTIFACT** described in Section 4.3. They are broken up into files based on the nature of the event handled, each file hence corresponding to a subsection.
- `TopLevel.v`      provides the full model and the full interpreter, by plugging all components together, **ARTIFACT** i.e. the final result of Section 4.4.

Theory, in `src/coq/Theory/`:
- `src/coq/Utils/PropT.v` metatheory required to reason about sets of itrees, i.e. about the propositional monad transformer.
- `InterpreterMCFG.v`     the layers of interpretation **ARTIFACT** shown in Figure 6 and some of their metatheory
- `InterpreterCFG.v`      the same layers of interpretation and metatheory, but when reasoning about single functions (i.e. single cfg)
- `Refinement.v`          definition of the refinement relations between layers of interpretations **ARTIFACT** mentioned in Section 5.4
- `TopLevelRefinements.v` proof of inclusion of the refinement relations between layers of interpretations; proof of soundness of the interpreter as described in Section 5
- `DenotationTheory`      Equational theory to reason directly about the structure of vir programs; in particular, reasoning principles about open control-flow-graphs.

## OCaml front-end and driver for execution and testing

On the OCaml side, we provide a parser for legal LLVM IR syntax as well as an
infrastructure to run differential tests between our interpreter and llc.
These unverified parts of the development live in the `src/ml` folder.

- `extracted/Extract.v`    instructions for the extraction of the development to OCaml
- `libvellvm/interpreter.ml`  OCaml driver for running the interpreter; the `step` function walks over the ITree that remains after complete interpretation of the denotation of a program
- `libvellvm/llvm_parser.mly` the parser, adapted from Vellvm, **ARTIFACT** as discussed in Section 4.5.
- `testing/assertion.ml`   custom annotations of llvm programs as comments used to define our tests.
- `main.ml`                top-level from which our executable is obtained.

## Test suite

Our current test-suite of LLVM programs for which we compare our semantics against llc is stored in `tests/`

- `tests/` directory containing the test suite of LLVM IR programs discussed in Section 6

# Installing / Compiling Vellvm

### Assumes: 
  - Coq: version 8.13 (installed via *opam*, see below)
  - OCaml: version 4.12.0 (installed via *opam*, see below)
  - opam: version 2.0.5.
    It is available via [homebrew](https://brew.sh/) on Mac, and most system's package managers on Linux, e.g. `sudo apt-get install opam`.
    If this is the first time you are using opam you need to initialize it: 
    + On Linux: `opam init`
    + On Mac:  `opam init --disable-sandboxing` (sandboxing needs to be disabled due to a known [issue](https://github.com/ocaml/opam-repository/issues/12973)).
- Add the Coq package repository:
    `opam repo add coq-released https://coq.inria.fr/opam/released`.
- Finally, create an opam *switch* with:
    `opam switch create vellvm ocaml-base-compiler.4.12.0`.
  - Clang 7.0.1+ (available for Mac OSX in XCode 4.2+, or installed via, e.g. `sudo apt-get install clang`)
  - External Coq libraries: 
    * ext-lib    (installed via, e.g. `opam install coq-ext-lib`)
    * paco       (installed via, e.g. `opam install coq-paco`)
    * itrees     (installed via, e.g. `opam install coq-itrees`)
    * flocq      (installed via, e.g. `opam install coq-flocq`) 
    * ceres      (installed via, e.g. `opam install coq-ceres`)
    * mathcomp   (installed via, e.g. `opam install coq-mathcomp-ssreflect`)
    * simple-io  (installed via, e.g. `opam install coq-simple-io`)

  - Additional opam packages: 
    * ocamlbuild (installed via, e.g. `opam install ocamlbuild`)
    * dune       (installed via, e.g. `opam install dune`)
    * menhir     (installed via, e.g. `opam install menhir`)
    * qcheck     (installed via, e.g. `opam install qcheck`)

Compilation:

1. Clone the vellvm git repo with the `--recurse-submodule` option
2. Install all external dependencies
   - Note: you should be able to install all of the opam libraries  with `make opam` in the `src/` directory.
3. Run `make` in the `src/` directory: it will first compile the quickchick library, then vir, and finally extract the OCaml executable

# Running

The executable `vellvm` will be found in `src/`.
Do `src/vellvm -help` from the command line to see all available options.
In particular:
- `src/vellvm -interpret tests/ll/factorial.ll` to run the interpreter on a given file.
- `cd src && ./vellvm --test` to run the test suite against llc
- `src/vellvm --test-file tests/ll/gep2.ll` to test a specific file using inlined assertions


# Adding a new test case

One way to create new test cases for Vellvm is to compile a C program using `clang` and then add assertions to turn it in to an executable for adding assertions.  The steps below illustrate this process:

1. Create a C program (e.g. in the directory `tests/c`), for instance, `example.c`.

 - The C program should not use C libraries (yet!), which are not part of the LLVM IR standard, but the program should be able to use general C language features.

For example: the following C program contains a simple function called `foo` that multiplies its input by 3:

```
int foo(int x) {
  return 3*x;
}
```

2. Compile the C program using `clang` with the `-emit-llvm` and `-S` flags to generate an LLVM `.ll` version.

```
~/vellvm/tests/c> clang -S -emit-llvm example.c
```

3. The resulting `.ll` file should look something like this:
~/vellvm/tests/c> cat example.ll
; ModuleID = 'example.c'
source_filename = "example.c"
target datalayout = "e-m:o-i64:64-f80:128-n8:16:32:64-S128"
target triple = "x86_64-apple-macosx10.15.0"

; Function Attrs: noinline nounwind optnone ssp uwtable
define i32 @foo(i32) #0 {
  %2 = alloca i32, align 4
  store i32 %0, i32* %2, align 4
  %3 = load i32, i32* %2, align 4
  %4 = mul nsw i32 3, %3
  ret i32 %4
}

attributes #0 = { noinline nounwind optnone ssp uwtable "correctly-rounded-divide-sqrt-fp-math"="false" "darwin-stkchk-strong-link" "disable-tail-calls"="false" "less-precise-fpmad"="false" "min-legal-vector-width"="0" "no-frame-pointer-elim"="true" "no-frame-pointer-elim-non-leaf" "no-infs-fp-math"="false" "no-jump-tables"="false" "no-nans-fp-math"="false" "no-signed-zeros-fp-math"="false" "no-trapping-math"="false" "probe-stack"="___chkstk_darwin" "stack-protector-buffer-size"="8" "target-cpu"="penryn" "target-features"="+cx16,+fxsr,+mmx,+sahf,+sse,+sse2,+sse3,+sse4.1,+ssse3,+x87" "unsafe-fp-math"="false" "use-soft-float"="false" }

!llvm.module.flags = !{!0, !1, !2}
!llvm.ident = !{!3}

!0 = !{i32 2, !"SDK Version", [2 x i32] [i32 10, i32 15]}
!1 = !{i32 1, !"wchar_size", i32 4}
!2 = !{i32 7, !"PIC Level", i32 2}
!3 = !{!"Apple clang version 11.0.0 (clang-1100.0.33.16)"}
```

4. Edit the `.ll` file to add some assertions about the behavior of the program.  For example, we could add the following three assertions (the last of which is actually incorrect):

```
; ASSERT EQ: i32 0 = call i32 @foo(i32 0)
; ASSERT EQ: i32 3 = call i32 @foo(i32 1)
; ASSERT EQ: i32 5 = call i32 @foo(i32 2)
```

5. Run vellvm with the `--test-file example.ll` flags to see the results of running the test cases:

```
~/vellvm/tests/c> ../../src/vellvm --test-file example.ll
(* -------- Vellvm Test Harness -------- *)

example:
  passed - UVALUE_I32(0) = foo(UVALUE_I32(0))
  passed - UVALUE_I32(3) = foo(UVALUE_I32(1))
  failed - UVALUE_I32(5) = foo(UVALUE_I32(2))
	   ERROR: not equal
(*-------------------------------------- *)
Passed: 2/3
Failed: 1/3
```