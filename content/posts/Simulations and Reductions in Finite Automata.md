---
title: Simulations and Reductions in Finite Automata
date: 2024-02-01T11:51:18+05:30
draft: false
tags:
  - reductions
  - finite-automata
categories:
  - pedagogy
---
Consider the following problem:
> If $A$ is any language, let $\sqrt{A}$ denote the set of first halves of strings in $A$, where both the first and second halves are identical
 $$\sqrt{A} = \{x \mid \mbox{$xx \in A$}\}$$
 Show that if $A$ is regular, then so is $\sqrt{A}$.

**Proof Sketch**

Before trying to come up with a solution to this problem, let us understand what we can deduce simply from the problem statement:
- $A$ is a regular language $\iff$ $\exists$ some NFA $N_A=(\Sigma_{A},Q_{A},\delta_{A},q^A_{0},F_{A})$ that accepts $A$.

Clearly, we can make use of the NFA $N_A$ to solve the above problem in a piecemeal fashion as follows:
- Assume that for an arbitrary input string $w$, the string $ww\in A$.
- Then there exists a path inside the DFA $N_A$ from the starting state $q^A_0$ to a state $q^A_w=\delta^*_{A}(q^A_0,w)$ on reading the input $w$, where $q^A_w\in Q_{A}$.
- There also exists a path inside the DFA $N_A$ from the intermediate state $q^A_w$ to some final state $f_w=\delta^*_{A}(q^A_w,w)$ on reading the input $w$, where $f_w\in {F_A}$.

Therefore, if we could somehow 
1. Guess the state $q^{A}_{w}$. 
2. Using $w$ as input, run the DFA $D_{A}$ simultaneously from the states $q^{A}_{0}$ and $q^{A}_{w}$ to obtain states $q^A_{a}$ and $q^A_{b}$ respectively.

Then, we can determine the membership of  $ww$ in $A$ , simply by checking the states $q^A_{a}$ and $q^A_{b}$, as 
> $ww\in A$ $\iff q^A_{a}=q^A_{w}$ and $q^A_{b}\in F_{A}$.

We can perform both steps by constructing an NFA $N$ that moves a 3-tuple of states from $Q_{A}$ to another 3-tuple of states of $Q_{A}$ as follows:
$\delta_{N}((q_1,q_2,q_3),z)\;\;=\;\;(\delta^*_{A}(q_1,\varepsilon),\;\;\delta^*_{A}(q_2,z),\;\;\delta^*_{A}(q_{3},z))\;\;=\;\;(q^A_z,q^A_z,q)$, where $q\in Q_{A}$.

Since we can therefore construct a finite automata for the language $\sqrt{A}$ given that $A$ is a regular language, the language $\sqrt{A}$ is regular.

> Exercise: Figure out the other entries of the five tuple of $N$ explicitly in terms of the five-tuple of $N_A$.

We saw how to construct an NFA $N$ using the transitions of the NFA $N_A$ to accept a language which is different from $A$. Alternatively phrased, we used $N$ to simulate the NFA $N_A$. This technique is extremely pervasive in all of computer science and is known as a reduction. A concrete introduction this concept will be introduced later, but here is a small primer for the curious folks.

# The basic concept of Reductions
Take two problems, $P_O$ (*an old known problem*) and $P_N$ (*a new unknown problem*). Consider the two following situations that might arise where we would ideally like to avoid reinventing the wheel to solve $P_N$.

- We know that $P_O$ is easy to solve, and we notice that the underlying mathematical structure of $P_N$ is similar to $P_O$. 
	- An example is the problem that we solved above, where we know that $P_O=A$ is a regular language, and $P_{N}=\sqrt{A}$ has a mathematical structure similar to $P_O$.
- We know that $O$ is hard to solve, and once again, $N$ is similar to $O$.

### Case 1: Solving $P_N$ when $P_O$ is easy to solve
In the first case, our primary approach is to transform instances of $P_N$ into instances of $P_O$ and then use the algorithm for $P_O$ to solve for $P_N$. In other words, using our existing knowledge of $P_O$, we solve $P_N$ and demonstrate that $P_N$ is a special case of $P_O$. 

In the example above, once we identified that $A$ is regular, implying that there exists a finite automata that "solves" for $A$, we can simulate the finite automata in a clever way to solve $\sqrt{A}$.

>Formally, we say that $N$ reduces to $O$. 

### Case 2: Showing $P_N$ is hard to solve when $P_O$ is hard to solve
In this case, we can again exploit the structural similarities, but this time to a different end. We shall see a lot of examples of this case later when we work with Turing Machines. We shall defer the discussion on this case till then.