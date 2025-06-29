---
title: "Self-Reducibility"
date: 2025-02-01T11:51:18+05:30
draft: false
tags:
  [
    reductions,
    p-np,
    eth,
    self-reducibility,
    search-to-decision,
    factoring,
    primality,
    TFNP,
  ]
categories: [toc, complexity]
---

> _This post assumes basic familiarity with Turing machines, P, NP, NP-completeness, decidability, and undecidability. The reader is referred to the book by Sipser, or the book by Arora and Barak for any formal definitions that have been skipped in this post. Without further ado, let's dive in._

---

# Introduction

In an earlier post, we familiarised ourselves with the [notion of reductions](https://theoreticles.netlify.app/posts/reductions). Towards the end, we introduced the notion of **self-reducibility** which is our main topic of focus today. We start by familiarising ourselves with a few concepts.

## Preliminaries

_Readers familiar with concepts such as Decision problems, Search Problems, and complexity classes TFNP, PPAD, PLS, etc. can skip this section._

- **Languages:** In the world of complexity and computability, a language is a set of strings formed out of some alphabet. Formally, $L\subseteq\Sigma^{\*}$, where the alphabet $\Sigma$ is a finite set of symbols, and $\Sigma^{\*}$ refers to the [Kleene closure](https://en.wikipedia.org/wiki/Kleene_star#Definition) of $\Sigma$.

> Last time we formalized reductions in terms of Turing Machines. We now explicitly define mapping reductions (equivalent to Karp reductions for TMs) in terms of languages. Let $L_1 \subseteq \Sigma_1^{\*}$ and $L_2 \subseteq \Sigma_2^{\*}$ be languages. Recall that $L_1\leq_m L_2$ (or, $L_1$ reduces to $L_2$), if there exists a computable function $f: \Sigma_1^{\*}\mapsto\Sigma_2^{\*}$ s.t. for every $w\in\Sigma_1^{\*}$, $w\in L_1\iff f(w)\in L_2$. We sometimes use the notation $A\leq^p_m B$ to denote that the function $f$ is polynomial time constructible.

- **Decision Problems:** A decision problem is a Boolean-valued function $f:\Sigma^{\*}\mapsto\\{0,1\\}$.

Every language $L\subseteq\Sigma^{\*}$ can be uniquely associated with a unique decision problem called the **membership problem**. Here, $x\in L\iff f(x)=1$. A Turing machine $T$ computes/solves the decision problem $f$ if for any input $x\in\Sigma^{\*}$, $T$ halts on any input $x$ and produces output $T(x)=f(x)$.

> Hence problems themselves can be construed as languages.

> Complexity classes **P** and **NP** are defined w.r.t. decision problems.

- **Search Problems:** A search problem is a relation $R\subset\Sigma_{in}^{\*}\times\Sigma_{out}^{\*}$, i.e., $(x,y)\in R$, where $x\in\Sigma_{in},y\in\Sigma_{out}$ are strings belonging to the input and output alphabets respectively. Search problems are also known as relational problems / optimization problems.
  - $R\subset\Sigma_{in}^{\*}\times\Sigma_{out}^{\*}$ is a polynomially-balanced relation if $(x,y)\in R\iff|y|=\text{poly}(|x|)$.
  - A Turing machine $T$ computes/solves $R$, if for any input $x\in\Sigma_{in}$, $T(x)$ halts and produces $y\in\Sigma_{out}$ s.t. $(x,y)\in R$.

Since the complexity class **NP** is defined w.r.t. decision problems, we need to introduce an equivalent notion for search problems. Informally, this is denoted by the class **FNP** (or Function **NP**). Formally, a polynomially-balanced relation $R$ defines a NP search problem if $R$ is polynomial-time computable.

- **Complexity class TFNP:** The complexity class TFNP consists of all search problems in FNP that are total in the sense that a solution is guaranteed to exist. In other words, $R\in$**TFNP**$\iff R\in$**FNP** and $R$ is total, i.e., $\forall x\Sigma_{in}, \exists y\in\Sigma_{out}$ s.t. $(x,y)\in R$. It is straightforward to show that **TFNP** = **FNP** $\cap$ **co-FNP**.

> It is not believed that **TFNP** has complete problems since it is a semantic class [^semantic], and various syntactic subclasses have been used to classify the many diverse problems that belong to **TFNP**. The syntactic subclasses are defined based on the combinatorial principle used to argue totality in **TFNP**.

Each subclass of TFNP has a corresponding existence proof principle (for example, an instance of a circuit or graph), one that when applied in a general context, does not yield a polynomial-time algorithm.

### Subclasses of TFNP

- **Complexity class PLS:** Also known as Polynomial Local Search, this is a subclass of TFNP function problems which contains problems that are guaranteed to have a solution because of the lemma that "every finite directed acyclic graph has a sink."

> **PLS** captures problems of finding a local minimum of an objective function $f$, in contexts where any candidate solution $x$ has a local neighbourhood within which we can readily check for the existence of some other point having a lower value of $f$. One of the motivations behind defining PLS was to capture the notion of local optimum in **NP-hard problems** like TSP.

- **Complexity class PPA:** Also known as Polynomial Parity Argument, this class captures computational problems whose totality is rooted in the handshaking lemma for undirected graph: "all graphs of maximum degree 2 have an even number of leaves."

> More precisely, **PPA** captures search problems for which there is a polynomial-time algorithm that, given any string, computes its 'neighbor' strings (of which there are at most two). Then given a leaf string (i.e. one with only one neighbor), the problem is to output another leaf string.

- **Complexity class PPAD:** PPAD is contained in **PPA** $\cap$ **PPP**[^PPP] and could be regarded as the directed version of the class PPA.

> > - The problem of finding a Nash equilibrium (NASH) in a normal form game of two or more players with specified utilities, is in **PPAD**. NASH with three players is **PPAD-complete**.
> > - Brouwer's fixed-point theorem states that for any continuous function $f$ mapping a nonempty compact convex set to itself, there is a point $x_0$ such that $f(x_0)=x_0$. Computing fixed-points in Brouwer functions is in **PPAD**.
> > - Computing a Karush-Kuhn-Tucker (KKT) point of a continuously differentiable function over the domain $[0, 1]^2$ is **PPAD** $\cap$ **PLS-complete**.
> > - Gradient Descent is in **PPAD** $\cap$ **PLS**.

## Search to Decision Reductions

We note here that Decision Problems answer the following flavour of questions:

> Given a problem $P$, is $x$ a solution to $P$? (Yes/No).

On the other hand, Search Problems answer the following flavour of questions:

> Given a problem $P$, output a solution to $P$ with some property. For example, given a problem $P$, output a solution to $P$ that has the minimum length.

We use the satisfiability problem (SAT) as an example to further illustrate the two notions:

- **Decision problem:** Given a propositional formula $\phi$, decide if $\phi$ is satisfiable.
- **Search problem:** Given a propositional formula $\phi$, find a satisfying assignment for $\phi$.

Note that SATSearch $\in$ **FNP** but SATSearch $\notin$ **TFNP**, since a formula may be unsatisfiable.

As we see above, if it is easy to solve the Search version of a problem $P$, it is straightforward to solve the Decision version of $P$. The more challenging question is:

> Can we efficiently solve the Search version of a problem $P$, if we know how to solve the Decision version of $P$ efficiently?

Formally, let $O_D^p$ be a decision oracle for a search problem $R\subset\Sigma_{in}^{\*}\times\Sigma_{out}^{\*}$ s.t. querying $O_D^p$ produces $\mathbb{I}[ \exists x\in\Sigma_{in}\;|\; x \text{ has property } p]$; i.e., querying $O_D$ with an appropriate parameter for a _property_ $p$ outputs a <span style="color:blue">yes</span> or a <span style="color:red">no</span> indicating if there exists any input that satisfies the property $p$ (usually taken to be some bound on the input size). Our goal now is to produce $y\in\Sigma_{out}$ s.t. $(x,y)\in R$, using oracle calls to $O_D^p$.

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

Above we saw examples of problems where both search and decision verions are hard. There exist problems where both the search and decision versions are easy, for example, Maximum Matching, Shortest Path, etc. A typical example of a problem where the decision version is easy and while the search version is supposedly hard is as follows:

- **The Decision problem (Primality testing):** Given a natural number $n$, decide if $n$ is prime.
- **The Search problem (Factoring):** Given (the binary representation of) a natural number $n$, produce all of its factors.

Note that unlike SATSearch and CliqueSearch, Factoring$\in$**TFNP** since any natural number will always have at least two factors.

Many important classes of cryptosystems such as [RSA](https://en.wikipedia.org/wiki/RSA_cryptosystem) [^R1] depend on the hardness of the Factoring Search problem.[^R2] The Factoring Decision problem is also known as the Primality testing problem.[^primality]

### Formal notion of Self-reducibility

We now move on to the main topic of this post - the notion of self-reducibility (also known as auto-reducibility). A problem is self-reducible if it admits an efficient search-to-decision reduction, i.e., any efficient solution to the decision version of the problem implies an efficient solution to the search version of the problem.

> Note: Not every problem in NP is necessarily self-reducible. If EE$\neq$NEE, then there exists a language in NP that is not self-reducible (Bellare and Goldwasser'94).[^EE]

## Downward self-reducibility

A search problem $R$ is downward self-reducible (d.s.r) if there is a polynomial time oracle algorithm for $R$ that on input $x \in \Sigma^{\*}$ makes queries to an $R$-oracle of size strictly less than $|x|$. In other words, a language $L$ is d.s.r. if there exists a polynomial time algorithm $A^O$ deciding $x\overset{?}{\in} L$ with a membership oracle $O$ for $L$ that can handle subqueries for strings $z\overset{?}{\in} L$ s.t. $|z|<|x|$.

> We can extend the notion of downward self-reducibility to _functions_ or decision problems as follows: A function $f:\Sigma^{\*}\mapsto \\{0,1\\}$ is downward self-reducible if there exists a polynomial time algorithm $A^{O_f}$ s.t. on any input of length $n$, $A$ only makes queries of length $<n$ to the membership oracle ${O_f}$, and for every input $x$, $A^{O_f}(x)=f(x)$.

It is easy to see that SAT is d.s.r. since given any formula $\phi$ on $n$-variables, one can consider only querying on restrictions of $\phi$ to figure out if $\phi$ is satisfiable.

> All **NP-complete** decision problems are downward self-reducible.

This is straightforward to show using the fact that there exists a Karp-reduction from SAT to $L$ for any $NP-complete$ problem $L$.

> Every downward self-reducible decision problem is in **PSPACE**.

_Proof._ Let the input to a d.s.r. problem $f$ be $x$. Then any algorithm $A$ that solves $f$ will make queries to some oracle and recursively compute the answer to each query. The depth of the recursion is at most |x|, and at each level of recursion, the algorithm needs to remember the state which requires space at most poly(|x|). This last point holds because the basic computation runs in polynomial time, and hence polynomial space.

**Note:** We also recall that **PSPACE** is closed [^complete] under Karp-reductions. Since, all d.s.r. languages belong to **PSPACE**, we can conclude that **PSPACE** is **hard** (worst-case or average-case) iff a d.s.r. language is **hard** (in the same sense.)

> Every downward self-reducible search problem in TFNP is in PLS. [^harsha23]

Hence **PLS** is in some senses the functional analogue of **PSPACE**. Harsha et al. (2023) [^harsha23] also show that most natural **PLS-complete** problems are downward self-reducible. We end with an important open question by Harsha et al. (2023) [^harsha23]:

> Is Factoring downward self reducible?

If Factoring is downward self-reducible, then Factoring$\in$**UEOPL**$\subseteq$**PPAD**$\cap$**PLS** [^harsha23]. The complexity class **UniqueEOPL** (Unique End of Potential Line) captures search problems with the property that their solution space forms an _exponentially_ large line with increasing cost. From one candidate solution we can calculate another candidate solution in polynomial time. The end of that line is the (unique) solution of the search problem. This implies that no efficient factoring algorithm exists using the factorization of smaller numbers.

### An application of Downward Self-Reductions: Mahaney's Theorem

A language $L$ is (polynomially) **sparse** if it the number of strings of length $n$ in $L$ is bounded by a polynomial in $n$.

> **[Mahaney's Theorem]** Assuming **P** $\neq$ **NP**, there are no sparse **NP-complete** languages.

_Proof Sketch:_ Recall the d.s.r tree of SAT. Given a SAT formula $\phi[x_1,\ldots,x_n]$ at the first level we can restrict the formula to $\phi_0[0,\ldots,x_n]$ and $\phi_1[1,\ldots,x_n]$. If $\phi$ is satisfiable, then at least one of $\phi_0$ or $\phi_1$ is satisfiable. Hence, at the $\ell$th level, at least one of the $2^{\ell}$ formulas have to be satisfiable for the original formula to be satisifable. If $L$ is a sparse NP-complete language, we have $SAT\leq^p_m L$. Hence, using a mapping reduction from SAT to $L$, we can prune the d.s.r. tree s.t. at the $\ell$th level to only contend with $\text{poly}(\ell)$ formulas. This straightforwarly yields a polynomial time SAT algorithm, since there are only $n$ levels. This violates the [Exponential Time Hypothesis](https://en.wikipedia.org/wiki/Exponential_time_hypothesis), and therefore there does not exist any $L$ that is both sparse and **NP-complete**.

The above proof sketch is due to Joshua Grochow[^grochow], who credits Manindra Agrawal for the original idea.

## What's Next?

We have looked at self-reducibility and downward self-reducibility. In follow-up posts, we will introduce the notions of randomized reductions, random self-reducibility, and pseudorandom self-reducibility.

[^semantic]: A complexity class is called a semantic class if the Turing Machine (TM) defining this class has a property that is undecidable. See [Papadimitrou's original paper](https://www.karlin.mff.cuni.cz/~krajicek/papadim1.pdf) and [this Stackexchange link](https://cstheory.stackexchange.com/questions/1233/semantic-vs-syntactic-complexity-classes) for a more formal discussion on this topic. In a nutshell, promise classes such as **RP**, **ZPP**, **BPP** are semantic.
[^PPP]: The existence of solutions for problems in **complexity class PPP:** (also known as Polynomial Pigeonhole Principle) is guaranteed by the pigeonhole principle: if $n$ balls are placed in $m < n$ bins then at least one bin must contain more than one ball.
[^R1]: **The RSA problem:** Find $M$ given the public key $(n,e)$ and a cipher text $C\equiv M^e\mod n$.
[^R2]: If we can solve the factoring problem then we can solve the RSA problem by factoring the modulus n. Hence, Factoring $\implies$ RSA.
[^primality]: The seminal result of Agarwal, Kayal, and Saxena showed that this problem is in **P**. See [this writeup](https://www.tcs.tifr.res.in/~jaikumar/Papers/AKS-revised.pdf) for more details on the result.
[^EE]: **EE** equals DTIME($2^{2^{O(n)}}$).
[^complete]: We recall the notion of [notion of complete problems](https://theoreticles.netlify.app/posts/reductions#completeness) here.
[^harsha23]: Prahladh Harsha, Daniel Mitropolsky, and Alon Rosen. Downward Self-Reducibility in TFNP. ITCS 2023. [DOI](https://doi.org/10.4230/LIPIcs.ITCS.2023.67).
[^grochow]: (See this preprint)[https://arxiv.org/pdf/1610.05825].
