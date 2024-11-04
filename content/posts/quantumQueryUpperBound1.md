---
title: Quantum Query Upper Bounds based on Classical Decision Trees
date: 2023-03-01T12:04:49+05:30
draft: true
tags:
  - tcs
  - quantum
  - decision-trees
  - pedagogy
  - query-complexity
  - rank
  - guessing-complexity
categories:
  - pedagogy
---
# Introduction

In many areas of theoretical computer science, we come across various problems where we can exploit structures in the problem to obtain quantum speedups over known classical algorithms.

Examples of this include the famous Shor's algorithm (Shor95 [^shor95]) which allows us to compute discrete log or factor integers efficiently, while there are no known classical algorithms that do the same. 

In this vein, the central question addressed by **CMP22** [^cmp22] is as follows:

> Let $f$ be a Boolean function that can be computed by an efficient classical query algorithm. For what kind of $f$ is it possible to obtain a quantum query algorithm with a speedup over the classical counterpart?

Phrased alternatively, we are looking for Boolean functions with some structure or property (preferably combinatorial in nature) which is amenable to quantum speedup in the query model. 

Below we give a small overview of the main arguments of **CMP22**.

**LL14** [^linlin14] introduced a new complexity measure on Boolean functions known as the **Guessing Complexity** (abbreviated as G.C.), and used G.C. to bound the quantum query complexity of associated Boolean functions. This is defined formally in **Lemma1**.
### Lemma 1 (LL14)

> Let $f$ be a boolean function with an associated decision tree $\mathcal{T}$ s.t. depth of $\mathcal{T}$ is $T$, and the G.C. of $\mathcal{T}$ is $G$. Then the quantum query complexity of $f$ is upper-bounded as follows: $$Q(f)=O\left(\sqrt{TG}\right)$$

## Main Contributions of CMP22
The main contributions of **CMP22** include showing that: 
- The Guessing complexity of a Boolean function is equivalent to the **rank of the Boolean function**. 
	- The rank of a Boolean function (a combinatorial property of the Boolean function introduced by **EH89**[^eh89]) is directly tied to the decision tree representation computing the Boolean function, and hence, the query complexity of said Boolean function.
- There exists a class of Boolean functions called complete $\mathrm{And-Or}$ binary trees which have randomized rank that is polynomially smaller than the deterministic rank. 
	- This was proved using a technique known as **Prover-Delayer Games** introduced by **PI00**[^pi00] in the context of satisfiability.

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
>
> - leaf nodes labeled in $\{-1,+1\}$,
> - internal nodes are labeled by $x_i\in\{0,1\}: x=x_0\ldots x_{n-1}\; \forall x\in D_f$,
> - edges labeled in $\{0,1\}$.
>
> s.t. the tree computes $f$ according to the node's label and takes the edge with value equivalent to the value of $x_i$. The output value at the leaf $b\in\{-1,+1\}$ is s.t. $(x,b)\in f$.

Given a relation $f$ and a deterministic decision tree $\mathcal{T}$ computing it, we define the function $\tilde{f}$ that takes an input $x\in\{0,1\}^n$ and outputs the leaf of $\mathcal{T}$ reached on input $x$. We make two assumptions on $\mathcal{T}$ as follows:

1. We assume that in the decision trees computing $f$, for all leaves there is an input $x\in D_f$ that reaches that leaf.
2. On each path from root to leaf, each bit $x_i$ is queried at most once.

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

>1.  Let $f$ be a Boolean function. Then $\mathrm{rank}(f)=\mathrm{val}(f)$.
>2. Resolution complexity measures can be completely characterized by Prover-Delayer Games.


# Polynomial Separation between Rank and Randomized Rank

## Rank and Randomized Rank of a Boolean function

Rank is a combinatorial measure of a decision tree introduced by **EH89** in the context of learning theory. We define rank and randomized rank below:

> Let $\mathcal{T}$ be a binary decision tree. We define the rank of $\mathcal{T}$ as follows:
> 1. $\mathrm{rank}(u)=0$, if $u$ is a leaf
> 2. $\mathrm{rank}(u)=\mathrm{max} \left\\{ \mathrm{rank}(u_L),\mathrm{rank}(u_R)\right\\}$, if $\mathrm{rank}(u_L)\neq\mathrm{rank}(u_R)$
> 3. $\mathrm{rank}(u)=\mathrm{rank}(u_L)+1$ , if $\mathrm{rank}(u_L)=\mathrm{rank}(u_R)$.

The rank of a decision tree $\mathrm{rank}(\mathcal{T})$ is the rank of its root. The randomized rank (denoted by $\mathrm{rrank}(\mathcal{T})$) of a randomized decision tree is the maximum rank of a deterministic decision tree in its support.

We make the following observation here:
> $\mathrm{rank}(\mathcal{T})\leq\mathrm{depth}(\mathcal{T})=D(f)$. Similarly, $\mathrm{rrank}(\mathcal{T})\leq R(f)$.

