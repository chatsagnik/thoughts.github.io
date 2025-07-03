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

In an earlier post, we familiarised ourselves with the [notion of reductions](https://theoretickles.netlify.app/posts/reductions). Towards the end, we introduced the notion of **self-reducibility** which is our main topic of focus today. We start by familiarising ourselves with a few concepts.

# Search-to-Decision Reductions

In the world of complexity and computability, a _language_ is a set of strings formed out of some alphabet. Formally, $L\subseteq\Sigma^{\*}$, where the alphabet $\Sigma$ is a finite set of symbols, and $\Sigma^{\*}$ refers to the [Kleene closure](https://en.wikipedia.org/wiki/Kleene_star#Definition) of $\Sigma$.

[Last time we formalized reductions in terms of Turing Machines](https://theoretickles.netlify.app/posts/reductions). We now explicitly define mapping reductions (equivalent to Karp reductions for TMs) in terms of languages. Let $L_1 \subseteq \Sigma_1^{\*}$ and $L_2 \subseteq \Sigma_2^{\*}$ be languages. Recall that $L_1\leq_m L_2$ (or, $L_1$ reduces to $L_2$), if there exists a computable function $f: \Sigma_1^{\*}\mapsto\Sigma_2^{\*}$ s.t. for every $w\in\Sigma_1^{\*}$, $w\in L_1\iff f(w)\in L_2$. We sometimes use the notation $A\leq^p_m B$ to denote that the function $f$ is polynomial time constructible.

A **decision problem** is a Boolean-valued function $D:\Sigma^{\*}\mapsto\\{0,1\\}$. We can view $D$ as a language $L_D = \\{ x\in\Sigma^{\*} : D(x)=1\\}$. Conversely, every language $L\subseteq\Sigma^{\*}$ can be uniquely associated with a unique decision problem $D_L$ called the **membership problem**. Here, $x\in L\iff D_L(x)=1$. A Turing machine $T$ computes/solves the decision problem $D$ if for any input $x\in\Sigma^{\*}$, $T$ halts on any input $x$ and produces output $T(x)=D(x)$.

Recall that complexity classes **P** and **NP** are defined w.r.t. decision problems. $L$ is in **P** if $\exists$ an efficient decider for the membership problem of any string in $\Sigma^{\*}$. $L$ is in **NP** if there exists an efficient verifier and a polynomial sized certificate for the membership problem of any string in $\Sigma^{\*}$.

In complexity theory, many problems can be naturally expressed as **search problems** (for example, TSP, HamCycle, etc.), but are shoehorned into the above model of **decision problems** in order for us to easily classify them into classes like **P** and **NP**.

- **Search Problems:** A search problem is a relation $R\subset\Sigma_{in}^{\*}\times\Sigma_{out}^{\*}$, i.e., $(x,y)\in R$, where $x\in\Sigma_{in},y\in\Sigma_{out}$ are strings belonging to the input and output alphabets respectively. Search problems are also known as relational problems / optimization problems.

A Turing machine $T$ decides/computes/solves $R$, if for any input $x\in\Sigma_{in}$, $T(x)$ halts and produces $y\in\Sigma_{out}$ s.t. $(x,y)\in R$, or correctly states that no such $y$ exists.

- A relation $R\subset\Sigma_{in}^{\*}\times\Sigma_{out}^{\*}$ is a **polynomially-balanced** if for any $(x,y)\in R$, $|y|=\text{poly}(|x|)$.

Since the complexity class **NP** is defined w.r.t. decision problems, we need to introduce an equivalent notion for search problems. Informally, this is denoted by the class **FNP** (or Function **NP**). Formally, a polynomially-balanced relation $R\in$ **FNP** (i.e., $R$ is an NP search problem) if $R$ is polynomial-time computable. Note that if $R$ is polynomially balanced, any $y$ s.t. $(x,y)\in R$ serves as the certificate/witness for $x$. A search problem $R$ is in **FP** if $R\in$ **FNP** and if there exists an efficient decider for $R$.

> **FP** = **FNP** iff **P** = **NP**.

## Examples of Search-to-Decision reductions

In this section, we see some examples of search-to-decision reductions. Let us start with designing a search-to-decision reduction for SAT, which is an **NP-complete** problem. Recall that decision problems answer the following flavour of questions:

> Given a problem $P$, is $x$ a solution to $P$? (Yes/No).

On the other hand, search problems answer the following flavour of questions:

> Given a problem $P$, output a solution to $P$ with some property.

For example, given a problem $P$, output a solution to $P$ that has the minimum length. We use the satisfiability problem (SAT) as an example to further illustrate the two notions:

- **Decision problem:** Given a propositional formula $\phi$, decide if $\phi$ is satisfiable.
- **Search problem:** Given a propositional formula $\phi$, find a satisfying assignment for $\phi$.

Note that SATSearch $\in$ **FNP** but SATSearch $\notin$ **TFNP**, since a formula may be unsatisfiable.

As we see above, if it is easy to solve the Search version of a problem $P$, it is straightforward to solve the Decision version of $P$. The more challenging question is:

> Can we efficiently solve the Search version of a problem $P$, if we know how to solve the Decision version of $P$ efficiently?

### Search-to-Decision Reduction for SAT

Formally, let $O_D^p$ be a decision oracle for a search problem $R\subset\Sigma_{in}^{\*}\times\Sigma_{out}^{\*}$ s.t. querying $O_D^p$ produces $\mathbb{I}[ \exists x\in\Sigma_{in}\;|\; x \text{ has property } p]$; i.e., querying $O_D$ with an appropriate parameter for a _property_ $p$ outputs a <span style="color:blue">yes</span> or a <span style="color:red">no</span> indicating if there exists any input that satisfies the property $p$ (usually taken to be some bound on the input size). Our goal now is to produce $y\in\Sigma_{out}$ s.t. $(x,y)\in R$, using oracle calls to $O_D^p$.

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

### Search-to-Decision Reduction for CLIQUE

Earlier we saw that the property used by the decision oracle was a restricted assignment. We list another example of a search-to-decision reduction for the Clique problem (another one of Karp's original 21 **NP-complete** problems), to give a flavour of a different decision oracle property - based on size.

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

---

# Formal notion of Self-reducibility

Above we saw examples of problems admitting efficient search-to-decision reductions where both the search and decision verions are computationally hard. `Maximum Matching` and `Shortest Path` are problems where both the search and decision versions are computationally easy, and hence admit efficiently constructible search-to-decision reductions by definition. This is interesting since these two categories of problems are very different from a computational perspective.

However, in the context of the existence of efficient search-to-decision reductions, not all problems are created equal. While, some problems have naturally equivalent notions of search and decision problems, for others search and decision problems may not be computationally equivalent.

A problem is **self-reducible** or **auto-reducibile** if it admits an efficient search-to-decision reduction, i.e., any efficient solution to the decision version of the problem implies an efficient solution to the search version of the problem.

## Downward self-reducibility

A search problem $R$ is **downward self-reducible** (d.s.r) if there is a polynomial time oracle algorithm for $R$ that on input $x \in \Sigma^{\*}$ makes queries to an $R$-oracle of size strictly less than $|x|$. In other words, a language $L$ is d.s.r. if there exists a polynomial time algorithm $A^O$ deciding $x\overset{?}{\in} L$ with a membership oracle $O$ for $L$ that can handle subqueries for strings $z\overset{?}{\in} L$ s.t. $|z|<|x|$.

We can extend the notion of downward self-reducibility to _functions_ or decision problems as follows: A function $f:\Sigma^{\*}\mapsto \\{0,1\\}$ is downward self-reducible if there exists a polynomial time algorithm $A^{O_f}$ s.t. on any input of length $n$, $A$ only makes queries of length $<n$ to the membership oracle ${O_f}$, and for every input $x$, $A^{O_f}(x)=f(x)$.

It is easy to see that SAT is d.s.r. since given any formula $\phi$ on $n$-variables, one can consider only querying on restrictions of $\phi$ to figure out if $\phi$ is satisfiable.

> All **NP-complete** decision problems are downward self-reducible.

This is straightforward to show using the fact that there exists a Karp-reduction from SAT to $L$ for any **NP-complete** problem $L$.

> Every downward self-reducible decision problem is in **PSPACE**.

_Proof._ Let the input to a d.s.r. problem $f$ be $x$. Then any algorithm $A$ that solves $f$ will make queries to some oracle and recursively compute the answer to each query. The depth of the recursion is at most |x|, and at each level of recursion, the algorithm needs to remember the state which requires space at most poly(|x|). This last point holds because the basic computation runs in polynomial time, and hence polynomial space.

**Note:** We also recall that **PSPACE** is closed [^complete] under Karp-reductions. Since, all d.s.r. languages belong to **PSPACE**, we can conclude that **PSPACE** is **hard** (worst-case or average-case) iff a d.s.r. language is **hard** (in the same sense.)

### An application of Downward Self-Reductions: Mahaney's Theorem

A language $L$ is (polynomially) **sparse** if it the number of strings of length $n$ in $L$ is bounded by a polynomial in $n$.

> **[Mahaney's Theorem]** Assuming **P** $\neq$ **NP**, there are no sparse **NP-complete** languages.

_Proof Sketch:_ Recall the d.s.r tree of SAT. Given a SAT formula $\phi[x_1,\ldots,x_n]$ at the first level we can restrict the formula to $\phi_0[0,\ldots,x_n]$ and $\phi_1[1,\ldots,x_n]$. If $\phi$ is satisfiable, then at least one of $\phi_0$ or $\phi_1$ is satisfiable. Hence, at the $\ell$th level, at least one of the $2^{\ell}$ formulas have to be satisfiable for the original formula to be satisifable. If $L$ is a sparse NP-complete language, we have $SAT\leq^p_m L$. Hence, using a mapping reduction from SAT to $L$, we can prune the d.s.r. tree s.t. at the $\ell$th level to only contend with $\text{poly}(\ell)$ formulas. This straightforwarly yields a polynomial time SAT algorithm, since there are only $n$ levels. This violates the [Exponential Time Hypothesis](https://en.wikipedia.org/wiki/Exponential_time_hypothesis), and therefore there does not exist any $L$ that is both sparse and NP-complete.

The above proof sketch is due to Joshua Grochow[^grochow], who credits Manindra Agrawal for the original idea. See [Oded Goldreich's comments here](https://www.wisdom.weizmann.ac.il/~oded/MC/208.html). Due to the lack of formatting in the above linked blog (and for my own archival purposes), I reproduce his comments _as-is_ below.

> **Oded's comments:**
> The proof is indeed simple, not any harder than the proof that assumes that the sparse set is a subset of a set in P (see Exercise 3.12 in my computational complexity book).
>
> The first idea is to scan the downwards self-reducibility tree of SAT and extend at most polynomially many viable vertices at each level of the tree, where this polynomial is determined by the polynomials of the sparseness bound and the stretch of the Karp-reduction (of SAT to the sparse set). The crucial idea is the following: Rather than testing the the residual formulae at the current level by applying the Karp-reduction to each of them, one applies the Karp-reduction to the disjunction of the first formula (i.e., $\phi_1$) and each of the remaining formulae (i.e, the $\phi_i$'s for $i>1$).
>
> The point is that if all reduction values are distinct, then one of these values is not in the sparse set, and it follows that the first formula is not in SAT, and so the first formula can be discarded. Otherewise, if the reduction maps $\phi_1\vee\phi_i$ and $\phi_1\vee\phi_i$ to the same value, then we can discard (say) $\phi_i$, since either $\phi_1$ is in SAT (and remains viable) or $\phi_1$ is not in SAT and so $\phi_i$ and $\phi_j$ have the same SAT-value (and so it suffices to keep either of them).

---

# Which problems are (not) known to be self-reducible?

Recall that self-reducibility is precluded in cases where the decision version is easy and while the search version is hard. For example, consider the `Factoring` problem:

- **The Factoring Decision problem:** Given a natural number $n$, decide if $n$ is prime.
- **The Factoring Search problem:** Given (the binary representation of) a natural number $n$, produce all of its factors.

Many important classes of cryptosystems such as [RSA](https://en.wikipedia.org/wiki/RSA_cryptosystem) [^R1] are based on the hardness of the `Factoring Search problem`.[^R2] On the other hand, the `Factoring Decision problem` (also known as the `Primality testing problem`) has long been known to computationally tractable.[^primality] Hence, `Factoring` does not appear to be straightforwardly self-reducible.

This leans into the conjecture that not every problem in **NP** is necessarily self-reducible (for example, Factoring). Bellare and Goldwasser'94 formally proved that if **EE** $\neq$ **NEE**, then there exists a language in **NP** that is not self-reducible.[^EE]

Following the above result, Beame et al. (1998)[^beame98] showed the existence of various search problems in **FNP** that are not computationally equivalent to their decision versions. This is particularly important when a solution for the search problem is **guaranteed to exist** (but hard to find).

One of the research directions on the notion of self-reducible problems is to characterise their complexity. Even though decisional complexity is well understood at an undergraduate level, the complexity of search problems is strictly a graduate level topic due to the sheer amount of prerequisites involved. In the remainder of this post, I will attempt to provide an introduction to the complexity landscape of **NP**-search problems and state a few connected results related to self-reducibility.

## Taxonomy of NP search problems

The complexity class **TFNP** consists of all search problems in **FNP** that are total in the sense that a solution is guaranteed to exist. In other words, $R\in$**TFNP**$\iff R\in$**FNP** and $R$ is total, i.e., $\forall x\in\Sigma_{in}, \exists y\in\Sigma_{out}$ s.t. $(x,y)\in R$. Using the totality of **TFNP**, it is straightforward to show that **TFNP** $=$ **FNP** $\cap$ **coFNP**.

In the case of decision problems, the notion of **NP-completeness** plays a huge role in establishing relative hardness results among many problems of interests, and has a beautiful relationship with the concept of reductions.

> There is an **FNP-complete** problem in **TFNP** if **NP** = **coNP**.

Since we consider a **P** $\neq$ **NP** to be a reasonable assumption, it is very likely that **NP** $\neq$ **coNP**. Does this imply that there is no notion of a _hard_ search problem? That also seems unlikely, because common sense dictates that search versions of problems in **NP** would be in some sense reducible to search versions of **NP-complete** problems!

How do we then characterize the hardness of efficiently verifiable search problems? In other words, can we formally provide examples of search problems that are (for some notion of hardness) the hardest in **TFNP**? At this point, we list some interesting candidate computational problems known to be in **TFNP** that are not known to have polynomial time algorithms:

- `NASHx`: Finding a mixed Nash equilibrium of a game with $x$ players.
- `Brouwer`/`Kakutani`: Finding Brouwer/Kakutani fixed points for continuous functions.[^Brouwer][^Kakutani]
- `Factoring`: Finding a (or all) prime factors of a number $n\geq 2$.
- `SetCover`: Finding a minimum Set Cover.

Unfortunately, it is believed that **TFNP** has [no complete problems](https://theoretickles.netlify.app/posts/reductions/#completeness) since it is a **semantic class**![^semantic]

Hence, in order to characterize the notion of hardness in search problems, various syntactic subclasses have been defined in an attempt to classify the many diverse problems that belong to **TFNP**.

Recall that TFNP problems have the following two characteristics - **(a)** any solution can be checked efficiently _(the NP property)_, and **(b)** there always exists at least one solution _(the totality property)_. The syntactic subclasses are defined based on the combinatorial principle used to argue totality in **TFNP**. In other words, each subclass of **TFNP** has a corresponding existence proof principle (for example, an instance of a circuit or graph), _one that when applied in a general context, does not yield a polynomial-time algorithm_.

### Important subclasses of TFNP

We define two important subclasses of TFNP in this post - **PLS** and **PPAD** (along with a whole host of others in the footnotes section).

The complexity class **PLS** (also known as **Polynomial Local Search**), is a subclass of TFNP function problems which contains problems that are guaranteed to have a solution because of the lemma that "every finite directed acyclic graph has a sink."

> _Remark:_ The graph instance above may be exponentially large. For a finer intuition see the description of **EOL**[^EOL] in the footnotes.

**PLS** captures problems of finding a local minimum of an objective function $f$, in contexts where any candidate solution $x$ has a local neighbourhood within which we can readily check for the existence of some other point having a lower value of $f$. One of the motivations behind defining **PLS** was to capture the notion of problems with existing local optimum in **NP-hard problems** like `TSP`. Since a **PLS** problem always has a local optimum, it therefore always a solution (as the set of solutions is finite). Hence, using the above definitions, we have:

> **FP** $\subseteq$ **PLS** $\subseteq$ **TFNP** $\subseteq$ **FNP**.[^FP]

Examples of **PLS-complete problems** include search versions of `Set-Cover`, `Metric-TSP`, and `Weighted Independent-Set` (among many many others).

Another subclass of **TFNP** is the complexity class **PPAD**. **PPAD** is defined as the set of functions in **TFNP** that reduce in polynomial time to the `End-Of-Line (EOL)` problem.[^EOL] In other words, a problem is complete for **PPAD** if it belongs to **PPAD** and if **EOL** reduces in polynomial time to that problem. **PPAD** is contained in **PPA** $\cap$ **PPP** [^PPA][^PPP] and could be regarded as the directed version of the class **PPA**.[^PPA]

`NASH2` $\in$ **PPAD**, and `NASH3` is **PPAD-complete**. The `Brouwer` and `Kakutani` problems are also in **PPAD**.

Problems in **PLS** can be solved with local improvement algorithms, while problems in **PPAD** admit "optimality equations" that characterize solutions as fixed points.

> PPAD and PLS are believed to be strictly incomparable.[^beame98][^oppenheim]

The complexity class Continuous Local Search **CLS** = **PPAD** $\cap$ **PLS**.[^KKT][^CLS][^EOL] `Gradient Descent` is in **CLS**.

The following diagram ([appropriated from this talk](https://www.youtube.com/watch?v=as720_SRpY0)) captures the above discussion in a nutshell.

![TFNP problem landscape](/posts/TFNP.jpg)

## Complexity of self-reducibile problems

Earlier we saw that every downward self-reducible decision problem is in **PSPACE**. The following result was recently shown, as an analogue for search problems.

> Every downward self-reducible search problem in TFNP is in PLS. [^harsha23]

Hence **PLS** is in some senses the functional analogue of **PSPACE**. Harsha et al. (2023) [^harsha23] also show that most natural **PLS-complete** problems are downward self-reducible. We end with an important open question by Harsha et al. (2023) [^harsha23]:

> Is Factoring downward self reducible?

If Factoring is downward self-reducible, then Factoring$\in$**UEOPL**$\subseteq$**PPAD**$\cap$**PLS** [^harsha23]. The complexity class **UniqueEOPL** (Unique End of Potential Line) captures search problems with the property that their solution space forms an _exponentially_ large line with increasing cost.[^EOPL][^UEOPL] From one candidate solution we can calculate another candidate solution in polynomial time. The end of that line is the (unique) solution of the search problem. This implies that no efficient factoring algorithm exists using the factorization of smaller numbers.

---

# Footnotes

[^semantic]: A complexity class is called a **semantic class** if the Turing Machine (TM) defining this class has a property that is undecidable. See [Papadimitrou's original paper](https://www.karlin.mff.cuni.cz/~krajicek/papadim1.pdf) and [this Stackexchange link](https://cstheory.stackexchange.com/questions/1233/semantic-vs-syntactic-complexity-classes) for a more formal discussion on this topic. In a nutshell, promise classes such as **RP**, **ZPP**, **BPP** are semantic.
[^PPP]: The existence of solutions for problems in **complexity class PPP:** (also known as Polynomial Pigeonhole Principle) is guaranteed by the pigeonhole principle: if $n$ balls are placed in $m < n$ bins then at least one bin must contain more than one ball.
[^EOL]: **EOL** is the class of problems which can be reduced to the `EOL` problem instance: Given a exponentially large directed graph consisting of lines and cycles on the vertex set $[2^𝑛]$, find any sink of the graph assuming vertex 1 is the source and every vertex has in and out degree at most 1. Note that we overload the notation **EOL** to refer to both the problem and the complexity class.
[^CLS]: The **EOL** class is a combinatorially defined alternative to the complexity class **CLS** (Continuous Local Search), which contains Gradient Descent and various fixed point problems. **CLS** is the smallest known subclass of **TFNP** not known to be in **P**, and hardness results for it imply hardness results for **PPAD** and **PLS** simultaneously.
[^EOPL]: One interesting subclass of **EOL** is **End-Of-Potential-Line** (**EOPL**) where we add the following constraint addition to the `EOL` problem instance: we are also given a potential function that increases along each edge. It is known that the `EOPL` problem is complete for **PPAD** $\cap$ **PLS**. Hence **EOPL** = **CLS**, suggesting an equivalence between purely combinatorially defined search problems and real-valued continuous optimisation problems. [See this thesis](https://ora.ox.ac.uk/objects/uuid:67e2d80b-76bf-4b49-9b7d-8bbd91633dd7) for a detailed perspective on this equivalence.
[^UEOPL]:
    If the graph instance in the `EOPL` problem has a unique sink, then the problem (and the related complexity class) is known as **UEOPL**. It is an open question if the `UEOPL` is complete for **PPAD** $\cap$ **PLS**.[^harsha23]
    Once again, we note that we overload the notations **EOPL** and **UEOPL** to refer to both the problems and their corresponding complexity class.

[^Brouwer]: **Brouwer's fixed-point theorem** states that for any continuous function $f$ mapping a nonempty compact convex set to itself, there is a point $x_0$ such that $f(x_0)=x_0$.
[^Kakutani]: The **Kakutani fixed point theorem** is a generalization of the Brouwer fixed point theorem to set-valued functions. By [Weller's Theorem](https://en.wikipedia.org/wiki/Weller%27s_theorem), Kakutani's fixed-point theorem is used in proving the existence of cake allocations that are both envy-free and Pareto efficient.
[^KKT]: Finding a point where Gradient Descent terminates is equivalent to finding a KKT point—when the domain is bounded. Computing a KKT point of a continuously differentiable function over $[0, 1]^2$ is complete for **PPAD** $\cap$ **PLS**.
[^R1]: **The RSA problem:** Find $M$ given the public key $(n,e)$ and a cipher text $C\equiv M^e\mod n$.
[^R2]: If we can solve the factoring problem then we can solve the RSA problem by factoring the modulus n. Hence, Factoring $\implies$ RSA.
[^primality]: The seminal result of Agarwal, Kayal, and Saxena showed that this problem is in **P**. See [this writeup](https://www.tcs.tifr.res.in/~jaikumar/Papers/AKS-revised.pdf) for more details on the result.
[^EE]: **EE** equals DTIME($2^{2^{O(n)}}$).
[^complete]: We recall the notion of [notion of complete problems](https://theoretickles.netlify.app/posts/reductions#completeness) here.
[^harsha23]: Prahladh Harsha, Daniel Mitropolsky, and Alon Rosen. Downward Self-Reducibility in TFNP. ITCS 2023. [DOI](https://doi.org/10.4230/LIPIcs.ITCS.2023.67).
[^grochow]: [See this preprint](https://arxiv.org/pdf/1610.05825).
[^PPA]:
    The **Complexity class PPA** (also known as **Polynomial Parity Argument**) captures computational search problems whose totality is rooted in the handshaking lemma for undirected graphs: "all graphs of maximum degree 2 have an even number of leaves."
    More precisely, **PPA** captures search problems for which there is a polynomial-time algorithm that, given any string, computes its 'neighbor' strings (of which there are at most two). Then given a leaf string (i.e. one with only one neighbor), the problem is to output another leaf string.

[^beame98]:
    Paul Beame, Stephen Cook, Jeff Edmonds, Russell Impagliazzo, and Toniann
    Pitassi. 1998. The Relative Complexity of NP Search Problems. J. Comput. System
    Sci. 57, 1 (1998), 3–19. https://doi.org/10.1145/225058.225147

[^oppenheim]: Josh Buresh-Oppenheim, Tsuyoshi Morioka: **Relativized NP Search Problems and Propositional Proof Systems**. CCC 2004: 54-67.
[^FP]: From our discussions above, it may be unclear why search problems in **FP** have to be _total_. To see why, consider $R\in$ **FP** and fix a special string $y_0$ s.t. $\forall x\in\Sigma_{in}^{\*}$ s.t. $(x,y_0)\notin R$. An easy way to construct $y_0$ is to append every $y\in\Sigma_{out}^{\*}$ with a $1$, and set $y_0$ as the all zero string.
