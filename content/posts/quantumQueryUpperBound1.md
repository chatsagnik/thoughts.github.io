---
title: "Quantum Query Upper Bounds based on Classical Decision Trees"
date: 2023-03-01T12:04:49+05:30
draft: false
tags:
  [
    tcs,
    quantum,
    decision-trees,
    pedagogy,
    query-complexity,
    rank,
    guessing-complexity,
  ]
categories: [pedagogy]
---
# Introduction
In many areas of theoretical computer science, we come across various problems where we can exploit structures in the problem to obtain quantum speedups over known classical algorithms. Examples of this include the famous Shor's algorithm (Shor95 [^shor95]) which allows us to compute discrete log or factor integers efficiently, while there are no known classical algorithms that do the same. In this vein, the central question addressed by **CMP22** [^cmp22] is as follows:

> Let $f$ be a Boolean function that can be computed by an efficient classical query algorithm. For what kind of $f$ is it possible to obtain a quantum query algorithm with a speedup over the classical counterpart?

Phrased alternatively, 
> We are looking for Boolean functions with some structure or property (preferably combinatorial in nature) which is amenable to quantum speedup in the query model. 

Below we give a small overview of the main arguments of **CMP22** before we dive into the details in later sections.

**LL14** [^linlin14] introduced a new complexity measure on boolean functions known as the **Guessing Complexity** (abbreviated as G.C.), and used G.C. to bound the quantum query complexity of associated boolean functions.
### Lemma 1 (LL14)
Let $f$ be a boolean function with an associated decision tree $\mathcal{T}$ s.t. depth of $\mathcal{T}$ is $T$, and the G.C. of $\mathcal{T}$ is $G$. Then the quantum query complexity of $f$ is upper-bounded as follows: $$Q(f)=O\left(\sqrt{TG}\right)$$
**CMP22** showed that the Guessing complexity of a Boolean function is equivalent to the **rank of the Boolean function**, which is a combinatorial property of the Boolean function (introduced by **EH89**[^eh89]). On the other hand, the rank of a Boolean function is directly tied to the decision tree representation computing the Boolean function. 

Further CMP22 also prove the following using a technique known as **Prover-Delayer Games** introduced by **PI00**[^pi00] in the context of satisfiability. 

> There exists a class of Boolean functions called complete $\mathrm{And-Or}$ binary trees which have randomized rank that is polynomially smaller than the deterministic rank.

The proposition below follows from the above discussions.

### Proposition 1 (CMP22)
For complete $\mathrm{And-Or}$ binary trees, $Q(f) <_p R(f) <_p D(f)$. Here $<_p$ means "strictly polynomially smaller than".

In these notes, we will be providing a proof sketch for $Q(f)<_p D(f)$, given a class of Boolean functions, namely complete $\mathrm{And-Or}$ binary trees, which have $R(f)<_p D(f)$.

# Preliminaries
If the reader is well-versed in decision trees and query complexity, they can skip ahead to the section on Prover-Delayer games.

## Boolean Functions and Decision Trees
Boolean functions are of the form $f:\{0,1\}^n\xrightarrow{} \{-1,+1\}$. More generally, consider a relation $f\subseteq \{0,1\}^n\times \{-1,+1\}$, where the domain of $f$ is 
    $$D_f=\\{x\in\{0,1\}^n : \exists b\in\{-1,+1\} s.t. (x,b)\in f\\}$$
> A **deterministic** **decision tree** $\mathcal{T}$ computing such a relation $f$ is a binary tree with 
> - leaf nodes labeled in $\{-1,+1\}$,
> - internal nodes are labeled by $x_i\in\{0,1\}: x=x_0\ldots x_{n-1}\; \forall x\in D_f$,
> - edges labeled in $\{0,1\}$.
> 
> s.t.  the tree computes $f$ according to the node's label and takes the edge with value equivalent to the value of $x_i$. The output value at the leaf $b\in\{-1,+1\}$ is s.t. $(x,b)\in f$.

Given a relation $f$ and a deterministic decision tree $\mathcal{T}$ computing it, we define the function $\tilde{f}$ that takes an input $x\in\{0,1\}^n$ and outputs the leaf of $\mathcal{T}$ reached on input $x$. We make two assumptions on $\mathcal{T}$ as follows:
1. We assume that in the decision trees computing $f$, for all leaves there is an input $x\in D_f$ that reaches that leaf. 
2.  On each path from root to leaf, each bit $x_i$ is queried at most once.

We now state an important lemma on decision trees for Boolean functions:
> **Bshouty95**[^bshouty95] : Given any Boolean function $f$, $\exists$ a decision tree that computes $f$.

At this point, we mention a subtle fact that becomes important when trying to define complexity measures on $f$ using decision tree constructions.
> For a given Boolean function $f$, there does not exist a unique decision tree that can compute it.

We can observe the above fact, by considering the function $\mathrm{Or}$ as an example. For any decision tree $\mathcal{T}$ that computes $\mathrm{Or}$, we can simply permute the order of the nodes on any path from the root to leaf and obtain an equivalent tree $\mathcal{T}^{\prime}$ that computes $\mathrm{Or}$.

We now define randomized decision trees as follows:
> A **randomized decision tree** is a distribution $\mu$ over deterministic decision trees. A randomized D.T. computes a relation $f\subseteq \{0,1\}^n\times \{-1,+1\}$ with error $\varepsilon$, if $\forall x\in D_f$ and for all $\mathcal{T}$ in the support of $\mu$,
> $$\mathrm{Pr}_{\mathcal{T}\sim\mu}\left[\mathcal{T}\text{ outputs }b : (x,b)\in f\right]\geq 1-\varepsilon$$
## Query Complexity
In the query complexity model, instead of direct access to the input $x\in\{0,1\}^n$, we are given oracular access to $x$. To access the input, an algorithm is allowed to query the oracle $O_x$ for the $i$th bit $x_i$ of $x$, which we can then use to compute some function $f(x)$.