Next, we define the rank of a Boolean function $f$ as,
$$\mathrm{rank}(f) := \underset{\mathcal{T}:\mathcal{T}\text{ computes }f}{\mathrm{min}} \mathrm{rank}(\mathcal{T}).$$
Similarly, the randomized rank of a Boolean function $f$ is defined as,   
$$\mathrm{rrank}(f) := \underset{\mathcal{T}}{\mathrm{min}}\\;\mathrm{depth}(\mathcal{T})$$
where, $\mathcal{T}\text{ is a randomized D.T. that computes }f \text{ with }\varepsilon\leq 1/3$.

## Polynomial Separation between $rank(f)$ and $rrank(f)$

Consider the following approach:
- Let $f$ be a Boolean function s.t. $R(f)<_p D(f)$.
- If $\mathrm{rank}(f)\approx D(f)$, then we have $\mathrm{rrank}(f)<_p \mathrm{rank}(f)$ (since $\mathrm{rrank}(f)\leq R(f)$).

However, we also know that $\mathrm{rank}(f)\leq D(f)$. In fact, $\mathrm{rank}(f)$ itself can be polynomially smaller than $D(f)$ which would invalidate the above argument. Therefore, we need to find specific $f$ s.t. $\mathrm{rank}(f)\approx D(f)$. 

We now state the first result of **CMP22**.
### Theorem 1 (CMP22)
> Let $f$ be the **complete And-Or tree** of depth $\log{n}$. Then $\mathrm{rrank}(f)<_p \mathrm{rank}(f)$. 

***Proof sketch.***  Using Prover-Delayer games introduced by **PI00**, the authors showed that $\mathrm{rank}(f)=\frac{n+2}{3}$. In **SW86**[^sw86], it was shown that $D(f)=n$ and $R(f)=\Theta\left(n^{0.753\ldots}\right)$. Since $\mathrm{rrank}(f)\leq R(f)$, we have, $$\mathrm{rrank}(f)\leq R(f)<\frac{n+2}{3}=\mathrm{rank}(f)=D(f).$$
# Guessing Complexity

In this section we define guessing complexity of a Boolean function $f$, and prove that $G(f)=\mathrm{rank}(f)$. This will allow us to bound the query complexity of a Boolean functions using **Lemma1**, in terms of $\mathrm{rank}$ which is a combinatorial measure.

We first define **G-coloring** of a tree $\mathcal{T}$ as:
> A G-coloring of $\mathcal{T}$ is a coloring of the edges of $\mathcal{T}$ from $\\{B,R\\}$, s.t. any internal node of $\mathcal{T}$ has at most one outgoing $B$ edge.

The intuition behind G-coloring is that red edges indicate mistakes made by the decision tree for a particular assignment. We want a path from the root to a leaf s.t. number of red edges is small.

