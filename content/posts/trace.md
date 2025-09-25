---
title: "The Matrix Trace"
date: 2025-03-08T12:03:46+05:30
draft: false
tags: [linear algebra, quantum computing, matrix analysis]
categories: [math]
---

## A Prelude to this post

I was introduced to the notion of the **trace of a square matrix** as early as the eleventh grade, but never fully appreciated the nuances attached to this seemingly innocuous concept until I took a _second_ course in Linear algebra! Funnily, the reason I even took the second course was because I was struggling the intuition behind certain concepts such as density matrices and measurements in basic quantum computing and quantum information theory!

In hindsight, the lack of a deep dive into topics like trace very early on is perhaps intended by design. The computational aspects of matrices such as determinants and invertibility are crucial to a lot of school-level and undergraduate cs, physics, and maths, while a first course in linear algebra is supposed to gently handhold your way as you make your own connections between abstract concepts like vector spaces and other subjects like abstract algebra and analysis. We start this post with the first time any student might run into a deep think about the notion of trace!

# The Trace (First Definitions)

Let us start with two definitions of trace, and a series of quizzes.

> **Definition 1 (Trace of a Linear Operator):** Let $V$ be a vector space, and $A:V\mapsto V$ be a linear operator [^linmap] on $V$. The trace of $A$ is defined as $tr(A)=\sum_{i=1}^{n}\lambda_i(A)$, where the $\lambda_i(A)$'s are the eigenvalues of $A$.

> **Definition 2 (Trace of a Matrix):** Let $M$ be an $n\times n$ square matrix. The trace of $M$ is defined as $tr(M)=\sum_{i=1}^{n}M(i,i)$, where $M(i,j)$ is the $(i,j)$th entry of $M$. Hence the trace is the sum of diagonal entries of $M$.

Here is a small exercise for the readers at this point: Let $V$ be finite-dimensional, and let $B_1$ and $B_2$ be two different bases of $V$ s.t. the corresponding matrix representations of $A$ are $M_1$ and $M_2$.[^bounded] Prove that $$\sum_{i=1}^{n}M_1(i,i)=\sum_{i=1}^{n}M_2(i,i)=\sum_{i=1}^{n}\lambda_i(A).$$

Before proving the above statement in full generality, try assuming the following relaxations:

1. $V$ is a finite-dimensional inner product space [^innerproduct] (this gives us access to orthonormal bases), and
2. $A$ is Hermitian with distinct eigenvalues (this gives us access to the Spectral Theorem[^spectral]).[^quantum]

Let us pause at this moment and reflect on a few things. What does the above exercise actually tell us? Succintly put, under sufficiently well-behaved conditions (finite-dimensional operators/bounded operators), Definitions 1 and 2 are equivalent. Note that the following observations immediately from the equivalence between Definitions 1 and 2.

1. Most importantly it tells us that the trace of an operator is an invariant quantity, much like its determinant!

2. The sum of eigenvalues of an operator is equivalent to the sum of diagonal entries of any of its matrix representations!

In the following sections, we will prove the equivalence between Definitions 1 and 2, along with the above observations.

### The Characteristic Polynomial and a third definition of Trace

We start by relating the eigenvalues of an operator to the entries of any of its matrix representations by way of the characteristic polynomial.[^charpoly] Let $A$ be a linear operator on an $n$-dimensional vector space $V$. Let $B$ a basis of $V$ and we denote the matrix representation of $A$ w.r.t. $B$ is $M_B$.

Consider the following matrix representation $M_B = \begin{bmatrix}a_{11}\&\ldots\& a_{1n}\\\ \vdots\&\ddots\&\vdots \\\ a_{i1}\& a_{ii}\& a_{in} \\\ \vdots\&\ddots\&\vdots \\\ a_{1}\&\ldots\& a_{nn}\end{bmatrix}$ of the operator $A$. The characteristic polynomial of $M_B$ is as follows:

$$ p(M*B) = \det(M_B-\lambda\cdot I) = \prod*{i=1}^n (a\_{ii} - \lambda) + \text{terms of degree }\leq n-2$$

$$\implies p(M_B) = (-1)^n \big(\lambda^n - \textcolor{red}{\Bigg(\sum_{i=1}^n a_{ii}\Bigg)}  \,\lambda^{n-1} + \dots + (-1)^n \det M_B\big)$$

$$\implies p(M_B) = (-1)^n \big(\lambda^n - \textcolor{red}{(\text{tr}\, M_B)} \,\lambda^{n-1} + \dots + (-1)^n \det M_B\big)\,.\;\; \text{(by Definition 2)}$$

> **Exercise:** Check for yourself that the the coefficient of the degree 0 term of the characteristic polynomial is indeed $\det M_B$.

Recall that if a polynomial $p(x)$ has a root $r$, then $x-r$ divides $p(x)$.[^factor] Also recall that by definition, the roots of the characteristic polynomial are the eigenvalues of $A$. Hence, $p(M_B)=\prod_i (x-\lambda_i)$.

