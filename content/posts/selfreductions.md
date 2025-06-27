---
title: "Self-Reducibility"
date: 2025-02-01T11:51:18+05:30
draft: false
tags:
  [
    reductions,
    p-np,
    self-reducibility,
    search-to-decision,
    factoring,
    primality,
    cryptography,
    TFNP,
  ]
categories: [toolkit, toc]
---

> _This post assumes basic familiarity with Turing machines, P, NP, NP-completeness, decidability, and undecidability. The reader is referred to the book by Sipser, or the book by Arora and Barak for any formal definitions that have been skipped in this post. Without further ado, let's dive in._

---

# Introduction

In an earlier post, we familiarised ourselves with the [notion of reductions](./reductions.md). Towards the end, we introduced the notion of **self-reducibility** which is our main topic of focus today. We start by familiarising ourselves with a few concepts.

## Preliminaries

- **Languages:** In the world of complexity and computability, a language is a set of strings formed out of some alphabet. Formally, $L\subseteq\Sigma^*$, where the alphabet $\Sigma$ is a finite set of symbols, and $\Sigma^*$ refers to the [Kleene closure](https://en.wikipedia.org/wiki/Kleene_star#Definition) of $\Sigma$.

In the world of complexity, Properties and Problems are _computable_ functions/ relations defined w.r.t. languages.

- **Decision Problems:** A decision problem is a Boolean-valued function $f:\Sigma^*\mapsto\\{0,1\\}$.

Every language $L\subseteq\Sigma^*$ can be uniquely associated with a unique decision problem called the **membership problem**. Here, $x\in L\iff f(x)=1$. A Turing machine $T$ computes/solves the decision problem $f$ if for any input $x\in\Sigma^*$, $T$ halts on any input $x$ and produces output $T(x)=f(x)$.

> Hence problems themselves can be construed as languages.

> Complexity classes **P** and **NP** are defined w.r.t. decision problems.

- **Search Problems:** A search problem is a relation $R\subset\Sigma_{in}^*\times\Sigma_{out}^*$, i.e., $(x,y)\in R$, where $x\in\Sigma_{in},y\in\Sigma_{out}$ are strings belonging to the input and output alphabets respectively.

A Turing machine $T$ computes/solves $R$, if for any input $x\in\Sigma_{in}$, $T(x)$ halts and produces $y\in\Sigma_{out}$ s.t. $(x,y)\in R$.

Since the complexity class **NP** is defined w.r.t. decision problems, we need to introduce an equivalent notion for search problems. Informally, this is denoted by the class **FNP** or Function **NP**, i.e., the set of all function problems for which the validity of an (input, output) pair can be verified in polynomial time (by some algorithm).

- **Complexity class FNP (NP search problem):** A search problem $R$ is in **FNP** if
  1. The size of the witness/certificate set is polynomial, and the size of the witnesses are themselves polynomial in the size of the input. For every $x\in\Sigma_{in}^{*}$, the set $Y_x = \\{y\in{\Sigma_{out}}^{poly(|x|)}\;|(x,y)\in R\;\\}\subseteq{\\{\Sigma_{out}\\}}^{poly(|x|)}$.
  2. The verifier runs in polynomial time. There exists a polynomial time DTM $M$ s.t. for all $x\in\Sigma_{in},y\in\Sigma_{out}$, $M(x,y)=1\iff (x,y)\in R$.

> If $R\in$**FNP** and $R$ is total, i.e., $\forall x\Sigma_{in}, \exists y\in\Sigma_{out}$ s.t. $(x,y)\in R$, then $R$ belongs to the class **TFNP**.

## Search to Decision Reductions

We note here that Decision Problems answer the following flavour of questions:

> Given a problem $P$, is $x$ a solution to $P$? (Yes/No).

On the other hand, Search Problems answer the following flavour of questions:

> Given a problem $P$, output a solution to $P$ with some property. For example, given a problem $P$, output a solution to $P$ that has the minimum length.

We use the satisfiability problem (SAT) as an example to further illustrate the two notions:

- **Decision problem:** Given a propositional formula $\phi$, decide if $\phi$ is satisfiable.
- **Search problem:** Given a propositional formula $\phi$, find a satisfying assignment for $\phi$.

As we see above, if it is easy to solve the Search version of a problem $P$, it is straightforward to solve the Decision version of $P$. The more challenging question is:

> Can we efficiently solve the Search version of a problem $P$, if we know how to solve the Decision version of $P$ efficiently?

Formally, let $O_D^p$ be a decision oracle for a search problem $R\subset\Sigma_{in}^*\times\Sigma_{out}^*$ s.t. querying $O_D^p$ produces $\mathbb{I}[ \exists x\in\Sigma_{in} \;|\; x \text{ has property } p]$; i.e., querying $O_D$ with an appropriate parameter for a _property_ $p$ outputs a <span style="color:blue">yes</span> or a <span style="color:red">no</span> indicating if there exists any input that satisfies the property $p$ (usually taken to be some bound on the input size). Our goal now is to produce $y\in\Sigma_{out}$ s.t. $(x,y)\in R$, using oracle calls to $O_D^p$.

### Examples of Search-to-Decision reduction

In this section, we see some examples of search-to-decision reductions. Let us start with designing a search-to-decision reduction for SAT, which is an NP-complete problem.

There are two inputs to the `SATSearchToDecision()` reduction

- the propositional formula $\phi$ or `f`, and
- the decision oracle for SAT on $O_D$ or `DSAT(f,assign)` which takes as input a propositional formula `f` and a restricted assignment `assign` and returns yes iff `f` is satisfiable under the restriction `assign`. The output of the `SATSearchToDecision()` procedure is a satisfying assignment for $\phi$( or `f`).

```C
SATSearchToDecision(f,DSAT()){
    assignarr = [*,*,....,*];// Intitalize assignarr as an n-bit empty array.
    if(DSAT(f,assignarr)=0) // is f satisfiable without restrictions?
        return -1; // f is not satisfiable
    for (i=1;i<=n; i++){
        assignarr[i]= 0
        //Fix the ith bit in x to be 1.
        // This fixes the ith literal in f.
        if (DSAT(f,assignarr)==1)
            continue;
        // move on to the i+1th coordinate,
        // with the ith bit set to 0.
        assignarr[i]= 1
        //Fix the ith bit in x to be 0.
        // This fixes the ith literal in f.
        if (DSAT(f,assignarr)==1)
            continue;
        // move on to the i+1th coordinate,
        // with the ith bit set to 1.
    return assignarr;
}
```

Earlier we saw that the property used by the decision oracle was a restricted assignment. We list another example of a search-to-decision reduction for the Clique problem (another one of Karp's original 21 NP-complete problems), to give a flavour of a different decision oracle property - based on size.

- **The Clique Decision problem:** Given a graph $G=(V,E)$, decide if $G$ contains a clique of size $\leq k$.
- **The Clique Search problem:** Given a graph $G=(V,E)$, find a clique of size $\leq k$ in $G$ if it exists.

As seen above, there are two inputs to the `CliqueSearchToDecision()` reduction:

- the graph $G$ as an adjacency list `L`, and
- the decision oracle for Clique on $O_D$ or `DCLIQUE(L,k)` which takes as input a adjacency list `L` and a parameter `k` and returns yes iff the graph corresponding to `L` contains a clique of size at most `k`. The output of the `CliqueSearchToDecision()` procedure is an adjacency list corresponding to a clique in $G$ of size $\leq k$.

> Recall that an adjacency list is a collection of unordered lists used to represent a finite graph. We use the following definition of adjacency lists (this is a modification of the definition given in CLRS): An adjacency list is a singly linked list where each element in the list corresponds to a particular vertex, and each element in the list itself points to a singly linked list of the neighboring vertices of that vertex. See the diagram below.

![Adjacency List](/posts/adjacency_list.png)

```C
SATSearchToDecision(L,DCLIQUE()){
    if(DCLIQUE(L,k)=0)
        return -1; // There is no clique of size at most k
    for every vertex v of G {
        Let Lv be the new adjacency list
        obtained by removing vertex v from G.
        // easily done using the above datastructure.
        if(DCLIQUE(Lv,k)=1)
            L = Lv; // update the graph to reflect G = G-v.
    }
    return L;
}
```

Above we saw examples of problems where both search and decision verions are hard. There exist problems where both the search and decision versions are easy, for example, Maximum Matching, Shortest Path, etc. A typical example of a problem the decision version is easy and while the search version is supposedly hard is as follows:

- **The Factoring Decision problem:** Given a natural number $n$, decide if $n$ is prime.
- **The Factoring Search problem:** Given (the binary representation of) a natural number $n$, produce all of its factors.

Many important classes of cryptosystems such as [RSA](https://en.wikipedia.org/wiki/RSA_cryptosystem)^[**The RSA problem:** Find $M$ given the public key $(n,e)$ and a cipher text $C\equiv M^e\mod n$.] depend on the hardness of the Factoring Search problem.^[If we can solve the factoring problem then we can solve the RSA problem by factoring the modulus n. Hence, Factoring $\implies$ RSA.] The Factoring Decision problem is also known as the Primality testing problem.^[The seminal result of Agarwal, Kayal, and Saxena showed that this problem is in **P**. See [this writeup](https://www.tcs.tifr.res.in/~jaikumar/Papers/AKS-revised.pdf) for more details on the result.]

## Self-reducibility

We now move on to the main topic of this post - the notion of self-reducibility^[Self-reducibility is also known as Auto-reducibility.].

> PSPACE, TFNP, PLS, PPAD, PPP, PPA, CLS etc.

> Every downward self-reducible problem in TFNP is in PLS.

### Downward self-reducibility

A search problem R is downward self-reducible (d.s.r) if there is a
polynomial time oracle algorithm for R that on input x ∈ {0, 1}n makes queries to an
R-oracle of size strictly less than n = |x|

> conjecture on Factoring

### (Pseudo)Random self-reducibility

> Discrete logarithm

> Matrix Permanent

https://blog.computationalcomplexity.org/2019/01/search-versus-decision.html

https://blog.computationalcomplexity.org/2006/09/favorite-theorems-unique-witnesses.html

https://blog.computationalcomplexity.org/2006/07/full-derandomization.html

---
