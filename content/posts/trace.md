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

## A third definition of Trace via the Characteristic Polynomial

We start by relating the eigenvalues of an operator to the entries of any of its matrix representations by way of the characteristic polynomial.[^charpoly] Let $A$ be a linear operator on an $n$-dimensional vector space $V$. Let $B$ a basis of $V$ and we denote the matrix representation of $A$ w.r.t. $B$ is $M_B$.

Consider the following matrix representation $M_B = \begin{bmatrix}a_{11}\&\ldots\& a_{1n}\\\ \vdots\&\ddots\&\vdots \\\ a_{i1}\& a_{ii}\& a_{in} \\\ \vdots\&\ddots\&\vdots \\\ a_{1}\&\ldots\& a_{nn}\end{bmatrix}$ of the operator $A$. The characteristic polynomial of $M_B$ is as follows:

$$ p(M*B) = \det(M_B-\lambda\cdot I) = \prod*{i=1}^n (a\_{ii} - \lambda) + \text{terms of degree }\leq n-2$$

$$\implies p(M_B) = (-1)^n \big(\lambda^n - \textcolor{red}{\Bigg(\sum_{i=1}^n a_{ii}\Bigg)}  \,\lambda^{n-1} + \dots + (-1)^n \det M_B\big)$$

$$\implies p(M_B) = (-1)^n \big(\lambda^n - \textcolor{red}{(\text{tr}\, M_B)} \,\lambda^{n-1} + \dots + (-1)^n \det M_B\big)\,.\;\; \text{\small(by Definition 2)}$$

> **Exercise:** Check for yourself that the the coefficient of the degree 0 term of the characteristic polynomial is indeed $\det M_B$.