The goal is to compute $f(x)$ using as few queries to $O_x$ as possible. An important point to note is that the amount of computation between queries is not constrained, and we are only interested in the number of queries to $O_x$ instead of the total time needed to compute $f(x)$.

**Note:** Note that lower bounds in the query model do not imply lower bounds in the computational (or Turing Machine) model. **Example:** For sorting inputs in a bounded range, the computational bound is $\Theta({n})$ (using bucket sort or radix sort) while the query lower bound is $\Omega({nlogn})$.

We can view decision trees as follows: 
- the internal nodes of the tree represent the answers to the queries, and 
- the leaves represent the solutions as before. 
- The edges taken in the decision tree are based on the response the oracle makes for that query.

The previous view leads us to the following fact.
> The notion of query complexity is exactly captured by decision trees. The optimal query complexity for a function $f$ is equal to the depth of the optimal decision tree which computes $f$. 

We now define the **deterministic and randomized query complexities** as follows:
> - $D(f)$ is the smallest depth of a deterministic D.T. $\mathcal{T}$ computing $f$.
> - $R(f)$ is the smallest depth of a randomized D.T. $\mathcal{T}$ computing $f$ with error at most $1/3$.

Let a quantum circuit be represented as $U_1 O_x U_2 O_x \ldots O_x U_T$, where $U_i$'s are unitary operators and $O_x$ are oracle calls defined as follows: $$O_x\ket{i,a,z}\xrightarrow{}\ket{i,a\oplus x_i,z}$$
We now define the **quantum query complexity** as:
> The number of oracle calls made by the quantum circuit which computes $f$ with error at most $1/3$. In the above case it is $T-1$.

At this point let's take a gander at the different query complexities of some well known functions.
> **Query Complexity of** **Or**: Given input $x=x_0x_1\ldots x_{n-1}\in\{0,1\}^n$, we define $f(x)=\mathrm{Or}(x)=\lor_{i\in[n]}x_i$. Then we have $D(f)=n, R(f)=n, Q(f)=\Theta({\sqrt{n}})$.

The bound for $Q(f)$ of **Or** comes from the famous Grover's algorithm.

> **Query Complexity of** **Parity**: Given input $x=x_0x_1\ldots x_{n-1}\in\{0,1\}^n$, we define $f(x)=\mathrm{Parity}(x)=\oplus_{i\in[n]}x_i=\sum_{i\in[n]}x_i \mod 2$. Then we have $D(f)=n, R(f)=n, Q(f)=\Theta(n/2)$.

The bound for $Q(f)$ of **Parity** comes from the famous Deutsch-Josza algorithm. In this work, **CMP22** show that for **complete And-Or binary trees**, $D(f)=n, R(f)=n^{0.753}, Q(f)=\Theta(\sqrt{n})$.

## Prover-Delayer Games
While the full proof of the main result of **CMP22** using PD games is outside of the scope of these notes, we introduce the Prover-Delayer games in this section since the technique itself may be of independent interest. We introduce a simplified version of Prover-Delayer games as used by **CMP22** instead of the general setup as given in **PI00**.

Consider a two-player game between Prover (P) and Delayer (D). 
- Together P and D maintain a partial assignment $\rho=\{0,1,\perp\}$ to a Boolean function $f$. 
- Initially, we start with the assignment $\perp^n$. 
- Each round begins with 
	- P querying an index $i\in[n]$ to D for which $x_i=\perp$. 
	- D can answer with $x_i=0$, $x_i=1$, or defer the choice of $x_i$ up to P. 
- The game ends when P knows the value of $f$, i.e., $f|_\perp$ is a constant function. 
The value of the game is the maximum number of times D can defer choice of the assignment to P. For example, $\mathrm{val}(\mathrm{Or})=1$ since anytime D defers to P, P can set the corresponding $x_i=1$ and end the game.

Prover strategies are used to obtain upper bounds, while Delayer strategies are used to obtain lower bounds. We state the following results from **PI00** without proof.

> Let $f$ be a Boolean function. Then $\mathrm{rank}(f)=\mathrm{val}(f)$.

> Resolution complexity measures can be completely characterized by Prover-Delayer Games.


# References

[^shor95]: **Shor'95:** Peter W. Shor. 1997. Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer. SIAM J. Comput. 26, 5 (Oct. 1997), 1484–1509. https://doi.org/10.1137/S0097539795293172

[^cmp22]: **CMP22:** Cornelissen, Arjan, Mande, Nikhil S., & Patro, Subhasree. 2022. Improved Quantum Query Upper Bounds  Based on Classical Decision Trees. In: Foundations of Software Technology and Theoretical Computer  Science.

[^linlin14]: **LL14:** Lin, Cedric Yen-Yu, & Lin, Han-Hsuan. 2014. Upper Bounds on Quantum Query Complexity Inspired by  the Elitzur–Vaidman Bomb Tester. In: Theory of Computing.

[^eh89]: **EH89:** Ehrenfeucht, Andrzej, & Haussler, David. 1989. Learning decision trees from random examples. Information  and Computation, 82(3), 231–246.

[^pi00]: **PI00:** Pudlak, Pavel, & Impagliazzo, Russell. 2000. A lower bound for DLL algorithms for k-SAT (preliminary  version). In: ACM-SIAM Symposium on Discrete Algorithms.

[^bshouty95]: **Bshouty95:** Exact Learning Boolean Function via the Monotone Theory. Electron. Colloquium Comput. Complex., TR95.