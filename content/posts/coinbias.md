---
title: "Different Flavours of Coin Tossing"
date: 2022-07-17T11:51:18+05:30
draft: true
tags: [total variation distance, hypothesis testing, tail inequality, bias estimation]
categories: [toolkit,toc]
---

# Introduction

something about three different problems

----
----

## Estimating the bias of a Coin

In this section we talk about the following problem:

> How many tosses are needed to test if a coin is biased?

We answer this question from a hypothesis testing perspective. To do that, we first introduce a basic hypothesis testing setup.

----

### Crash Course in Binary Hypothesis Testing w/ fixed priors

> One can skip this section if they are familiar with Hypothesis testing.

Let $X$ be the r.v. modeling the actual underlying event, and $Y$ be the r.v. modeling the observed event, s.t. $(X,Y)\sim P_{XY}$. The conditional distribution $P_{Y|X}(\cdot|x)$ is called a **channel**, and denoted as $W:X\mapsto Y$. For discrete $X,Y$, 
$$
W(y|x) = P_{Y|X}(Y=y|X=x) = \frac{P_{XY}(X=x,Y=y)}{P_X(X=x)}.
$$

The goal is to determine $x$ by observing $Y\sim W_x$. This is known as the Statistical Inference Problem. Here we make the following two assumptions:

> **Bayesian:** X is drawn from a fixed prior distribution $P_X$.

> **Binary:** $|X|=2$. 

We have two hypotheses at this point:

- $H_0: Y\sim W_0$, known as the Null hypothesis.
- $H_1: Y\sim W_1$, known as the Alternate hypothesis.

Upon observing $Y\sim W_x$, we form an estimate $\hat{X}=g(Y)$. Here $g(\cdot)$ is known as a **test**. We wish to minimize the error of our test, i.e., we wish to minimize the probability
$$
P(X\neq\hat{X}) = P_X(0)\cdot P(g(Y)=1|0) + P_X(1)\cdot P(g(Y)=0|1).
$$

Let $A_0=\{y:g(y)=0\}$. Then the above probability becomes,
$$
P(X\neq\hat{X}) = P_X(0)\sum_{y\in A_0^c} W_0(y) + P_X(1)\sum_{y\in A_0} W_1(y).
$$

For a uniform prior, i.e., $P_X=\mathcal{U}(0,1)$, the probability of error becomes
$$
P(X\neq\hat{X}) = \frac{1}{2}-\frac{1}{2}\left[W_0(A_0)-W_1(A_0)\right].
$$

This probability is minimized when $A_{opt}=\{y:W_0(y)\geq W_1(y)\}$, which allows us to express the minimum probability of error in terms of the **Total variation distance** between the channels $W_0$ and $W_1$, as
$$
P_{\mathrm{opt}}^{\mathcal{U}(0,1)} = \frac{1}{2}\left[1-\mathrm{TV}(W_0(y),W_1(y))\right].
$$

----

### Testing the Bias of a Coin

Suppose $X$ is an r.v. that takes values in the set $\{ \frac{1}{2},\frac{1}{2}+\varepsilon\}$. Let $Y\in\{0,1\}$ correspond to observations of coin tosses where $0$ corresponds to Heads, and $1$ corresponds to Tails.
We can now formulate our hypotheses as follows:

- $H_0: Y\sim W_0=P=\mathrm{Ber}(\frac{1}{2})$.
- $H_0: Y\sim W_1=Q=\mathrm{Ber}(\frac{1}{2})+\varepsilon$.

> **Goal:** What is the number of observations $n$, s.t. we can use $Y_1,\ldots,Y_n$, to distinguish between $W_0$ and $W_1$ w.h.p.?

Let $P^n$ and $Q^n$ be the $n$-fold product distributions on $Y_1,\ldots,Y_n$. Then 
$$
P_{\mathrm{opt}}^{\mathcal{U}(0,1)} = \frac{1}{2}\left[1-\mathrm{TV}(P^n,Q^n)\right].
$$
Note that $\mathrm{TV}(P^n,Q^n)\leq \sum_{i=1}^{n} \mathrm{TV}(P_i,Q_i)\leq n\varepsilon$. If we require $P_{\mathrm{opt}}^{\mathcal{U}(0,1)}\geq \frac{2}{3}$, then $$