Recall that if a polynomial $p(x)$ has roots $\\{r_i\\}$, then the polynomials $\\{x-r_i\\}_i$ divide $p(x)$.[^factor] Also recall that by definition, the roots of the characteristic polynomial are the eigenvalues of $A$.[^charpoly] Hence, $p(M_B)=\prod_i (x-\lambda_i)$. By [Vieta's formula()], given a monic polynomial[^monic] $p(x)=x^n+ \sum_{i<n}c_{i}x^{i}$, $c_{n-1}=-\sum_{i} r_i$. Hence, we have

$$ \textcolor{red}{\Bigg(\sum*{i=1}^n a*{ii}\Bigg)} = \textcolor{red}{(\text{tr}\, M*B)} = \sum*{i=1}^{n} \lambda_i.\;\; \text{\small(Hence Definitions 1 and 2 are equivalent)}$$

> **Definition 3 (Trace via Characteristic Polynomial):** The trace of a matrix is the coefficient (upto a sign) of the degree $n-1$ term of its characteristic polynomial.

It is also straightforward to show that the **trace is invariant** using arguments about the polynomial coefficients. A linear algebraic way of showing it is as follows:

Let $B,C$ be different bases of $V$. Denote the matrix representations of $A:V\mapsto V$ w.r.t. $B,C$ as $M_B,M_C$ respectively. By definition, $M_B$ and $M_C$ are **similar** matrices with $S$ being the (invertible) change of basis matrix from $B$ to $C$.

> $$p(M_C) = \det(M_C - \lambda I) = \det(S\cdot M_B\cdot S^{-1} - S\cdot \lambda I\cdot S^{-1})$$

> $$\implies p(M_C)= \det(S\cdot (M_B-\lambda I)\cdot S^{-1}) = \det(S)\cdot p(M_B)\cdot\det(S^{-1})$$

> $$\implies p(M_C)= p(M_B).$$

Hence, the characteristic polynomial is invariant due to change of basis. This implies that **the roots of the characteristic polynomial are invariant under basis transformations**. Therefore, this implies that the trace (the sum of the roots of the characteristic polynomial by Definition 1) is also invariant under basis change.

> We note that the above arguments can also be used to show that the Determinant is invariant under basis change. We now leave a small exercise for the reader to verify: Given a bounded linear operator, it has $n+1$ invariant properties (including the trace and determinant).

## A fourth definition of Trace via the Jordan Normal Form

Since the characteristic polynomial of a bounded operator is invariant under basis change, we consider it to be a canonical representation of the operator. A different way of proving that the trace of a bounded operator is invariant under basis change, is to define the trace via a different canonical/basis-independent representation of linear operators: The **Jordan normal form**.

> Note that this view gives us a definition that allows us to understand why trace is the **sum of the eigenvalues counted along with algebraic multiplicities**.

Let $A:V\mapsto V$ be a linear operator on a finite dimensional vector space $V$ over a field $\mathbb{F}$, s.t. all eigenvalues of $A$ lie in $\mathbb{F}$.[^algclosure] Let the eigenvalues of $A$ along with their algebraic multiplicity be denoted by $\\{\lambda_i, m_i\\}$. The Jordan normal form of $A$ is denoted as $J_A$, and it is a block diagonal matrix as follows:

$$ J*A = \begin{bmatrix} B*{\lambda*1,m1} \& 0\&\ldots \&0 \\\ 0\& B*{\lambda*2,m_2}\&\ldots \&0 \\\ 0\&\ddots\&\ddots\& 0 \\\ 0\&\ldots\&\ldots\& B*{\lambda_k,m_k}\end{bmatrix},$$

where each block $B_{\lambda_i,m_i}$ is an upper-triangular matrix of dimension $m_i\times m_i$, with the diagonal entries each being $\lambda_i$ and every super-diagonal entry (corresponding to the coordinates $(i,i+1)$) being $1$.[^block] We denote the $i$th block of the Jordan matrix below, where we assume that $\lambda_i=\lambda$ and $m_i=3$.

$$ B\_{\lambda,3} = \begin{bmatrix}\lambda\& 1\& 0 \\\ 0\&\lambda\& 1 \\\ 0\&0\&\lambda\end{bmatrix}.$$

---

> **Digression on Jordan Blocks**

Note that $B_{\lambda,3} e_1 = \lambda e_1$, $B_{\lambda,3} e_2 = e_1+\lambda e_2$, and $B_{\lambda,3} e_3 = e_2+\lambda e_3$. Let $B_\lambda=B_{\lambda,3}-\lambda\cdot I$. This gives us $B_{\lambda} e_1 = 0$, implying that $e_1$ is an eigenvector of $B_{\lambda,3}$ with eigenvalue $\lambda$ (recall [^charpoly]), along with the following relations on $e_2$ and $e_3$.

$$B_{\lambda} e_2 = e_1\,\implies {\big(B_{\lambda}\big)}^2 e_2=0\,,$$

$$B_{\lambda} e_3 = e_2\,\implies {\big(B_{\lambda}\big)}^3 e_3=0\,.$$

We denote $e_2$ and $e_3$ as _generalized eigenvectors_ of $B_{\lambda,3}$. One can similarly extend the above argument to any $B_{\lambda_i,m_i}$ to obtain a set of generalized eigenvectors $\\{e_1,\ldots,e_{m_i}\\}$. Note that the set of generalized eigenvectors is independent. This indicates that there is a basis of generalized eigenvectos that spans the subspace on which $B_{\lambda_i,m_i}$ is an operator.[^block]

> The characteristic polynomial tells us the eigenvalues and the dimension of each generalized eigenspace, which is the algebraic multiplicity of the eigenvalue in the Jordan form.

---

We now state the following theorems without proof.

> Every square matrix over an algebraically closed field[^algclosure] is similar to matrix in Jordan normal form, i.e., for every $A$, $\exists S$ s.t. $J=S^{-1}\cdot A\cdot S$ is in Jordan normal form.[^answer]

It turns out that every matrix representation of $A$ is similar to the Jordan Normal Form of $A$.[^jnf] Since we have already seen that the characteristic polynomial of $A$ remains invariant under similarity transformations, we can focus on defining the trace solely via Jordan Normal Forms.

> **Definition 4 (Trace via Jordan Normal Forms):** The trace of an operator is the sum of the diagonal entries of its Jordan Normal Form.

#### To Do's

Trace definition for orthonormal systems

Trace for PSD

Trace for others

Trace of projectors

---

## The Geometric Picture of Trace

We digress for a moment to reflect on the geometric picture of the determinant of an operator. Consider any orthonormal basis $B$ of a finite-dimensional inner-product space $V$. One can visualise the action of an operator $A$ on $V$ as transforming the cube (of volume $1$) formed by the basis vectors into a parallelopiped. A geometric interpretation of the determinant of an operator $A$ is the (signed) volume of the parallelopiped formed by the transformed basis vectors under action of $A$. Since we started with the cube of normalized basis vectors with volume $1$, the determinant captures the _amount of distortion_ induced by the operator $A$. Keeping in mind the above picture, here are two questions for the reader:

1. What is the significance of the volume of the parallelopiped being **signed**?
2. What is the geometric intepretation of non-invertible/singular operators?

Unlike the determinant, however, it is not immediately clear to us what the physical/geometric interpretation of trace would be. Assuming that the operator has distinct eigenvectors, we know that each individual eigenvalue represents the scaling factor of the corresponding eigenvector. We leave it as an open ended exercise for the reader to figure this one out.

[^linmap]: **Linear Operator:** Let $V$ be a vector spaces over some field $\mathbb{F}$. A function $A:V\mapsto V$ is a linear operator on $V$ if $\forall \lambda,\sigma\in\mathbb{F}$ and $\forall v,w\in V$, $A(\lambda v + \sigma w) = \lambda A(v) + \sigma A(w)$.
[^bounded]: **Matrix Representation:** A linear operator on an infinite-dimensional vector space will not have a matrix representation if it is not _bounded_. For the purposes of our discussion, requiring that $V$ is finite-dimensional suffices and also avoids unneccessary complications.
[^spectral]: **Spectral Theorem:** If $A$ is a Hermitian operator (i.e., $A=A^{\dagger}$) on a finite-dimensional vector space $W$, $A=\sum_{i=1}^{n} \lambda_i v_i^{\dagger}\cdot v_i$, where $\lambda_i$ are real eigenvalues of $A$ corresponding to the eigenvectors $v_i$, which form an orthonormal basis of $V$. Note that the eigendecomposition of $A$ is not unique if the eigenvalues are not distinct. Equivalently, $A=V\Lambda V^{\dagger}$, where $V$ is a unitary matrix whose columns are $v_i$ and $\lambda$ is the diagonal matrix with $\lambda_i$ as its entries. This form allows us to succintly express powers of $A$ as $(A)^k=V(\Lambda)^k V^{\dagger}$, for any $k>0$.
[^innerproduct]: **Linear Independence vs Orthonormality:** Recall that for general vector spaces, the only property of basis vectors available to us is linear independence. Sometimes, it is desirable if we could add more structure to the basis set. An inner product space is a vector space equipped with the notion of an inner product function. This allows us to capture the notion of _angles_ between vectors. The immediate consequence of note is that we can define a set of orthonormal basis vectors for any vector space equipped with an inner product.
[^quantum]: Interestingly, these restrictions suffice for the purposes of quantum computation and quantum information theory. Proving the above equivalence in full generality is not straightforward (at least for me). We will prove the general version of the above equivalence shortly.
[^charpoly]: **Characteristic Polynomial and Eigenvalues:** The characteristic polynomial of an $n\times n$ matrix $M$ is $p(M) = det(M - \lambda\cdot I_{n\times n})$. Its roots are the eigenvalues of $M$, since for any $\lambda_i$ s.t. $p(M)=0$, we have $det(M - \lambda_i\cdot I)=0$ which implies that $(M - \lambda_i\cdot I)$ has a non-trivial null space, say $N_i$. Now, any $v_i\in N_i$ is an eigenvector of $M$ with eigenvalue $\lambda_i$ as $(M - \lambda\cdot I)v=0\implies Mv_i=\lambda_i v_i$, implying that $\lambda_i$ is an eigenvalue of $M$ w.r.t. $v_i$. Note that any non-trivial null space of $p(M)$ is an eigenspace of $M$.
[^factor]: This is known as the [Factor Theorem of polynomials](https://en.wikipedia.org/wiki/Factor_theorem).
[^jnf]: **Similarity and JNF:** Let $A=P\cdot J_A \cdot P^{-1}$ and $C=Q\cdot A \cdot Q^{-1}$. Then, $C=T\cdot J_A \cdot T^{-1}$, where $T=QP^{-1}$ is an invertible (change of basis matrix). Hence, $C$ is similar to $J_A$. Also by definition, $J_A$ is the JNF of $C$.
[^answer]: [An elegant proof of the theorem.](https://math.stackexchange.com/a/4901247/120548)
[^block]: **Generalized Eigenspaces:** We note that each block $B_{\lambda_i,m_i}$ of the JNF of a bounded linear operator $A:V\mapsto V$ represents the generalized eigenspace of $A$ corresponding to the eigenvalue $\lambda_i$. If one is familiar with tensor operations, $V = \otimes_{i=1}^{k} B_{\lambda_i,m_i}$.
[^algclosure]: **Algebraic Closure:** If $\mathbb{F}$ is not algebraically closed, i.e., if the eigenvalues of the operator do not belong to $\mathbb{F}$, we can first extend $\mathbb{F}$ to an algebraically closed field (for example, extending $\mathbb{R}$ to $\mathbb{C}$), and then considering the Jordan normal form of the operator, now considered as a matrix whose entries come from this algebraically closed field.
[^monic]: **Monic Polynomial:** In algebra, a monic polynomial is a non-zero univariate polynomial in which the leading coefficient (the coefficient of the nonzero term of highest degree) is equal to $1$.
