---
title: "An Introduction to Learning Theory"
date: 
tags: [learning-theory, tcs]
category: [pedagogy]
draft: true
---

# Introduction

Let's say we have black-box/oracle access to a Boolean function $f$ on $n$ variables $f:\{0,1\}^{n}\xrightarrow{}\{0,1\}$. We are also given a set $S$ **labeled examples** in the form of $n$-bit strings (*read:* instances) and the output of $f$ on each such string (*read:* labels).

> **Learning $f$:** Find a *good approximation* $f^\prime$ of $f$ *efficiently*.


There's a lot to unpack here. To define learnability, we have to first rigorously define what we mean by **efficient** and **good approximation**. To do this we take a page out of Valiant's [^1] definitions and expand and build on them. 

Before proceeding further, we note that the choice of $n$-bit Boolean functions for $f$ was arbitrary. We can in fact extend the definition of $f$ to other types of functions with different domains and ranges, and even relations. However, for the time being, $n$-bit Boolean functions will suffice to elucidate the core concepts about learning.

## Learnability from Examples

Consider a family of $n$-bit Boolean functions $\mathcal{C}$ which we will refer to henceforth as the **concept class**. Our goal will be to *learn* a **concept** (an $n$-bit Boolean function belonging to the concept class) $c\in \mathcal{C}$.

Let the domain of the **instances** (the $n$-bit strings) be denoted as $\mathcal{X}\subseteq \{0,1\}^n$. Since the concept $c$ is a Boolean function, the **labels** (the range of the concept) will be simply be a binary set $\{0,1\}$ or $\{-1,1\}$ depending on the context.

A **learning algorithm** $L$ will be provided a set $S$ of $m$ **labeled examples** (tuples containing instances and their corresponding labels) $(x,c(x))$, where the instances will be drawn according to an unknown arbitrary but fixed distribution $\mathcal{D}$ over $\mathcal{X}$. $L$ will then produce a hypothesis $h$ (which may or may not belong to $\mathcal{C}$) using the set $S$.

If $L$ uses relatively few examples, i.e., $m=\mathrm{poly}(n)$, and runs in time $\mathrm{poly}(n)$, to produce $h$ s.t.
$$
\mathrm{Pr}_{D}\left[h(x)\neq c(x)\right]\leq \varepsilon
$$
then, we 

[^1]: L. G. Valiant. 1984. A theory of the learnable. Commun. ACM 27, 11 (Nov. 1984), 1134–1142. https://doi.org/10.1145/1968.1972

