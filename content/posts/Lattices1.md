---
title: "Notes on Generating Hard Instances of the
Short Basis Problem [Ajtai'99] (Part 1)"
date: 2023-04-01T12:03:46+05:30
draft: false
tags: [tcs, lattices]
categories: [pedagogy]
---
# Hardness of problems
Computational hardness usually revolves around problems with **worst-case hardness guarantees**, while cryptographic applications require problems with **average-case hardness guarantees**. An intuitive reasoning for this would be as follows: 
- Cryptographic schemes require security guarantees for random keys, but it is not immediately apparent how to create hard instances of problems with worst-case hardness guarantees. 
- On the other hand, any random instance of a problem with average-case hardness guarantee is hard with a positive probability, which immediately lends itself to applications in cryptography.

## Lattice-based Cryptography
Ajtai'96 [^ajtai96] showed reductions from **lattice based problems** with *average-case hardness* guarantees to lattice problems with *worst-case guarantees*. This immediately makes lattices an extremely important tool in cryptography. In fact, lattice based cryptographic constructions are invaluable for their many potential advantages as follows:
1. Lattice-based schemes usually only require linear operations on integers which leads to asymptotic efficiency.
2. Lattice-based schemes have been shown to be resistant to cryptanalysis by quantum algorithms unlike current cryptographic schemes which are based on factoring or discrete log (Shor'95 [^shor95]). This makes lattice-based cryptography the cornerstone of post-quantum cryptography.
3. As noted earlier, random instances of lattice based constructions are “as hard as possible”, which lends itself to conceptual simplicity while designing cryptographic schemes.

# Some Hard Lattices problems

In this section, we take a concrete look at what a lattice is, and discuss some important worst-case hard problems associated with lattices.

> **Definition (Lattices):**  A Lattice $\Lambda$ or $\mathcal{L}$ is defined as the set of all integer linear
    combinations of $n$ linearly independent $m$-dimensional vectors 
    $B=\\{\mathrm{b_1},\mathrm{b_2},\ldots,\mathrm{b_n}\\}$.  Mathematically, this is represented as  $$\mathcal{L}(B)=\\{Bx, x\in\mathbb{Z}^{m}\\}=\left\\{\sum_{i=1}^{n}x_i\mathrm{b_i} : x_i\in\mathbb{Z}\right\\}$$

The set $B\in\mathbb{R}^{n\times m}$ is said to be the basis of the lattice $\Lambda$. We note here that any lattice $\Lambda$ is characterized by its basis. In the [part 2](), *we will deviate from this definition* by showing another characterization of lattices which will be useful in cryptographic construction schemes. We now note a few lattice problems with known worst-case hardness guarantees.

> **Shortest Vector Problem ($\mathrm{SVP}_\gamma$):** In the $\gamma$-approximate Shortest Vector Problem, we are asked find the length of the shortest non-zero vector (denoted by $\lambda_1$) in an $n$-dimensional lattice, approximately, up to a polynomial factor $\gamma$.

> **Shortest Independent Vector Problem ($\mathrm{SIVP}_\gamma$):** The goal of the $\gamma$-approximate Shortest Independent Vector problem is to output a set of $n$ linearly independent lattice vectors in an $n$-dimensional lattice, approximately of length$\leq\gamma\lambda_n$.

> **Shortest Basis Problem ($\mathrm{SBP}_\gamma$):** The $\gamma$-approximate Shortest Basis Problem asks us to find a basis $B=\{\mathrm{b_1},\mathrm{b_2},\ldots,\mathrm{b_n}\}$ for an $n$-dimensional lattice $\mathcal{L}(B)$ such that $\mathrm{max}_i \lVert\mathrm{b_i}\rVert$ is the smallest possible upto a factor of $\gamma$.

## Main Contribution of Ajtai'99
In [part 2]() we will see the connection between the above problems. We now state an important result from Ajtai'96[^ajtai96].

> **Lemma 1 (Ajtai'96):** If $\mathrm{SBP}_\gamma$ has no polynomial time solution for $\gamma=\mathrm{poly}(n)$, then we can generate a random lattice $\Lambda$ (over some distribution $\mathcal{D}$) together with a ``short" vector $x\in\Lambda$ in polynomial time s.t. there is no algorithm which can find a vector shorter than $\sqrt{n}$ in $\Lambda$ over $\mathcal{D}$ w.p. greater than $n^{-c}$, for sufficiently large $n$ and any $c>0$. 


Here "short'' refers to $\|x\|\leq\sqrt{n}$. Later Ajtai'99 [^ajtai99] extended **Lemma 1** to the following:

> **Theorem 2 (Main Theorem, Ajtai'99):** If $\mathrm{SBP}_\gamma$ has no polynomial time solution for $\gamma=\mathrm{poly}(n)$, then we can generate a random lattice $\Lambda$ (from the same distribution $\mathcal{D}$ as in \cref{lem:ajtmain}) together with a ``short" basis $B$ of $\Lambda$ in polynomial time s.t. there is no algorithm which can find a vector shorter than $\sqrt{n}$ in $\Lambda$ over $\mathcal{D}$ w.p. greater than $n^{-c}$, for sufficiently large $n$ and any $c>0$.

As we see, **Theorem 2** clearly gives us a way to **construct a random instance of a hard problem** ($\mathrm{SBP}_\gamma$). A short basis means $\lVert \mathrm{b_i} \rVert\leq n\sqrt{n},i\in[n]$, where $B=\\{\mathrm{b_1},\mathrm{b_2},\ldots,\mathrm{b_n}\\}$. In [part 2](), we concretely define what we mean by a class of "random" lattices. After that in [part 3](), we build up and give an overview of the construction as stated in **Theorem 2**.


# References
[^ajtai96]: M. Ajtai. 1996. Generating hard instances of lattice problems (extended abstract). In Proceedings of the twenty-eighth annual ACM symposium on Theory of Computing (STOC '96). Association for Computing Machinery, New York, NY, USA, 99–108. https://doi.org/10.1145/237814.237838

[^shor95]: Peter W. Shor. 1997. Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer. SIAM J. Comput. 26, 5 (Oct. 1997), 1484–1509. https://doi.org/10.1137/S0097539795293172

[^ajtai99]: Miklós Ajtai. 1999. Generating Hard Instances of the Short Basis Problem. In Proceedings of the 26th International Colloquium on Automata, Languages and Programming (ICAL '99). Springer-Verlag, Berlin, Heidelberg, 1–9.