Note that if $C$ is a different basis of $V$, then the matrix representation of $A$ w.r.t. $C$, denoted by $M_C$, then the matrices $M_B$ and $M_C$ are **similar**, i.e., there exists a matrix $S$ (precisely the change of basis matrix from $B$ to $C$), s.t. $M_B = S M_C S^{-1}$. Then, we have

> $$P_A(M_C) = \det(M_C - \lambda I) = \det(S\cdot M_B\cdot S^{-1} - S\cdot \lambda I\cdot S^{-1})$$

> $$\implies P_A(M_C)= \det(S\cdot (M_B-\lambda I)\cdot S^{-1}) = \det(S)\cdot P_A(M_B)\cdot\det(S^{-1})$$

> $$\implies P_A(M_C)= P_A(M_B).$$

Hence, the characteristic polynomial is invariant due to change of basis. This implies that **the roots of the characteristic polynomial are invariant under basis transformations**. Therefore, this implies that the trace (the sum of the roots of the characteristic polynomial by Definition 1) is also invariant under basis change. There still remains the question:

> Why is the sum of eigenvalues related to the sum of diagonal elements?

This question can be answered in two different ways.

-- second term, and the fact that two real-valued polynomials are (numerically) equal for all values of t if and only if their individual coefficients are equal

$$p(t) = \det(A-tI) = (-1)^n \big(t^n - (\text{tr} A) \,t^{n-1} + \dots + (-1)^n \det A\big)\,.$$

Trace is preserved under similarity and every matrix is similar to a Jordan block matrix.

https://math.stackexchange.com/questions/72303/proving-the-trace-of-a-transformation-is-independent-of-the-basis-chosen

Trace definition for orthonormal systems

Trace for PSD

Trace for others

Trace of projectors

---

## The Geometric Picture of Trace

We digress for a moment to reflect on the geometric picture of the determinant of an operator. Consider any orthonormal basis $B$ of a finite-dimensional inner-product space $V$. One can visualise the action of an operator $A$ on $V$ as transforming the cube (of volume $1$) formed by the basis vectors into a parallelopiped. A geometric interpretation of the determinant of an operator $A$ is the (signed) volume of the parallelopiped formed by the transformed basis vectors under action of $A$. Since we started with the cube of normalized basis vectors with volume $1$, the determinant captures the _amount of distortion_ induced by the operator $A$. Keeping in mind the above picture, here are two questions for the reader:

1. What is the significance of the volume of the parallelopiped being **signed**?
2. What is the geometric intepretation of non-invertible/singular operators?

Unlike the determinant, however, it is not immediately clear to us what the physical/geometric interpretation of trace would be. Assuming that the operator has distinct eigenvectors, we know that each individual eigenvalue represents the scaling factor of the corresponding eigenvector.

### Invariant quantities of a bounded operator

[^linmap]: Let $V$ be a vector spaces over some field $\mathbb{F}$. A function $A:V\mapsto V$ is a linear operator on $V$ if $\forall \lambda,\sigma\in\mathbb{F}$ and $\forall v,w\in V$, $A(\lambda v + \sigma w) = \lambda A(v) + \sigma A(w)$.
[^bounded]: A linear operator on an infinite-dimensional vector space will not have a matrix representation if it is not _bounded_. For the purposes of our discussion, requiring that $V$ is finite-dimensional suffices and also avoids unneccessary complications.
[^spectral]: If $A$ is a Hermitian operator (i.e., $A=A^{\dagger}$) on a finite-dimensional vector space $W$, $A=\sum_{i=1}^{n} \lambda_i v_i^{\dagger}\cdot v_i$, where $\lambda_i$ are real eigenvalues of $A$ corresponding to the eigenvectors $v_i$, which form an orthonormal basis of $V$. Note that the eigendecomposition of $A$ is not unique if the eigenvalues are not distinct. Equivalently, $A=V\Lambda V^{\dagger}$, where $V$ is a unitary matrix whose columns are $v_i$ and $\lambda$ is the diagonal matrix with $\lambda_i$ as its entries.
[^innerproduct]: Recall that for general vector spaces, the only property of basis vectors available to us is linear independence. Sometimes, it is desirable if we could add more structure to the basis set. An inner product space is a vector space equipped with the notion of an inner product function. This allows us to capture the notion of _angles_ between vectors. The immediate consequence of note is that we can define a set of orthonormal basis vectors for any vector space equipped with an inner product.
[^quantum]: Interestingly, these restrictions suffice for the purposes of quantum computation and quantum information theory. Proving the above equivalence in full generality is not straightforward (at least for me). We will prove the general version of the above equivalence shortly.
[^charpoly]: The characteristic polynomial of an $n\times n$ matrix $M$ is $p(M) = det(M - \lambda\cdot I_{n\times n})$.
[^factor]: This is known as the [Factor Theorem of polynomials](https://en.wikipedia.org/wiki/Factor_theorem).
