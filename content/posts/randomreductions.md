---
title: "Randomized Reductions and Random Self-reducibility"
date: 2025-03-01T11:51:18+05:30
draft: true
tags:
  [
    randomization,
    randomness,
    reductions,
    p-np,
    self-reducibility,
    worst-to-average case,
    BPP,
    pseudrandomness,
    permanent,
    discrete log,
  ]
categories: [toc, complexity]
---

# Introduction

https://www.csa.iisc.ac.in/~chandan/courses/complexity14/notes/lec21.pdf

https://people.seas.harvard.edu/~salil/cs221/fall02/scribenotes/nov25.pdf

https://lance.fortnow.com/papers/files/rsr.pdf

https://people.seas.harvard.edu/~salil/cs221/spring10/lec13.pdf

https://www.cs.uml.edu/~wang/acc-forum/avg/avgcomp/node12.html

https://cstheory.stackexchange.com/questions/20020/on-random-self-reducible-properties

http://cs-www.cs.yale.edu/homes/jf/FF.pdf

## Worst-Case to Average-Case Reductions

## Randomized Reductions

## Random-self-reducibility

http://cs-www.cs.yale.edu/homes/jf/FF.pdf

https://arxiv.org/pdf/2412.18134v1

Self-testing and
self-correcting allow one to use a program Π to compute a function 𝑓 without trusting that Π
works correctly for all inputs. The computed function 𝑓 can be seen as a formal mathematical
specification of what the program Π should compute. Two key components of this formalism are: a
self-tester and a self-corrector. A self-tester for 𝑓 estimates the fraction of values of 𝑥 for which
indeed it holds Π(𝑥) = 𝑓 (𝑥). A self-corrector for 𝑓 takes a program that is correct on most inputs
and turns it into a program that is correct on every input with high probability. Both self-testers
and self-correctors have only black-box access to Π and, importantly, do not directly compute 𝑓

> Self-correctors exist for any function that is randomly self-reducible.

### Adaptive and non-adaptive

### DLog is r.s.r.

### Permanent is r.s.r.

> f f is #P-complete, then f is poly-rsr.

## Pseudorandom-self-reducibility

file:///C:/Users/sagni/Downloads/TR21-170.pdf
