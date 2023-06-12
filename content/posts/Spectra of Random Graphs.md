---
title: "Spectra of Random Graphs"
date: 2023-06-11T11:51:18+05:30
draft: true
tags: [tcs, spectal, random-graphs]
categories: [pedagogy]
---
# Basics of Spectral Graph Theory
## Laplacian
## Adjacency Matrix
### Eigenvalues of Adjacency Matrix

# Erdos-Renyi random graphs
- The ER graph model $G(n,p)$ or $G_{n,p}$ is a family of $n$-vertex graphs  where each edge is picked i.i.d. with probability $p$. 
- We want to study the properties of the adjacency matrix of $G\sim G(n,p)$, i.e., any arbitrary $G$ drawn from $G(n,p)$.



## Structure of $G(n,p)$ for large $n$
- $p = \frac{\alpha}{n}$ for $\alpha<1$. All components have small size $O(\log n)$, mostly trees.
- $p = \frac{1}{n}$ for $\alpha<1$. Largest component has size on the order of $n^{2/3}$.
- $p = \frac{\alpha}{n}$ for $\alpha>1$. One **giant component** of linear size. Other components have small size $O(\log n)$, mostly trees.

**Known facts about $G\sim G(n,p)$:**
- These graphs have one large eigenvalue around $p\cdot n$.
- All other eigenvalues have absolute at most $(1 + o(1))\cdot 2\sqrt{p(1 − p)n}$.
- Spectra of $G(n,\frac{\alpha}{n})$ is **continuous** (giant component) **+ discrete** (trees + isolated vertices).