We define the Guessing Complexity of a decision tree $\mathcal{T}$ and a Boolean function $f$ as follows:
> $$ G(\mathcal{T})= \underset{\text{G-colorings of }\mathcal{T}}{\mathrm{min}}\\;\\;\underset{\text{paths }\mathcal{P}\text{ in }\mathcal{T}}{\mathrm{max}} \left\\{ \text{\# of red edges in }\mathcal{P}\right\\},$$
> $$G(f)= \underset{\mathcal{T}\text{ computing }f}{\mathrm{min}}\left\\{ G(\mathcal{T})\right\\}.$$

Here $\mathcal{T}$ can be either deterministic or randomized. We note that $G(\mathcal{T})\leq \mathrm{depth}(\mathcal{T})$. If $\mathcal{T}$ is a complete binary tree, then $G(\mathcal{T})= \mathrm{depth}(\mathcal{T})$, since we can always find a path $\mathcal{P}$ of red edges from root to a leaf (due to the constraints of G-coloring), s.t. $\left|{\mathcal{P}}\right|=\mathrm{depth}(\mathcal{T})$.

We now state and prove the following theorem.
### Theorem 2 (CMP22)
> Given a decision tree $\mathcal{T}$, $G(\mathcal{T})=\mathrm{rank}(\mathcal{T})$.

**Proof:** Let $v$ be the root of $\mathcal{T}$ with left child $v_L$ and right child $v_R$. 

***Forward Direction***: Consider a G-coloring of $\mathcal{T}$. This induces a G-coloring of $\mathcal{T_L}$ and $\mathcal{T_R}$ (subtree rooted at $v_L$ and $v_R$ respectively). This gives rise to two cases:
- ***Case 1:*** $G(\mathcal{T_L})=G(\mathcal{T_R})=k$. One of the edges $(v,v_L)$ or $(v,v_R)$ must be colored red. WLOG let $(v,v_L)$ be colored red. Since $G\mathcal{T_L}=k$, there exists a $k$ length path of red edges in $\mathcal{T_L}$, and therefore induces a $k+1$ length path of red edges in $\mathcal{T}$. Hence $G(\mathcal{T})\geq G(\mathcal{T_L})$.
- ***Case 2:*** Wlog, let $G(\mathcal{T_L})>G(\mathcal{T_R})$. Then $G(\mathcal{T})\geq \mathrm{max}\\{G(\mathcal{T_L}),G(\mathcal{T_R})\\}=G(\mathcal{T_L})$ using the above logic.

***Backward Direction***: 
- ***Case 1:*** $G(\mathcal{T_L})=G(\mathcal{T_R})=k$. Arbitrarily color one of the edges in $(v,v_L)$ or $(v,v_R)$ red. Maximum number of red edges in a path is increased by $1$. Therefore $G(\mathcal{T})\leq G(\mathcal{T_L})+1$.
- ***Case 2:*** Wlog let $G(\mathcal{T_L})>G(\mathcal{T_R})$. Color edge $(v,v_R)$ red. Thus the maximum number of red edges in a path equals $\mathrm{max}\\{G(\mathcal{T_L}),G(\mathcal{T_R})+1\\}=G(\mathcal{T_L})$. Therefore $G(\mathcal{T})\leq G(\mathcal{T_L})$.

Combining all the cases, we see that $G(\mathcal{T})=\mathrm{rank}(\mathcal{T})$.

# Quantum Query Speedup

### Lemma 1 (LL14) recall
> Let $f$ be a Boolean function with an associated decision tree $\mathcal{T}$ s.t. depth of $\mathcal{T}$ is $T$, and guessing complexity of $\mathcal{T}$ is $G$. Then the quantum query complexity of $f$ is upper bounded as follows: $Q(f)=O\left(\sqrt{TG}\right)$.

**Proof Sketch:** Let there be a classical deterministic query algorithm $A$ that computes $f$ but makes at most $G$ mistakes. Consider a quantum query algorithm that runs for $T$ rounds (since the depth of the tree is $T$) and uses a modified Grover algorithm to find the next mistake. Since there are $T$ iterations and at most $G$ mistakes, we can obtain an $O\left(\sqrt{TG}\right)$ quantum query algorithm that computes $f$.

Note that in the proof sketch of **Lemma1**, we are skipping a critical step related to span programs that is covered in detail in **LL14**.
### Theorem 3 (CMP22)
> Let $\mathcal{T}$ be a randomized decision tree (with an underlying distribution $\mu$) computing $f$, s.t. $\mathrm{depth}(\mathcal{T})=T$ and $\mathrm{rrank}(\mathcal{T})=G$. Then $Q(f)=O\left(\sqrt{TG}\right)$.

**Proof Sketch:** Sample a decision tree from the support of $\mathcal{T}$. Since  $\mathrm{rrank}(\mathcal{T})$ is nothing but its Guessing Complexity, we can use the construction in the proof of Lemma1 to obtain a quantum query algorithm s.t. $Q(f)=O\left(\sqrt{TG}\right)$.

### Theorem 4 (CMP22)
> For **complete And-Or tree** of depth $\log{n}$, we have $Q(f)<_p D(f)$.

**Proof Sketch:** The proof follows straightforwardly from using the polynomial separation between rank and randomized rank (see **Theorem 1**). Using **Theorem 3** on a randomized decision tree $\mathcal{T}$ s.t. $\mathrm{depth}(\mathcal{T})=T$ and $\mathrm{rrank}(\mathcal{T})=G$ we can obtain
 $$Q(f)=O\left(\sqrt{TG}\right)=O\left(\sqrt{T\cdot \mathrm{rrank}(\mathcal{T})}\right) =  O\left(\sqrt{n^{0.753}\log n }\right) < n = D(f).$$
# References

[^shor95]: **Shor'95:** Peter W. Shor. 1997. Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer. SIAM J. Comput. 26, 5 (Oct. 1997), 1484–1509. https://doi.org/10.1137/S0097539795293172

[^cmp22]: **CMP22:** Cornelissen, Arjan, Mande, Nikhil S., & Patro, Subhasree. 2022. Improved Quantum Query Upper Bounds Based on Classical Decision Trees. In: Foundations of Software Technology and Theoretical Computer Science.

[^linlin14]: **LL14:** Lin, Cedric Yen-Yu, & Lin, Han-Hsuan. 2014. Upper Bounds on Quantum Query Complexity Inspired by the Elitzur–Vaidman Bomb Tester. In: Theory of Computing.

[^eh89]: **EH89:** Ehrenfeucht, Andrzej, & Haussler, David. 1989. Learning decision trees from random examples. Information and Computation, 82(3), 231–246.

[^pi00]: **PI00:** Pudlak, Pavel, & Impagliazzo, Russell. 2000. A lower bound for DLL algorithms for k-SAT (preliminary version). In: ACM-SIAM Symposium on Discrete Algorithms.

[^bshouty95]: **Bshouty95:** Exact Learning Boolean Function via the Monotone Theory. Electron. Colloquium Comput. Complex., TR95.

[^sw86]: **SW86:** M. Saks and A. Wigderson, "Probabilistic Boolean decision trees and the complexity of evaluating game trees," _27th Annual Symposium on Foundations of Computer Science (sfcs 1986)_, Toronto, ON, Canada, 1986, pp. 29-38, doi: 10.1109/SFCS.1986.44.
