---
title: "Hoffman's Coloring Lower Bound"
date: 2023-07-11T12:04:49+05:30
draft: true
tags:
  [
    tcs,
    spectral-graph-theory,
    chromatic-number,
    graph-coloring,
    eigenvalue,
    adjacency-matrix,
    pedagogy,
  ]
categories: [pedagogy]
---

# Introduction
In this post we will be deriving a lower bound for the Chromatic Number $\chi(G)$ of a general graph $G$ in terms of the eigenvalues of the adjacency matrix $M$ of $G$. More precisely, let the eigenvalues of $M$ be $\mu_{1\geq}\mu_2\geq\ldots\mu_n$. Then, th

### Theorem: Hoffman's Bound
$$
\chi(G)\geq 1-\frac{\mu_1}{\mu_n}
$$


# References

