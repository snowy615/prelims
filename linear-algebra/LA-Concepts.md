# Linear Algebra — MT Concept Reference
*Stefan Kiefer / Jim Hefferon, University of Oxford. Notes compiled from all lectures.*

---

## Table of Contents
1. [Linear Systems and Gauss's Method](#1-linear-systems-and-gausss-method)
2. [Solution Set Structure: General = Particular + Homogeneous](#2-solution-set-structure-general--particular--homogeneous)
3. [Vector Spaces](#3-vector-spaces)
4. [Linear Independence](#4-linear-independence)
5. [Basis and Dimension](#5-basis-and-dimension)
6. [Rank and the Rank-Nullity Theorem](#6-rank-and-the-rank-nullity-theorem)
7. [Isomorphisms](#7-isomorphisms)
8. [Linear Maps (Homomorphisms)](#8-linear-maps-homomorphisms)
9. [Representing Linear Maps with Matrices](#9-representing-linear-maps-with-matrices)
10. [Matrix Algebra](#10-matrix-algebra)
11. [Change of Basis and Similar Matrices](#11-change-of-basis-and-similar-matrices)
12. [Orthogonality, Projection, and Gram-Schmidt](#12-orthogonality-projection-and-gram-schmidt)
13. [Determinants](#13-determinants)
14. [Eigenvalues and Eigenvectors](#14-eigenvalues-and-eigenvectors)
15. [Diagonalisability](#15-diagonalisability)
16. [Jordan Canonical Form](#16-jordan-canonical-form)
17. [LU and PLU Factorisation](#17-lu-and-plu-factorisation)
18. [QR Factorisation and Least Squares](#18-qr-factorisation-and-least-squares)
19. [Norms](#19-norms)
20. [Singular Value Decomposition (SVD)](#20-singular-value-decomposition-svd)
21. [Key Theorems Cheatsheet](#21-key-theorems-cheatsheet)

---

## 1. Linear Systems and Gauss's Method

### Definitions

**Linear equation:** An equation of the form $`a_1 x_1 + a_2 x_2 + \cdots + a_n x_n = d`$, where the $`a_i`$ are real coefficients and $`x_i`$ are unknowns.

**Linear system:** A collection of linear equations in the same unknowns.

**Solution:** An $`n`$-tuple $`(s_1, \ldots, s_n)`$ satisfying all equations simultaneously. The **solution set** is the set of all solutions.

### Elementary Row Operations

Three legal operations on an augmented matrix $`[A \mid b]`$:

| Operation | Notation | Effect |
|-----------|----------|--------|
| Swap two rows | $`\rho_i \leftrightarrow \rho_j`$ | Changes sign of det |
| Multiply row by nonzero scalar | $`k\rho_i`$ ($`k \neq 0`$) | Multiplies det by $`k`$ |
| Add multiple of row to another | $`k\rho_i + \rho_j \to \rho_j`$ ($`i \neq j`$) | Does not change det |

**Theorem:** Each elementary row operation produces an equivalent system (same solution set).

**Proof (combination operation):** A tuple $`(s_1, \ldots, s_n)`$ satisfies the new $`j`$-th equation $`k(\text{row }i) + (\text{row }j) = 0`$ iff it satisfies both the original $`i`$-th and $`j`$-th equations. The condition $`i \neq j`$ is essential.

**Reversibility:**
- $`\rho_i \leftrightarrow \rho_j`$ reversed by swapping again.
- $`k\rho_i`$ reversed by $`(1/k)\rho_i`$.
- $`k\rho_i + \rho_j`$ reversed by $`-k\rho_i + \rho_j`$.

### Echelon Form and Row Reduction

**Echelon form:** A matrix where:
- Each leading entry (leftmost nonzero in a row) appears strictly to the right of the leading entry in the row above.
- Zero rows are at the bottom.

**Reduced echelon form (RREF):** Echelon form where each leading entry is 1 and is the only nonzero in its column. Achieved by Gauss–Jordan elimination.

**Pivot:** A leading entry in echelon form. A **pivot column** contains a pivot.

**Free variable:** Any variable not corresponding to a pivot column. Free variables are parameters.

### Types of Solution Sets

| Echelon form | Solution type |
|---|---|
| Contradictory row (e.g. $`0 = 1`$) | No solution (empty set) |
| No contradictory row; every variable leads a row | Unique solution |
| No contradictory row; some free variable exists | Infinitely many solutions |

**Criterion for $`2 \times 2`$:** System $`ax + by = j`$, $`cx + dy = k`$ has a unique solution iff $`ad - bc \neq 0`$.

### Worked Examples

**Example 1:** System with unique solution

```math
\begin{array}{cc|c}
  2 & 3 & 13 \\
  1 & 1 & 5
\end{array}
\xrightarrow{-\frac{1}{2}\rho_1+\rho_2}
\begin{array}{cc|c}
  2 & 3 & 13 \\
  0 & -\tfrac{1}{2} & -\tfrac{3}{2}
\end{array}
```

Back-substitute: $`y = 3`$, $`x = 2`$.

**Example 2:** System with a free variable — solution set is a line through a particular point.

**Example 3:** Parameter-dependent system: $`x - y = 1`$, $`(k-3)y = 0`$. No solution if $`k \neq 3`$; infinitely many if $`k = 3`$. Never a unique solution.

**Example 4:** Fitting $`f(x) = ax^2 + bx + c`$ through three points leads to a $`3 \times 3`$ linear system. Given $`f(1)=2`$, $`f(-1)=6`$, $`f(2)=3`$: row reduction gives $`f(x) = x^2 - 2x + 3`$.

### Lemma (Row Swaps are Redundant)

$`\rho_i \leftrightarrow \rho_j`$ can be implemented using only the other two operations:

```math
\rho_i + \rho_j, \qquad -\rho_j + \rho_i, \qquad \rho_i + \rho_j, \qquad -\rho_i
```

So swap is theoretically redundant (but numerically essential for stability).

---

## 2. Solution Set Structure: General = Particular + Homogeneous

### Parametric Form of the Solution Set

After row reduction, free variables become **parameters**. The solution set is written as:

```math
\{p + c_1 h_1 + c_2 h_2 + \cdots + c_k h_k \mid c_i \in \mathbb{R}\}
```

where $`p`$ is a **particular solution** to $`Ax = b`$ and $`h_1, \ldots, h_k`$ are solutions to the **homogeneous system** $`Ax = 0`$.

### Homogeneous System

**Definition:** $`Ax = 0`$ (all right-hand sides zero). Always has the **trivial solution** $`x = 0`$.

**Theorem:** The solution set of $`Ax = 0`$ is a subspace of $`\mathbb{R}^n`$ (it contains $`0`$, and is closed under addition and scalar multiplication).

### General = Particular + Homogeneous

**Theorem:** If $`p`$ is any particular solution to $`Ax = b`$, then every solution is of the form $`p + h`$ where $`h`$ solves $`Ax = 0`$.

**Proof:**
- If $`Ap = b`$ and $`Ah = 0`$, then $`A(p + h) = Ap + Ah = b + 0 = b`$. ✓
- Conversely, if $`As = b`$, then $`A(s - p) = As - Ap = b - b = 0`$, so $`s - p`$ is a homogeneous solution, giving $`s = p + (s-p)`$. ✓

**Key distinction:**
- Homogeneous solution set: a **subspace** (contains $`0`$, flat through the origin).
- Nonhomogeneous solution set: a **coset** (translate of the homogeneous subspace, passes through $`p`$).

### Span

**Definition:** $`\text{Span}(v_1, \ldots, v_k) = \{c_1 v_1 + \cdots + c_k v_k \mid c_i \in \mathbb{R}\}`$.

**Theorem:** The span of any set of vectors is a subspace.

**Theorem:** The solution set of a homogeneous system is a span (possibly $`\{0\}`$).

**Example:** $`x + y - 2z = 0`$ has solution set $`\text{Span}\{(-1,1,0), (2,0,1)\}`$.

### Applications

**Balancing chemical equations:** Set up as a homogeneous system; any nonzero solution gives a valid balance.

**Network flow (Kirchhoff's current law):** Conservation at each node gives a linear system. Flow through a network corresponds to a particular + homogeneous solution.

---

## 3. Vector Spaces

### Definition — Vector Space

A **vector space** is a set $`V`$ with operations $`+`$ (addition) and $`\cdot`$ (scalar multiplication) satisfying for all $`v, w, u \in V`$ and scalars $`r, s`$:

| # | Axiom |
|---|-------|
| 1 | **Closure under $`+`$:** $`v + w \in V`$ |
| 2 | **Commutativity:** $`v + w = w + v`$ |
| 3 | **Associativity:** $`(v + w) + u = v + (w + u)`$ |
| 4 | **Zero:** $`\exists\, 0 \in V`$ with $`v + 0 = v`$ |
| 5 | **Additive inverses:** $`\exists\, {-v}`$ with $`v + (-v) = 0`$ |
| 6 | **Closure under $`\cdot`$:** $`r \cdot v \in V`$ |
| 7 | **Distributivity (vector):** $`r(v + w) = rv + rw`$ |
| 8 | **Distributivity (scalar):** $`(r + s)v = rv + sv`$ |
| 9 | **Associativity of $`\cdot`$:** $`(rs)v = r(sv)`$ |
| 10 | **Identity:** $`1 \cdot v = v`$ |

### Examples of Vector Spaces

| Space | Description |
|-------|-------------|
| $`\mathbb{R}^n`$ | $`n`$-tuples of reals, component-wise operations |
| $`M_{m \times n}(\mathbb{R})`$ | $`m \times n`$ matrices, component-wise operations |
| $`\mathcal{P}_n`$ | Polynomials of degree $`\leq n`$, $`\dim = n+1`$ |
| $`\mathcal{P}`$ | All polynomials, infinite-dimensional |
| $`C^\infty`$ | Infinitely differentiable functions |
| $`\{0\}`$ | Trivial vector space, $`\dim = 0`$ |

**Non-example:** $`\{(x,y) \mid x, y \geq 0\}`$ — not closed under scalar multiplication by $`-1`$.

### Theorems Derived from Axioms

**Zero vector is unique.** If $`0`$ and $`0'`$ both satisfy axiom 4, then $`0 = 0 + 0' = 0'`$.

**Additive inverses are unique.** If $`v + w = 0`$ and $`v + w' = 0`$, then $`w = w'`$.

**$`0 \cdot v = \mathbf{0}`$ for all $`v`$.**
Proof: $`0 \cdot v = (0 + 0) \cdot v = 0 \cdot v + 0 \cdot v`$; cancel $`0 \cdot v`$ from both sides. $`\square`$

**$`(-1) \cdot v = -v`$.**
Proof: $`v + (-1)v = 1v + (-1)v = (1 + (-1))v = 0 \cdot v = \mathbf{0}`$. $`\square`$

**$`r \cdot \mathbf{0} = \mathbf{0}`$ for all scalars $`r`$.**

### Subspaces

**Definition — Subspace:** A subset $`S \subseteq V`$ is a **subspace** if it is itself a vector space under the inherited operations.

**Subspace Test (efficient):** A nonempty subset $`S`$ is a subspace iff it is closed under linear combinations:

```math
v, w \in S,\ r, s \in \mathbb{R} \implies rv + sw \in S
```

Equivalently: $`S \neq \emptyset`$, closed under $`+`$, closed under scalar multiplication.

**Examples:**
- $`\{0\}`$ and $`V`$ itself are always subspaces.
- Solution set of $`Ax = 0`$ is a subspace of $`\mathbb{R}^n`$.
- $`\{(x,y,z) \mid x + y + z = 0\}`$ is a subspace of $`\mathbb{R}^3`$.

**Non-examples:**
- $`\{(x,y) \mid x + y = 1\}`$ — does not contain $`\mathbf{0}`$.
- $`\{(x,y) \mid x \geq 0\}`$ — not closed under scalar multiplication by negatives.

---

## 4. Linear Independence

### Definition

A set $`\{v_1, \ldots, v_k\}`$ is **linearly independent** if the only solution to

```math
c_1 v_1 + c_2 v_2 + \cdots + c_k v_k = \mathbf{0}
```

is $`c_1 = c_2 = \cdots = c_k = 0`$.

Otherwise the set is **linearly dependent**.

### Equivalent Characterisations

1. A set is **dependent** iff at least one vector is a linear combination of the others.
2. A set is **dependent** iff it contains a vector that can be removed without shrinking the span.
3. A set in $`\mathbb{R}^n`$ is independent iff the homogeneous system with these vectors as columns has only the trivial solution (equivalently, all columns are pivot columns after row reduction).

### Key Facts

- Any set containing $`\mathbf{0}`$ is dependent: $`1 \cdot \mathbf{0} = \mathbf{0}`$.
- A single nonzero vector is independent.
- Any set of more than $`n`$ vectors in $`\mathbb{R}^n`$ is dependent.
- A set of exactly $`n`$ vectors in $`\mathbb{R}^n`$ is independent iff it spans $`\mathbb{R}^n`$.

### Test via Row Reduction

Form the matrix $`A`$ with the vectors as **columns**. Row reduce. The set is linearly independent iff $`Ax = \mathbf{0}`$ has only the trivial solution, i.e. every column is a pivot column.

**Examples:**
- $`\{(1,2), (3,6)\}`$ is dependent: $`3(1,2) - 1(3,6) = (0,0)`$.
- $`\{e_1, e_2, e_3\}`$ (standard basis in $`\mathbb{R}^3`$) is independent.
- $`\{(1,0,0), (0,1,0), (0,0,1), (1,1,1)\}`$ in $`\mathbb{R}^3`$: four vectors in $`\mathbb{R}^3`$ — automatically dependent.

### Extension Lemma

If $`\{v_1, \ldots, v_k\}`$ is independent and $`v_{k+1} \notin \text{Span}\{v_1, \ldots, v_k\}`$, then $`\{v_1, \ldots, v_{k+1}\}`$ is independent.

---

## 5. Basis and Dimension

### Basis

**Definition:** A **basis** for $`V`$ is a linearly independent spanning set.

Equivalently, $`B = \{b_1, \ldots, b_n\}`$ is a basis iff every $`v \in V`$ can be written **uniquely** as $`v = c_1 b_1 + \cdots + c_n b_n`$.

**Unique representation theorem:** If $`v = \sum c_i b_i = \sum d_i b_i`$, then $`\sum(c_i - d_i)b_i = \mathbf{0}`$, so independence gives $`c_i = d_i`$ for all $`i`$. $`\square`$

**Standard bases:**
- $`\mathbb{R}^n`$: $`\{e_1, \ldots, e_n\}`$ where $`e_i`$ has $`1`$ in position $`i`$, $`0`$ elsewhere.
- $`\mathcal{P}_n`$: $`\{1, x, x^2, \ldots, x^n\}`$ — dimension $`n+1`$.
- $`M_{m \times n}`$: unit matrices $`E_{ij}`$ (single $`1`$ in position $`(i,j)`$) — dimension $`mn`$.

**Coordinates:** If $`B = (b_1, \ldots, b_n)`$ is an ordered basis, the **coordinate vector** of $`v`$ w.r.t. $`B`$ is $`\text{Rep}_B(v) = (c_1, \ldots, c_n)^T`$ where $`v = \sum c_i b_i`$.

### Exchange Lemma

**Theorem (Exchange Lemma):** If $`\{b_1, \ldots, b_n\}`$ is a basis and $`v = c_1 b_1 + \cdots + c_n b_n`$ with $`c_j \neq 0`$, then replacing $`b_j`$ by $`v`$ gives another basis:

```math
\{b_1, \ldots, b_{j-1},\, v,\, b_{j+1}, \ldots, b_n\}
```

**Proof:** Let $`B' = \{b_1, \ldots, b_{j-1}, v, b_{j+1}, \ldots, b_n\}`$.

*Spanning:* Since $`c_j \neq 0`$, we can solve for $`b_j`$:

```math
b_j = c_j^{-1}\left(v - \sum_{i \neq j} c_i b_i\right)
```

So $`b_j \in \text{Span}(B')`$. Since every vector in $`V`$ is a linear combination of $`b_1, \ldots, b_n`$, and we can replace $`b_j`$ using the equation above, every vector is in $`\text{Span}(B')`$.

*Independence:* Suppose $`d_1 b_1 + \cdots + d_{j-1}b_{j-1} + d_j v + d_{j+1}b_{j+1} + \cdots + d_n b_n = \mathbf{0}`$.  
Substitute $`v = \sum_i c_i b_i`$:

```math
\sum_{i \neq j}(d_i + d_j c_i)b_i + d_j c_j b_j = \mathbf{0}
```

Since $`\{b_i\}`$ is a basis (independent), all coefficients are zero. From the $`b_j`$-term: $`d_j c_j = 0`$; since $`c_j \neq 0`$, $`d_j = 0`$. Then each $`d_i + d_j c_i = d_i = 0`$. $`\square`$

### Invariance of Basis Size (Dimension Well-Defined)

**Theorem:** All bases for a finite-dimensional vector space $`V`$ have the same number of elements.

**Proof:** Suppose $`B = \{b_1, \ldots, b_n\}`$ and $`D = \{d_1, \ldots, d_m\}`$ are both bases. Apply the exchange lemma repeatedly: $`d_1`$ can replace some $`b_j`$ in $`B`$; then $`d_2`$ can replace another; after $`m`$ exchanges, all $`d_i`$ have replaced some $`b_i`$'s. If $`m > n`$, some $`d_i`$ would already be in the span of previous $`d_j`$'s — contradiction with $`D`$ being independent. So $`m \leq n`$; by symmetry $`n \leq m`$, giving $`m = n`$. $`\square`$

### Dimension

**Definition:** $`\dim(V)`$ = number of elements in any basis. $`\dim(\{0\}) = 0`$.

| Space | Dimension |
|-------|-----------|
| $`\mathbb{R}^n`$ | $`n`$ |
| $`\mathcal{P}_n`$ | $`n+1`$ |
| $`M_{m \times n}(\mathbb{R})`$ | $`mn`$ |
| $`\mathcal{P}`$ | $`\infty`$ |

### Corollaries (Equal-Dimension Shortcuts)

In a space of dimension $`n`$:
- Any **$`n`$ independent vectors** form a basis.
- Any **$`n`$ vectors that span** form a basis.
- Any spanning set **contains** a basis (remove dependent vectors).
- Any independent set **extends** to a basis (add vectors from standard basis as needed).
- If $`W \subseteq V`$ is a subspace, then $`\dim(W) \leq \dim(V)`$, with equality iff $`W = V`$.

**Proof — Any spanning set contains a basis:**  
Let $`S = \{s_1, \ldots, s_m\}`$ span $`V`$. If $`S`$ is independent, it is already a basis. Otherwise some $`s_k`$ is a linear combination of the others; remove it — the remaining set still spans $`V`$ (the span did not shrink). Repeat until independent; the result is a basis. $`\square`$

**Proof — Any independent set extends to a basis:**  
Let $`T = \{t_1, \ldots, t_k\}`$ be independent in $`V`$ with $`\dim V = n`$. If $`\text{Span}(T) = V`$ then $`T`$ is a basis. Otherwise there exists $`v \in V \setminus \text{Span}(T)`$; by the Extension Lemma $`T \cup \{v\}`$ is independent. Repeat until we have $`n`$ independent vectors, which must span $`V`$ (by invariance of dimension). $`\square`$

**Proof — $`\dim W < \dim V`$ for proper subspaces:**  
Any basis of $`W`$ is an independent set in $`V`$ of size $`\dim W`$. Extend it to a basis of $`V`$; if $`W \subsetneq V`$ then at least one new vector was added, so $`\dim W < \dim V`$. $`\square`$

---

## 6. Rank and the Rank-Nullity Theorem

### Definitions

For a matrix $`A`$ of size $`m \times n`$:

**Rank:** $`\text{rank}(A)`$ = number of pivot columns after row reduction = $`\dim(\text{Col}(A))`$ = $`\dim(\text{Row}(A))`$.

**Nullity:** $`\text{nullity}(A) = \dim(\text{Null}(A))`$ where $`\text{Null}(A) = \{x \mid Ax = \mathbf{0}\}`$.

**Column space:** $`\text{Col}(A) = \{Ax \mid x \in \mathbb{R}^n\}`$ — span of the columns.

**Row space:** $`\text{Row}(A)`$ — span of the rows.

**Note:** $`\text{rank}(A) = \text{rank}(A^T)`$ (row rank = column rank).

### Rank-Nullity Theorem

**Theorem:** For any $`m \times n`$ matrix $`A`$:

```math
\text{rank}(A) + \text{nullity}(A) = n
```

Equivalently, for a linear map $`h: V \to W`$ with $`V`$ finite-dimensional:

```math
\dim(\text{Range}(h)) + \dim(\text{Null}(h)) = \dim(V)
```

**Proof:** Let $`\{k_1, \ldots, k_p\}`$ be a basis for $`\text{Null}(h)`$. Extend to a basis $`\{k_1, \ldots, k_p, b_1, \ldots, b_q\}`$ for $`V`$ (so $`p + q = \dim V`$). Claim $`\{h(b_1), \ldots, h(b_q)\}`$ is a basis for $`\text{Range}(h)`$:

- *Spanning:* For any $`h(v) \in \text{Range}(h)`$, write $`v = \sum a_i k_i + \sum c_j b_j`$, so $`h(v) = \sum c_j h(b_j)`$.
- *Independence:* Suppose $`\sum c_j h(b_j) = \mathbf{0}`$. Then $`h(\sum c_j b_j) = \mathbf{0}`$, so $`\sum c_j b_j \in \text{Null}(h) = \text{Span}\{k_i\}`$. Write $`\sum c_j b_j = \sum d_i k_i`$; since $`\{k_i, b_j\}`$ is a basis, all $`c_j = 0`$.

Thus $`\text{rank}(h) = q = \dim(V) - p = \dim(V) - \text{nullity}(h)`$. $`\square`$

### Consequences

- $`h`$ is injective (one-to-one) $`\iff`$ $`\text{Null}(h) = \{0\}`$ $`\iff`$ $`\text{nullity}(h) = 0`$.
- $`h: V \to W`$ with $`\dim V = \dim W`$ is injective $`\iff`$ surjective $`\iff`$ isomorphism.
- Solution set of $`Ax = \mathbf{0}`$ has dimension $`n - \text{rank}(A)`$.
- $`Ax = b`$ has a solution $`\iff`$ $`\text{rank}(A) = \text{rank}([A \mid b])`$.

---

## 7. Isomorphisms

### Definition

A map $`f: V \to W`$ is an **isomorphism** if:
1. $`f`$ is a **bijection** (injective and surjective).
2. $`f`$ **preserves structure:** $`f(v + w) = f(v) + f(w)`$ and $`f(cv) = c f(v)`$.

$`V`$ and $`W`$ are **isomorphic** ($`V \cong W`$) if an isomorphism exists.

### Properties of Isomorphisms

- $`f(\mathbf{0}_V) = \mathbf{0}_W`$. **Proof:** $`f(\mathbf{0}) = f(0 \cdot v) = 0 \cdot f(v) = \mathbf{0}`$. $`\square`$
- $`f(-v) = -f(v)`$.
- $`f`$ preserves linear combinations: $`f(\sum c_i v_i) = \sum c_i f(v_i)`$.
- $`f`$ maps bases to bases: if $`\{b_i\}`$ is a basis for $`V`$, then $`\{f(b_i)\}`$ is a basis for $`W`$.

**Proof (bases to bases):**
- *Spanning:* Any $`w = f(v) = f(\sum c_i b_i) = \sum c_i f(b_i)`$.
- *Independence:* If $`\sum c_i f(b_i) = \mathbf{0}`$, then $`f(\sum c_i b_i) = \mathbf{0}_W = f(\mathbf{0}_V)`$, so by injectivity $`\sum c_i b_i = \mathbf{0}_V`$, giving all $`c_i = 0`$. $`\square`$

### Isomorphism is an Equivalence Relation

- *Reflexive:* Identity map $`\text{id}_V`$.
- *Symmetric:* If $`f: V \to W`$ is an isomorphism, $`f^{-1}: W \to V`$ is also an isomorphism.
- *Transitive:* Composition of isomorphisms is an isomorphism.

### Dimension Characterises Isomorphism

**Theorem:** Two finite-dimensional real vector spaces are isomorphic **iff** they have the same dimension.

**Proof ($`\Rightarrow`$):** Isomorphisms map bases to bases, so $`\dim V = \dim W`$.

**Proof ($`\Leftarrow`$):** If $`\dim V = \dim W = n`$, choose bases $`B = \{b_i\}`$ for $`V`$ and $`D = \{d_i\}`$ for $`W`$. Define $`f(c_1 b_1 + \cdots + c_n b_n) = c_1 d_1 + \cdots + c_n d_n`$. This is well-defined (unique representations), linear, and bijective. $`\square`$

**Corollary:** Every $`n`$-dimensional real vector space is isomorphic to $`\mathbb{R}^n`$.

---

## 8. Linear Maps (Homomorphisms)

### Definition

A function $`h: V \to W`$ is **linear** (a **linear map** / **homomorphism**) if:

```math
h(c_1 v_1 + c_2 v_2) = c_1 h(v_1) + c_2 h(v_2) \quad \text{for all } v_i \in V,\, c_i \in \mathbb{R}
```

Equivalently: $`h(u + v) = h(u) + h(v)`$ and $`h(cv) = ch(v)`$.

**Key consequence:** $`h(\mathbf{0}) = \mathbf{0}`$ (set $`c_1 = c_2 = 0`$).

### Examples

| Map | Linear? |
|-----|---------|
| Zero map: $`h(v) = \mathbf{0}`$ | Yes |
| Identity: $`h(v) = v`$ | Yes |
| Differentiation $`\frac{d}{dx}`$ on $`\mathcal{P}`$ | Yes |
| Integration $`\int_a^b`$ on continuous functions | Yes |
| Projection $`(x,y,z) \mapsto (x,y,0)`$ | Yes |
| Rotation by $`\theta`$ in $`\mathbb{R}^2`$ | Yes |
| Translation $`v \mapsto v + c`$ ($`c \neq 0`$) | **No** — $`h(\mathbf{0}) = c \neq \mathbf{0}`$ |
| Squaring $`(x \mapsto x^2)`$ | **No** — not additive |

### Linear Maps are Determined by Basis Values

**Theorem:** A linear map is completely determined by its values on a basis.

If $`\{b_1, \ldots, b_n\}`$ is a basis for $`V`$ and $`v = \sum c_i b_i`$, then $`h(v) = \sum c_i h(b_i)`$.

**Existence and uniqueness:** Given any basis $`\{b_i\}`$ for $`V`$ and any vectors $`w_1, \ldots, w_n \in W`$, there exists a unique linear map $`h: V \to W`$ with $`h(b_i) = w_i`$.

### Range Space and Null Space

**Definition — Range (Image):** $`\mathcal{R}(h) = \{h(v) \mid v \in V\}`$

**Definition — Null Space (Kernel):** $`\mathcal{N}(h) = \{v \in V \mid h(v) = \mathbf{0}\}`$

**Theorem:** $`\mathcal{R}(h)`$ is a subspace of $`W`$; $`\mathcal{N}(h)`$ is a subspace of $`V`$.

**Proof for $`\mathcal{N}(h)`$:** Contains $`\mathbf{0}`$ (since $`h(\mathbf{0}) = \mathbf{0}`$). If $`h(v) = h(w) = \mathbf{0}`$, then $`h(v + w) = h(v) + h(w) = \mathbf{0}`$, and $`h(cv) = ch(v) = \mathbf{0}`$. $`\square`$

**Rank and Nullity:**
- $`\text{rank}(h) = \dim(\mathcal{R}(h))`$
- $`\text{nullity}(h) = \dim(\mathcal{N}(h))`$
- **Rank-Nullity:** $`\text{rank}(h) + \text{nullity}(h) = \dim(V)`$ (proved in §6)

### Inverse Images

**Definition:** For $`w \in W`$, the **inverse image** (preimage) is $`h^{-1}(w) = \{v \in V \mid h(v) = w\}`$.

**Theorem:** $`h^{-1}(w)`$ is either empty (if $`w \notin \mathcal{R}(h)`$) or a coset of $`\mathcal{N}(h)`$:

```math
h^{-1}(w) = \{p\} + \mathcal{N}(h) = \{p + n \mid n \in \mathcal{N}(h)\}
```

where $`p`$ is any particular element with $`h(p) = w`$.

**Proof:** This is the homomorphism analogue of "general = particular + homogeneous."
- If $`h(p) = w`$ and $`h(n) = \mathbf{0}`$, then $`h(p + n) = h(p) + h(n) = w + \mathbf{0} = w`$. So $`p + \mathcal{N}(h) \subseteq h^{-1}(w)`$.
- Conversely, if $`h(s) = w`$, then $`h(s - p) = h(s) - h(p) = w - w = \mathbf{0}`$, so $`s - p \in \mathcal{N}(h)`$, giving $`s = p + (s-p) \in p + \mathcal{N}(h)`$. $`\square`$

**Lemma (Preimage of a subspace):** If $`X \subseteq W`$ is a subspace of $`W`$, then $`h^{-1}(X) = \{v \in V \mid h(v) \in X\}`$ is a subspace of $`V`$.

**Proof:** Contains $`\mathbf{0}`$: $`h(\mathbf{0}_V) = \mathbf{0}_W \in X`$. Closed: if $`h(u), h(v) \in X`$, then $`h(ru + sv) = rh(u) + sh(v) \in X`$ (since $`X`$ is a subspace). $`\square`$

**Geometric picture:** The domain $`V`$ is partitioned into parallel "fibres" $`h^{-1}(w)`$ for each $`w \in \mathcal{R}(h)`$ — all cosets of $`\mathcal{N}(h)`$. The map $`h`$ collapses each fibre to a single point. This is why $`\dim V = \dim(\mathcal{R}(h)) + \dim(\mathcal{N}(h))`$: the domain has a "fibre direction" of dimension $`\text{nullity}(h)`$ and a "base direction" of dimension $`\text{rank}(h)`$.

---

## 9. Representing Linear Maps with Matrices

### The Matrix of a Linear Map

**Setup:** Choose ordered bases $`B = (b_1, \ldots, b_n)`$ for $`V`$ and $`D = (d_1, \ldots, d_m)`$ for $`W`$.

**Definition:** The matrix $`\text{Rep}_{B,D}(h)`$ is the $`m \times n`$ matrix whose **$`j`$-th column** is $`\text{Rep}_D(h(b_j))`$ — the coordinate vector of $`h(b_j)`$ in basis $`D`$.

If $`h(b_j) = \sum_i h_{ij} d_i`$, then $`\text{Rep}_{B,D}(h)_{ij} = h_{ij}`$.

**Computation rule:**

```math
\text{Rep}_D(h(v)) = \text{Rep}_{B,D}(h) \cdot \text{Rep}_B(v)
```

This is ordinary matrix-vector multiplication.

**Example:** $`h: \mathbb{R}^2 \to \mathbb{R}^3`$, $`(x,y) \mapsto (x+y, 2x, x-y)`$. With standard bases:

```math
h(e_1) = (1,2,1), \qquad h(e_2) = (1,0,-1)
```

```math
\text{Rep}(h) = \begin{pmatrix}1 & 1 \\ 2 & 0 \\ 1 & -1\end{pmatrix}
```

### Composition of Linear Maps is Linear

**Lemma:** If $`g: U \to V`$ and $`h: V \to W`$ are linear, then $`h \circ g: U \to W`$ is linear.

**Proof:** For any $`u_1, u_2 \in U`$ and scalars $`c_1, c_2`$:

```math
(h \circ g)(c_1 u_1 + c_2 u_2) = h(g(c_1 u_1 + c_2 u_2)) = h(c_1 g(u_1) + c_2 g(u_2))
```

```math
= c_1 h(g(u_1)) + c_2 h(g(u_2)) = c_1(h \circ g)(u_1) + c_2(h \circ g)(u_2). \quad\square
```

### Composition = Matrix Multiplication

If $`g: U \to V`$ has matrix $`G`$ and $`h: V \to W`$ has matrix $`H`$ (with compatible bases), then $`h \circ g: U \to W`$ has matrix $`HG`$.

**Why is matrix multiplication defined as row-times-column?** To represent $`h \circ g`$, the $`j`$-th column of $`\text{Rep}(h \circ g)`$ must be $`\text{Rep}_D(h(g(b_j)))`$.

Write $`g(b_j) = \sum_k G_{kj} e_k`$ (the $`j`$-th column of $`G`$ gives coordinates in the $`e_k`$ basis). Then:

```math
h(g(b_j)) = h\left(\sum_k G_{kj} e_k\right) = \sum_k G_{kj} h(e_k)
```

The $`i`$-th coordinate of $`h(g(b_j))`$ in basis $`D`$ is $`\sum_k H_{ik} G_{kj}`$ — exactly the $`(i,j)`$-entry of $`HG`$. $`\square`$

This is the *motivation* for the definition of matrix multiplication:

```math
(HG)_{ij} = \sum_k H_{ik} G_{kj}
```

### Arrow Diagrams

An **arrow diagram** is a visual representation of a composition of maps, written as $`U \xrightarrow{g} V \xrightarrow{h} W`$, with matrices $`G`$ and $`H`$ beneath each arrow:

```math
\mathbb{R}^n \xrightarrow{G} \mathbb{R}^m \xrightarrow{H} \mathbb{R}^p
```

For change of basis: if $`t: V \to V`$ has matrix $`T`$ w.r.t. basis $`B`$, and $`M`$ is the change-of-basis matrix from $`B'`$ to $`B`$, then the matrix w.r.t. $`B'`$ is $`M^{-1}TM`$. The arrow diagram shows: convert to $`B`$-coordinates (multiply by $`M`$), apply $`T`$, then convert back to $`B'`$-coordinates (multiply by $`M^{-1}`$).

### Rank of a Map Equals Rank of its Matrix

**Theorem:** For any linear map $`h: V \to W`$ and any choice of bases, $`\text{rank}(h) = \text{rank}(H)`$ where $`H = \text{Rep}_{B,D}(h)`$.

**Proof:** The columns of $`H`$ are the coordinate vectors $`\text{Rep}_D(h(b_j))`$. Since $`\text{Rep}_D`$ is an isomorphism (preserves independence and spanning), the column space of $`H`$ has the same dimension as $`\mathcal{R}(h)`$. $`\square`$

**Corollary 2.6:**
- $`h`$ is **onto** $`\iff`$ $`\text{rank}(H) = \dim(W) =`$ number of rows of $`H`$.
- $`h`$ is **one-to-one** $`\iff`$ $`\text{rank}(H) = \dim(V) =`$ number of columns of $`H`$.

### Nonsingular Maps and Matrices

**Definition:** A linear map $`h: V \to W`$ is **nonsingular** if it is one-to-one and onto (i.e., an isomorphism). Otherwise it is **singular**.

**Definition:** An $`n \times n`$ matrix $`H`$ is **nonsingular** if $`\det(H) \neq 0`$ (equivalently, $`H`$ represents a nonsingular map).

**Lemma 2.9:** A linear map $`h: V \to W`$ is nonsingular if and only if it is represented by a square nonsingular matrix (for any choice of bases).

**Proof:** If $`h`$ is nonsingular then $`\dim V = \dim W`$ (isomorphic spaces have equal dimension), so $`H`$ is square. By Corollary 2.6, one-to-one means rank = cols = $`n`$, and onto means rank = rows = $`n`$; so $`\text{rank}(H) = n`$, i.e., $`H`$ is nonsingular. Conversely, square nonsingular $`H`$ has rank = $`n`$ = rows = cols, so $`h`$ is both one-to-one and onto. $`\square`$

### Rotation Matrix Derivation

The counter-clockwise rotation $`t_\theta: \mathbb{R}^2 \to \mathbb{R}^2`$ by angle $`\theta`$ sends:

```math
e_1 = (1,0) \mapsto (\cos\theta, \sin\theta), \qquad e_2 = (0,1) \mapsto (-\sin\theta, \cos\theta)
```

(unit vectors mapped to points on the unit circle at angles $`\theta`$ and $`\theta + 90°`$).

Since the matrix columns are $`t_\theta(e_1)`$ and $`t_\theta(e_2)`$:

```math
\text{Rep}(t_\theta) = \begin{pmatrix}\cos\theta & -\sin\theta \\ \sin\theta & \cos\theta\end{pmatrix}
```

### Change of Basis for Representations

If $`B, B'`$ are bases for $`V`$ and $`D, D'`$ for $`W`$, and $`h`$ has matrix $`H = \text{Rep}_{B,D}(h)`$, then:

```math
\text{Rep}_{B',D'}(h) = P^{-1} H Q
```

where $`Q = \text{Rep}_{B',B}(\text{id}_V)`$ and $`P = \text{Rep}_{D',D}(\text{id}_W)`$.

---

## 10. Matrix Algebra

### Matrix Operations

**Addition:** $`(A + B)_{ij} = A_{ij} + B_{ij}`$. Defined for same-size matrices only.

**Scalar multiplication:** $`(cA)_{ij} = c \cdot A_{ij}`$.

**Matrix multiplication:** If $`A`$ is $`m \times p`$ and $`B`$ is $`p \times n`$, then $`AB`$ is $`m \times n`$:

```math
(AB)_{ij} = \sum_{k=1}^p A_{ik} B_{kj}
```

The $`(i,j)`$-entry is the **dot product** of row $`i`$ of $`A`$ with column $`j`$ of $`B`$.

**Properties:**
- **Not commutative:** $`AB \neq BA`$ in general.
- **Associative:** $`(AB)C = A(BC)`$.
- **Distributive:** $`A(B+C) = AB + AC`$; $`(A+B)C = AC + BC`$.
- $`M_{m \times n}`$ is a vector space of dimension $`mn`$ under $`+`$ and scalar multiplication.

### Transpose

$`(A^T)_{ij} = A_{ji}`$ — swap rows and columns.

**Properties:**
- $`(A + B)^T = A^T + B^T`$
- $`(AB)^T = B^T A^T`$
- $`(A^T)^T = A`$
- $`(cA)^T = cA^T`$

**Proof of $`(AB)^T = B^T A^T`$:**

```math
((AB)^T)_{ij} = (AB)_{ji} = \sum_k A_{jk} B_{ki} = \sum_k (B^T)_{ik}(A^T)_{kj} = (B^T A^T)_{ij}. \quad\square
```

### Matrix Inverses

**Definition:** A square matrix $`A`$ is **invertible** if $`\exists A^{-1}`$ with $`AA^{-1} = A^{-1}A = I`$.

**Theorem:** The inverse is unique (if it exists).

**Properties:**
- $`(A^{-1})^{-1} = A`$
- $`(AB)^{-1} = B^{-1}A^{-1}`$
- $`(A^T)^{-1} = (A^{-1})^T`$

**Proof of $`(AB)^{-1} = B^{-1}A^{-1}`$:**
$`(AB)(B^{-1}A^{-1}) = A(BB^{-1})A^{-1} = AIA^{-1} = AA^{-1} = I`$. $`\square`$

**Computing the inverse:** Row reduce $`[A \mid I]`$ to $`[I \mid A^{-1}]`$.

**Equivalent conditions for invertibility of $`A`$ ($`n \times n`$):**
1. $`\det(A) \neq 0`$
2. $`\text{rank}(A) = n`$
3. $`\text{Null}(A) = \{0\}`$
4. Rows/columns are linearly independent
5. $`Ax = b`$ has a unique solution for every $`b`$

---

## 11. Change of Basis and Similar Matrices

### Change of Basis Matrix

If $`B = (b_1, \ldots, b_n)`$ and $`B' = (b'_1, \ldots, b'_n)`$ are two ordered bases for $`V`$, the **change of basis matrix from $`B'`$ to $`B`$** is:

```math
M = \text{Rep}_{B', B}(\text{id})
```

Its $`j`$-th column is $`\text{Rep}_B(b'_j)`$ — coordinates of new basis vectors in the old basis.

**Usage:** $`\text{Rep}_{B}(v) = M \cdot \text{Rep}_{B'}(v)`$ for all $`v`$.

**Invertibility:** The change-of-basis matrix is always invertible; $`M^{-1}`$ changes from $`B`$ to $`B'`$.

### Similar Matrices

**Definition:** $`A`$ and $`B`$ are **similar** if $`B = P^{-1}AP`$ for some invertible $`P`$.

**Meaning:** $`A`$ and $`B`$ represent the same linear transformation under different bases. $`P`$ is the change-of-basis matrix.

**Shared invariants:** Similar matrices have the same:
- Rank
- Determinant
- Trace ($`\text{tr}(A) = \sum_i a_{ii}`$)
- Characteristic polynomial
- Eigenvalues

### Diagonalisation as Change of Basis

If $`t: V \to V`$ has eigenvectors $`v_1, \ldots, v_n`$ with eigenvalues $`\lambda_1, \ldots, \lambda_n`$, choosing $`B = (v_1, \ldots, v_n)`$ as basis makes $`\text{Rep}_B(t)`$ diagonal:

```math
\text{Rep}_B(t) = \text{diag}(\lambda_1, \ldots, \lambda_n)
```

**Example:** $`t: \mathbb{R}^2 \to \mathbb{R}^2`$, $`(x,y) \mapsto (3x+y, x+3y)`$.
- Standard basis matrix:

```math
\begin{pmatrix}3 & 1 \\ 1 & 3\end{pmatrix}
```

- Eigenvectors $`(1,1)`$ (eigenvalue $`4`$) and $`(1,-1)`$ (eigenvalue $`2`$) give diagonal representation:

```math
\begin{pmatrix}4 & 0 \\ 0 & 2\end{pmatrix}
```

---

## 12. Orthogonality, Projection, and Gram-Schmidt

### Dot Product and Orthogonality

**Dot product:** $`v \cdot w = v^T w = \sum_i v_i w_i`$ for $`v, w \in \mathbb{R}^n`$.

**Length:** $`\|v\| = \sqrt{v \cdot v} = \sqrt{\sum_i v_i^2}`$.

**Orthogonal:** $`v \perp w`$ iff $`v \cdot w = 0`$.

**Orthogonal complement:** $`S^\perp = \{w \mid w \cdot v = 0 \text{ for all } v \in S\}`$ — a subspace.

**Orthogonal set:** Pairwise orthogonal nonzero vectors are linearly independent.

**Proof:** Suppose $`c_1 v_1 + \cdots + c_k v_k = \mathbf{0}`$. Take dot product with $`v_i`$: $`c_i(v_i \cdot v_i) = 0`$. Since $`v_i \neq \mathbf{0}`$, $`v_i \cdot v_i > 0`$, so $`c_i = 0`$. $`\square`$

**Orthonormal basis:** A basis $`\{q_1, \ldots, q_k\}`$ where $`q_i \cdot q_j = \delta_{ij}`$ (each vector has unit length; all pairs perpendicular).

### Projection onto a Subspace

**Projection onto a line spanned by $`s`$:**

```math
\text{proj}_s(v) = \frac{v \cdot s}{s \cdot s} \, s
```

**Projection onto subspace $`S`$ with orthogonal basis $`\{b_1, \ldots, b_k\}`$:**

```math
\text{proj}_S(v) = \frac{v \cdot b_1}{b_1 \cdot b_1} b_1 + \cdots + \frac{v \cdot b_k}{b_k \cdot b_k} b_k
```

**Key property:** $`v - \text{proj}_S(v)`$ is orthogonal to every vector in $`S`$.

**Orthogonal decomposition:** Any $`v \in V`$ decomposes uniquely as $`v = s + n`$ where $`s \in S`$ and $`n \in S^\perp`$.

### Gram-Schmidt Orthogonalisation

Given linearly independent vectors $`a_1, \ldots, a_n`$, produce orthogonal vectors $`q_1, \ldots, q_n`$ with the same span:

```math
q_1 := a_1
```

```math
q_2 := a_2 - \frac{q_1 \cdot a_2}{q_1 \cdot q_1} q_1
```

```math
q_3 := a_3 - \frac{q_1 \cdot a_3}{q_1 \cdot q_1} q_1 - \frac{q_2 \cdot a_3}{q_2 \cdot q_2} q_2
```

```math
q_k := a_k - \sum_{i=1}^{k-1} \frac{q_i \cdot a_k}{q_i \cdot q_i} q_i
```

At each step: subtract the projection of $`a_k`$ onto the span of already-computed $`q`$'s.

**Normalise:** $`\hat{q}_i = q_i / \|q_i\|`$ to get an orthonormal basis.

### Projection Matrix

If $`W = \text{Col}(A)`$ and $`A`$ has orthonormal columns ($`Q^TQ = I`$):

```math
P = QQ^T
```

In general (columns not orthonormal):

```math
P = A(A^TA)^{-1}A^T
```

**Properties of a projection matrix:**
- $`P^2 = P`$ (idempotent) — projecting twice is the same as projecting once.
- $`P^T = P`$ (symmetric).
- $`\text{rank}(P) = \dim(W)`$.

---

## 13. Determinants

### Axiomatic Definition

The **determinant** $`\det: M_{n \times n}(\mathbb{R}) \to \mathbb{R}`$ is the unique function satisfying:

1. **Multilinearity:** $`\det`$ is linear in each row separately.
2. **Alternating:** Swapping any two rows changes the sign.
3. **Normalisation:** $`\det(I) = 1`$.

**Consequence of alternating:** If two rows are equal, $`\det(A) = -\det(A) \Rightarrow \det(A) = 0`$.

### Effect of Row Operations on det

| Row operation | Effect on $`\det`$ |
|---|---|
| Swap rows $`\rho_i \leftrightarrow \rho_j`$ | Multiply by $`-1`$ |
| Scale: $`k\rho_i`$ ($`k \neq 0`$) | Multiply by $`k`$ |
| Combination: $`k\rho_i + \rho_j`$ ($`i \neq j`$) | No change |

### $`2 \times 2`$ Determinant

```math
\det\begin{pmatrix}a & b \\ c & d\end{pmatrix} = ad - bc
```

$`A`$ is invertible iff $`ad - bc \neq 0`$.

### Cofactor Expansion (Laplace Expansion)

**Minor:** $`M_{ij} = \det(A_{ij})`$ where $`A_{ij}`$ is the $`(n-1)\times(n-1)`$ submatrix with row $`i`$ and column $`j`$ deleted.

**Cofactor:** $`C_{ij} = (-1)^{i+j} M_{ij}`$.

**Expansion along row $`i`$:**

```math
\det(A) = \sum_{j=1}^n a_{ij} C_{ij} = \sum_{j=1}^n (-1)^{i+j} a_{ij} \det(A_{ij})
```

**Expansion along column $`j`$:**

```math
\det(A) = \sum_{i=1}^n a_{ij} C_{ij}
```

**$`3 \times 3`$ formula (expanding row 1):**

```math
\det\begin{pmatrix}a&b&c\\d&e&f\\g&h&i\end{pmatrix} = a(ei-fh) - b(di-fg) + c(dh-eg)
```

### Permutation Expansion

**Definition:** A **permutation** $`\sigma: \{1,\ldots,n\} \to \{1,\ldots,n\}`$ is a bijection. The **sign** $`\text{sgn}(\sigma) = (-1)^{\text{(number of inversions)}}`$ where an inversion is a pair $`(i,j)`$ with $`i < j`$ but $`\sigma(i) > \sigma(j)`$.

**Theorem (Permutation Formula):**

```math
\det(A) = \sum_{\sigma \in S_n} \text{sgn}(\sigma) \prod_{i=1}^n a_{i,\sigma(i)}
```

where $`S_n`$ is the set of all $`n!`$ permutations of $`\{1,\ldots,n\}`$.

**For $`n=3`$:** 6 permutations — 3 even (sign $`+1`$), 3 odd (sign $`-1`$) — gives the $`3 \times 3`$ formula above.

**Proof that the permutation formula satisfies the three axioms:**

1. *Multilinearity:* Fix row $`i`$; every term $`\text{sgn}(\sigma)\prod_k a_{k,\sigma(k)}`$ contains exactly one factor from row $`i`$, namely $`a_{i,\sigma(i)}`$. Scaling row $`i`$ by $`c`$ scales every term by $`c`$, so $`\det`$ scales by $`c`$. Replacing row $`i`$ by the sum of two rows distributes across the terms. Hence $`\det`$ is linear in each row.

2. *Alternating:* Swapping rows $`p`$ and $`q`$ replaces each permutation $`\sigma`$ with $`\tau\circ\sigma`$ where $`\tau`$ transposes $`p`$ and $`q`$. Since $`\text{sgn}(\tau\circ\sigma) = \text{sgn}(\tau)\cdot\text{sgn}(\sigma) = -\text{sgn}(\sigma)`$ (transpositions are odd), every term changes sign, so $`\det`$ changes sign. $`\square`$

3. *Normalisation:* For $`I`$, $`a_{i,\sigma(i)} = 1`$ only when $`\sigma(i) = i`$ for all $`i`$, i.e., $`\sigma = \text{id}`$. The only nonzero term is $`\text{sgn}(\text{id})\cdot 1 = 1`$.

**Uniqueness of the determinant:** Any alternating multilinear function $`D`$ with $`D(I) = 1`$ equals the permutation formula. *Sketch:* By multilinearity, $`D(A)`$ is determined by the values $`D(e_{\sigma(1)}, \ldots, e_{\sigma(n)})`$ for permutations $`\sigma`$ of the standard basis rows. By the alternating property, $`D(e_{\sigma(1)}, \ldots, e_{\sigma(n)}) = \text{sgn}(\sigma)D(I) = \text{sgn}(\sigma)`$. Expanding $`D(A)`$ via multilinearity recovers exactly the permutation formula. $`\square`$

### Key Properties

**Theorem:** $`\det(A^T) = \det(A)`$.
**Proof:** In the permutation formula, substituting $`\tau = \sigma^{-1}`$ (using $`\text{sgn}(\sigma) = \text{sgn}(\sigma^{-1})`$) gives $`\det(A^T)`$. $`\square`$

**Corollary:** Column operations have the same effect as row operations.

**Theorem:** $`\det(AB) = \det(A) \det(B)`$.

**Corollary:** $`\det(A^{-1}) = 1/\det(A)`$.

**Corollary:** $`\det(kA) = k^n \det(A)`$ for $`n \times n`$ matrices.

**Invertibility criterion:** $`A`$ is invertible $`\iff`$ $`\det(A) \neq 0`$ $`\iff`$ rows of $`A`$ are linearly independent.

### Adjugate and Cramer's Rule

**Adjugate (classical adjoint):** $`\text{adj}(A)_{ij} = C_{ji}`$ (transpose of the cofactor matrix).

**Theorem:** $`A \cdot \text{adj}(A) = \det(A) \cdot I`$.

**Consequence:** If $`\det(A) \neq 0`$, then $`A^{-1} = \frac{1}{\det(A)} \text{adj}(A)`$.

**Cramer's Rule:** For $`Ax = b`$ with $`A`$ invertible:

```math
x_i = \frac{\det(A_i)}{\det(A)}
```

where $`A_i`$ is $`A`$ with column $`i`$ replaced by $`b`$.

### Geometric Interpretation

$`|\det(A)|`$ equals the **volume** of the parallelepiped spanned by the rows of $`A`$.

For a linear transformation with matrix $`A`$ and any region $`S`$:

```math
\text{volume}(A(S)) = |\det(A)| \cdot \text{volume}(S)
```

$`\det(A) = 0`$ iff the transformation collapses volume to lower dimension (rows dependent).

---

## 14. Eigenvalues and Eigenvectors

### Definitions

A linear transformation $`t: V \to V`$ has **eigenvalue** $`\lambda`$ if there exists a nonzero $`v \in V`$ with $`t(v) = \lambda v`$. (This definition is basis-independent — the eigenvalue is a property of the transformation, not a particular matrix.)

A **nonzero** vector $`v`$ is an **eigenvector** of $`A`$ with **eigenvalue** $`\lambda`$ if:

```math
Av = \lambda v
```

### Finding Eigenvalues

```math
Av = \lambda v \iff (A - \lambda I)v = \mathbf{0} \iff \det(A - \lambda I) = 0
```

**Definition — Characteristic polynomial:**

```math
p(\lambda) = \det(A - \lambda I)
```

This is a degree-$`n`$ polynomial for an $`n \times n`$ matrix.

**Eigenvalues** are the roots of $`p(\lambda) = 0`$.

**The spectrum** of $`A`$ is the set of all eigenvalues.

### Finding Eigenvectors

For each eigenvalue $`\lambda`$, the **eigenspace** is:

```math
E_\lambda = \text{Null}(A - \lambda I)
```

Eigenvectors are the nonzero vectors in $`E_\lambda`$.

**Lemma:** $`E_\lambda`$ is a subspace of $`V`$ (for each fixed $`\lambda`$).

**Proof:** $`E_\lambda = \ker(A - \lambda I)`$ is the null space of a linear map, hence a subspace. Directly: $`E_\lambda`$ contains $`\mathbf{0}`$ (since $`(A-\lambda I)\mathbf{0} = \mathbf{0}`$); if $`u, v \in E_\lambda`$ then $`(A-\lambda I)(ru + sv) = r(A-\lambda I)u + s(A-\lambda I)v = \mathbf{0}`$, so $`ru + sv \in E_\lambda`$. $`\square`$

### Worked Example

```math
A = \begin{pmatrix}3 & 1 \\ 1 & 3\end{pmatrix}
```

```math
p(\lambda) = \det\begin{pmatrix}3-\lambda & 1 \\ 1 & 3-\lambda\end{pmatrix} = (3-\lambda)^2 - 1 = \lambda^2 - 6\lambda + 8 = (\lambda-4)(\lambda-2)
```

Eigenvalues: $`\lambda_1 = 4`$, $`\lambda_2 = 2`$.

For $`\lambda = 4`$: $`(A - 4I)v = \mathbf{0}`$ gives

```math
\begin{pmatrix}-1 & 1 \\ 1 & -1\end{pmatrix}v = \mathbf{0}
```

so $`E_4 = \text{Span}\{(1,1)\}`$.

For $`\lambda = 2`$: $`(A - 2I)v = \mathbf{0}`$ gives

```math
\begin{pmatrix}1 & 1 \\ 1 & 1\end{pmatrix}v = \mathbf{0}
```

so $`E_2 = \text{Span}\{(1,-1)\}`$.

### Existence of Eigenvalues over ℂ

**Lemma 3.10:** Every linear transformation on a nontrivial vector space **over $`\mathbb{C}`$** has at least one eigenvalue.

**Proof:** The characteristic polynomial $`p(\lambda) = \det(A - \lambda I)`$ has degree $`n \geq 1`$. By the Fundamental Theorem of Algebra, every polynomial of degree $`\geq 1`$ over $`\mathbb{C}`$ has at least one root. That root is an eigenvalue. $`\square`$

**Remark:** Over $`\mathbb{R}`$, this fails — e.g., the $`90^\circ`$ rotation matrix

```math
\begin{pmatrix}0 & -1 \\ 1 & 0\end{pmatrix}
```

has characteristic polynomial $`\lambda^2 + 1`$, with no real roots. This is why Jordan Canonical Form is stated over $`\mathbb{C}`$.

### Key Facts

- Eigenvalues of a **triangular matrix** are its diagonal entries.
- $`\text{tr}(A) = \sum_i a_{ii} = \sum_i \lambda_i`$ (sum of eigenvalues).
- $`\det(A) = \prod_i \lambda_i`$ (product of eigenvalues).
- $`\lambda = 0`$ is an eigenvalue $`\iff`$ $`A`$ is singular.

### Linear Independence of Eigenvectors

**Theorem:** Eigenvectors corresponding to **distinct** eigenvalues are linearly independent.

**Proof (by induction on number of eigenvectors):** Base case: one nonzero vector is independent. Inductive step: suppose $`\{v_1, \ldots, v_k\}`$ are independent eigenvectors for distinct eigenvalues $`\lambda_1 < \cdots < \lambda_k`$. Suppose $`c_1 v_1 + \cdots + c_k v_k = \mathbf{0}`$. Apply $`A`$: $`c_1\lambda_1 v_1 + \cdots + c_k \lambda_k v_k = \mathbf{0}`$. Subtract $`\lambda_k`$ times the first: $`\sum_{i=1}^{k-1} c_i(\lambda_i - \lambda_k) v_i = \mathbf{0}`$. By induction, $`c_i(\lambda_i - \lambda_k) = 0`$; since $`\lambda_i \neq \lambda_k`$, all $`c_i = 0`$ for $`i < k`$. Then $`c_k v_k = \mathbf{0}`$ gives $`c_k = 0`$. $`\square`$

### Algebraic and Geometric Multiplicity

**Algebraic multiplicity** of $`\lambda`$: its multiplicity as a root of $`p(\lambda)`$.

**Geometric multiplicity** of $`\lambda`$: $`\dim(E_\lambda) = \dim(\text{Null}(A - \lambda I))`$.

**Theorem:** $`1 \leq \text{geometric multiplicity} \leq \text{algebraic multiplicity}`$.

### Geometry of Eigenvectors

A linear transformation $`t: \mathbb{R}^2 \to \mathbb{R}^2`$ maps lines through the origin to lines through the origin (since $`t(cv) = ct(v)`$). An eigenvector with eigenvalue $`\lambda`$ spans a line that is **mapped to itself**, scaled by $`\lambda`$.

**Visualisation via the upper half unit circle:** Parametrise all directions by $`\theta \in [0°, 180°)`$ (each represents a line through the origin). Plot $`\theta`$ on the horizontal axis and the angle of $`t(v_\theta)`$ on the vertical axis, where $`v_\theta = (\cos\theta, \sin\theta)`$. Eigendirections are **fixed points** of this map (where the curve crosses the diagonal $`y = x`$). A crossing with positive slope corresponds to a positive eigenvalue; the direction is preserved. A crossing with negative slope corresponds to a negative eigenvalue; the direction is reversed.

This picture makes clear:
- A rotation has **no fixed points** on the circle (no real eigenvectors).
- A reflection has **two fixed points**: along the mirror axis ($`\lambda = 1`$) and perpendicular to it ($`\lambda = -1`$).
- Any symmetric matrix has two perpendicular eigendirections (spectral theorem).

**Similar matrices share eigenvalues:** If $`B = P^{-1}AP`$, then $`\det(B - \lambda I) = \det(P^{-1}(A-\lambda I)P) = \det(A - \lambda I)`$. So the characteristic polynomial, and hence all eigenvalues, are the same.

---

## 15. Diagonalisability

### Definition

$`A`$ is **diagonalisable** if it is similar to a diagonal matrix: $`A = PDP^{-1}`$ where $`D = \text{diag}(\lambda_1, \ldots, \lambda_n)`$.

### Criterion

**Theorem:** $`A`$ is diagonalisable $`\iff`$ it has $`n`$ linearly independent eigenvectors.

**Proof:** Let $`v_1, \ldots, v_n`$ be $`n`$ independent eigenvectors, eigenvalues $`\lambda_1, \ldots, \lambda_n`$. Set $`P = [v_1 \mid \cdots \mid v_n]`$. Then:

```math
AP = [Av_1 \mid \cdots \mid Av_n] = [\lambda_1 v_1 \mid \cdots \mid \lambda_n v_n] = PD
```

Since $`P`$ has independent columns, $`P`$ is invertible, giving $`A = PDP^{-1}`$. $`\square`$

**Corollary:** $`A`$ with $`n`$ **distinct** eigenvalues is diagonalisable (eigenvectors for distinct eigenvalues are independent).

### Full Criterion (Using Multiplicities)

**Theorem:** $`A`$ is diagonalisable $`\iff`$ for every eigenvalue $`\lambda`$:

```math
\text{geometric multiplicity of }\lambda = \text{algebraic multiplicity of }\lambda
```

**Failure example:** The matrix

```math
A = \begin{pmatrix}1 & 1 \\ 0 & 1\end{pmatrix}
```

has $`\lambda = 1`$ with algebraic multiplicity $`2`$ but $`E_1 = \text{Span}\{(1,0)\}`$ — geometric multiplicity $`1`$. Not diagonalisable.

### Powers of Diagonal Matrices

If $`A = PDP^{-1}`$, then $`A^k = PD^kP^{-1}`$, and $`D^k = \text{diag}(\lambda_1^k, \ldots, \lambda_n^k)`$ — easy to compute.

---

## 16. Jordan Canonical Form

### Nilpotent Maps

**Definition:** A linear map $`t: V \to V`$ is **nilpotent** if $`t^k = 0`$ for some $`k \geq 1`$.

**Properties:** All eigenvalues of a nilpotent map are $`0`$.

A nilpotent $`n \times n`$ matrix satisfies $`t^n = 0`$.

### Jordan Blocks

A **$`p \times p`$ Jordan block** for eigenvalue $`\lambda`$:

```math
J_p(\lambda) = \begin{pmatrix}
  \lambda & 1       & 0      & \cdots & 0      \\
  0       & \lambda & 1      & \cdots & 0      \\
  \vdots  & \vdots  & \ddots & \ddots & \vdots \\
  0       & 0       & \cdots & \lambda & 1      \\
  0       & 0       & \cdots & 0      & \lambda
\end{pmatrix}
```

$`\lambda`$'s on the diagonal, $`1`$'s on the superdiagonal, $`0`$'s elsewhere.

A $`1 \times 1`$ Jordan block $`J_1(\lambda) = (\lambda)`$ — an ordinary diagonal entry.

### Jordan Canonical Form Theorem

**Theorem:** Every $`n \times n`$ matrix $`A`$ over $`\mathbb{C}`$ is similar to a **block diagonal Jordan matrix**:

```math
J = \begin{pmatrix}
  J_{p_1}(\lambda_1) & & 0 \\
  & \ddots & \\
  0 & & J_{p_k}(\lambda_k)
\end{pmatrix}
```

where the Jordan blocks are unique up to reordering.

- The $`\lambda_i`$ are the eigenvalues (with repetition).
- Number of Jordan blocks for eigenvalue $`\lambda`$ = geometric multiplicity of $`\lambda`$ = $`\dim(\text{Null}(A - \lambda I))`$.
- The sizes of the blocks for $`\lambda`$ satisfy $`\sum p_i = \text{algebraic multiplicity of }\lambda`$.
- $`A`$ is diagonalisable $`\iff`$ all Jordan blocks are $`1 \times 1`$.

### Generalised Eigenvectors

$`v`$ is a **generalised eigenvector** of order $`k`$ if $`(A - \lambda I)^k v = \mathbf{0}`$ but $`(A - \lambda I)^{k-1} v \neq \mathbf{0}`$.

**Generalised eigenspace:** $`V_\lambda = \text{Null}((A - \lambda I)^n)`$ for an $`n \times n`$ matrix.

**Theorem:** $`V = V_{\lambda_1} \oplus V_{\lambda_2} \oplus \cdots \oplus V_{\lambda_r}`$ (direct sum over distinct eigenvalues).

### Computing the Jordan Form

1. Find eigenvalues (roots of $`\det(A - \lambda I) = 0`$).
2. For each eigenvalue $`\lambda`$, compute $`\dim(\text{Null}((A - \lambda I)^k))`$ for $`k = 1, 2, \ldots`$
3. Number of Jordan blocks of size $`\geq k`$ = $`\dim(\text{Null}((A-\lambda I)^k)) - \dim(\text{Null}((A-\lambda I)^{k-1}))`$.
4. This sequence of differences determines all block sizes.

### Cayley–Hamilton Theorem

**Theorem:** If $`p(\lambda) = \det(A - \lambda I)`$ is the characteristic polynomial, then $`p(A) = O`$ (the zero matrix).

**Example:** For

```math
A = \begin{pmatrix}3 & 1 \\ 1 & 3\end{pmatrix}
```

we have $`p(\lambda) = \lambda^2 - 6\lambda + 8`$, so $`A^2 - 6A + 8I = O`$.

**Proof via Jordan form:** Since every matrix is similar to its Jordan form over $`\mathbb{C}`$, write $`A = PJP^{-1}`$. Then $`p(A) = Pp(J)P^{-1}`$, so it suffices to show $`p(J) = O`$.

Since $`J`$ is block-diagonal with Jordan blocks $`J_{p_k}(\lambda_k)`$, and $`p(\lambda) = \prod_k (\lambda - \lambda_k)^{a_k}`$ (where $`a_k`$ is the algebraic multiplicity of $`\lambda_k`$):

For a single Jordan block $`J_p(\lambda_k)`$ of size $`p \leq a_k`$:

```math
J_p(\lambda_k) = \lambda_k I + N, \qquad
N = \begin{pmatrix}
  0      & 1      & 0      & \cdots & 0 \\
  0      & 0      & 1      & \cdots & 0 \\
  \vdots & \vdots & \ddots & \ddots & \vdots \\
  0      & 0      & \cdots & 0      & 1 \\
  0      & 0      & \cdots & 0      & 0
\end{pmatrix}
```

is nilpotent with $`N^p = 0`$. Then:

```math
(J_p(\lambda_k) - \lambda_k I)^{a_k} = N^{a_k} = 0
```

since $`p \leq a_k`$. Therefore $`(J_p(\lambda_k) - \lambda_k I)^{a_k} = 0`$, and since $`p(\lambda) = \prod_k (\lambda - \lambda_k)^{a_k}`$, the factor $`(\lambda - \lambda_k)^{a_k}`$ in $`p`$ annihilates the $`k`$-th block. All blocks are annihilated, so $`p(J) = O`$. $`\square`$

### Minimal Polynomial

**Definition:** The **minimal polynomial** $`m(\lambda)`$ is the monic polynomial of least degree such that $`m(A) = O`$.

- $`m(\lambda)`$ divides $`p(\lambda)`$ (the characteristic polynomial).
- Every eigenvalue of $`A`$ is a root of $`m(\lambda)`$.
- $`A`$ is diagonalisable $`\iff`$ $`m(\lambda)`$ has no repeated roots.

**Worked example:** Let

```math
A = \begin{pmatrix}2 & 1 & 0 \\ 0 & 2 & 0 \\ 0 & 0 & 3\end{pmatrix}
```

(Jordan form with blocks $`J_2(2)`$ and $`J_1(3)`$).

- Characteristic polynomial: $`p(\lambda) = (\lambda-2)^2(\lambda-3)`$.
- Try $`m(\lambda) = (\lambda-2)(\lambda-3)`$:

```math
(A-2I)(A-3I) = \begin{pmatrix}0&1&0\\0&0&0\\0&0&1\end{pmatrix}\begin{pmatrix}-1&1&0\\0&-1&0\\0&0&0\end{pmatrix} = \begin{pmatrix}0&-1&0\\0&0&0\\0&0&0\end{pmatrix} \neq O
```

- Try $`m(\lambda) = (\lambda-2)^2(\lambda-3) = p(\lambda)`$: by Cayley-Hamilton, $`p(A) = O`$. ✓

So $`m(\lambda) = (\lambda-2)^2(\lambda-3)`$ — the largest Jordan block for eigenvalue $`2`$ has size $`2`$, matching the exponent.

**Rule:** The minimal polynomial is $`m(\lambda) = \prod_k (\lambda - \lambda_k)^{s_k}`$ where $`s_k`$ is the **size of the largest Jordan block** for eigenvalue $`\lambda_k`$.

---

## 17. LU and PLU Factorisation

### LU Factorisation

**Setting:** $`A`$ is $`n \times n`$. Gaussian elimination without row swaps can be written as:

```math
L_{n-1} \cdots L_2 L_1 A = U
```

where each $`L_k`$ is a unit lower-triangular matrix that zeroes out column $`k`$ below the diagonal.

**LU Factorisation:** Setting $`L = L_1^{-1} L_2^{-1} \cdots L_{n-1}^{-1}`$ gives $`A = LU`$ where:
- $`L`$ is **unit lower-triangular** (1's on diagonal, entries below; $`l_{jk}`$ are the multipliers).
- $`U`$ is **upper-triangular**.

**Multipliers:** $`l_{jk} = u_{jk}/u_{kk}`$ (value divided by pivot).

**First stroke of luck:** $`L_k^{-1}`$ is obtained by **negating** the subdiagonal entries of $`L_k`$. Since $`L_k = I - \tilde{l}_k e_k^T`$:

```math
(I - \tilde{l}_k e_k^T)(I + \tilde{l}_k e_k^T) = I - \tilde{l}_k(e_k^T \tilde{l}_k)e_k^T = I
```

(the cross-term vanishes since $`e_k^T \tilde{l}_k = 0`$ by sparsity).

**Second stroke of luck:** $`L = L_1^{-1}\cdots L_{n-1}^{-1}`$ simply places each set of multipliers at their natural positions:

```math
L = \begin{pmatrix}
  1      & 0      & \cdots & 0 \\
  l_{21} & 1      & \cdots & 0 \\
  l_{31} & l_{32} & \cdots & 0 \\
  \vdots & \vdots & \ddots & \vdots \\
  l_{n1} & l_{n2} & \cdots & 1
\end{pmatrix}
```

**Proof:** The cross-terms in $`L_k^{-1}L_{k+1}^{-1} = (I+\tilde{l}_{k} e_k^T)(I+\tilde{l}_{k+1}e_{k+1}^T)`$ vanish since $`e_k^T \tilde{l}_{k+1} = 0`$. So the multipliers simply stack up. $`\square`$

**Algorithm:**
```
U := A;  L := I
for k = 1 to n-1:
    for j = k+1 to n:
        l_{jk} := u_{jk} / u_{kk}
        u_{j, k:n} := u_{j, k:n} - l_{jk} * u_{k, k:n}
```
**Cost:** $`\approx \frac{2}{3}n^3`$ floating-point operations.

### Solving $`Ax = b`$ via LU

Given $`A = LU`$, solve in two steps:
1. **Forward substitution:** Solve $`Ly = b`$ top-to-bottom. Cost: $`O(n^2)`$.
2. **Back substitution:** Solve $`Ux = y`$ bottom-to-top. Cost: $`O(n^2)`$.

**Advantage:** The LU factorisation can be **reused** for multiple right-hand sides $`b`$.  
**Total cost:** $`\frac{2}{3}n^3`$ (once) + $`O(n^2)`$ (per right-hand side).

**Principle: never compute $`A^{-1}`$** — factorize instead.

### PLU Factorisation (Partial Pivoting)

**Problem:** LU without row swaps is numerically unstable.

**Instability example:** Without pivoting, for

```math
A = \begin{pmatrix}10^{-20} & 1 \\ 1 & 1\end{pmatrix}
```

$`L`$ has entry $`10^{20}`$; in floating-point arithmetic, $`1 - 10^{20}`$ rounds to $`-10^{20}`$, and the computed "solution" $`x = (0,1)`$ differs entirely from the correct $`x = (-1,1)`$.

**Partial Pivoting:** Before eliminating column $`k`$, swap row $`k`$ with the row that has the **largest absolute value** in column $`k`$. This ensures all multipliers $`|l_{jk}| \leq 1`$.

**PLU Factorisation:** With partial pivoting:

```math
PA = LU
```

where $`P`$ is a **permutation matrix**, $`L`$ is unit lower-triangular with $`|l_{jk}| \leq 1`$, and $`U`$ is upper-triangular.

**Third stroke of luck (separating Ps and Ls):** With partial pivoting, elimination produces $`L_{n-1}P_{n-1}\cdots L_1P_1 A = U`$. Naively this mixes permutations and lower-triangular matrices. The key fact is:

For a permutation matrix $`P_k`$ (a swap of rows $`k`$ and $`j`$, $`j > k`$) and a lower-triangular $`L_{k+1}`$ that only touches rows below row $`k+1`$, we have:

```math
P_k L_{k+1} = L'_{k+1} P_k
```

where $`L'_{k+1}`$ is $`L_{k+1}`$ with its subdiagonal entries **permuted** by $`P_k`$. This lets us "bubble" all the $`P_k`$'s to the right, collecting $`P = P_{n-1}\cdots P_1`$ on the right and modified $`L'_k`$'s on the left whose product is still unit lower-triangular with $`|l_{jk}| \leq 1`$. The result is $`L^{-1}U = PA`$, i.e., $`PA = LU`$. $`\square`$

**Interpretation:** Permute the rows of $`A`$ according to $`P`$, then LU-factorize $`PA`$ without pivoting.

### Worked PLU Example

```math
A = \begin{pmatrix}2 & 1 & 1 & 0 \\ 4 & 3 & 3 & 1 \\ 8 & 7 & 9 & 5 \\ 6 & 7 & 9 & 8\end{pmatrix}
```

**Step 1 — Pivot column 1:** Largest entry in column 1 is $`8`$ (row 3). Swap rows 1 and 3. Record $`P_1`$ as a swap of rows 1 and 3.

```math
\begin{pmatrix}
  8 & 7 & 9 & 5 \\
  4 & 3 & 3 & 1 \\
  2 & 1 & 1 & 0 \\
  6 & 7 & 9 & 8
\end{pmatrix}
```

Eliminate below pivot: multipliers $`l_{21}=\tfrac{4}{8}=\tfrac{1}{2}`$, $`l_{31}=\tfrac{2}{8}=\tfrac{1}{4}`$, $`l_{41}=\tfrac{6}{8}=\tfrac{3}{4}`$.

```math
\begin{pmatrix}
  8 & 7 & 9 & 5 \\
  0 & -\tfrac{1}{2} & -\tfrac{3}{2} & -\tfrac{3}{2} \\
  0 & -\tfrac{3}{4} & -\tfrac{5}{4} & -\tfrac{5}{4} \\
  0 & \tfrac{7}{4} & \tfrac{9}{4} & \tfrac{17}{4}
\end{pmatrix}
```

**Step 2 — Pivot column 2 (rows 2–4):** Largest $`|\cdot|`$ in column 2 below pivot is $`\tfrac{7}{4}`$ (row 4). Swap rows 2 and 4. Record $`P_2`$.

Continue elimination to get $`U`$. The combined permutation $`P = P_2 P_1`$ and the collected multipliers give $`L`$.

**Final result:** $`PA = LU`$ where $`P`$ encodes all swaps, $`L`$ is unit lower-triangular with $`|l_{jk}| \leq 1`$, and $`U`$ is upper-triangular. Solve $`Ax = b`$ via: compute $`Pb`$, then solve $`Ly = Pb`$ (forward), then $`Ux = y`$ (backward).

---

## 18. QR Factorisation and Least Squares

### Orthogonal Matrices

**Definition:** A square matrix $`Q`$ is **orthogonal** if $`Q^TQ = I`$ (equivalently $`Q^T = Q^{-1}`$).

Columns of $`Q`$ form an **orthonormal basis**: unit length and pairwise orthogonal.

**Key property:** Multiplication by $`Q`$ preserves lengths and angles:

```math
\|Qx\|^2 = (Qx)^T(Qx) = x^TQ^TQx = x^Tx = \|x\|^2
```

```math
(Qx)\cdot(Qy) = x^TQ^TQy = x^Ty
```

Geometrically: multiplication by an orthogonal matrix is a **rotation or reflection**.

### QR Factorisation

**Theorem:** If $`A`$ is $`m \times n`$ ($`m \geq n`$) with linearly independent columns, then $`A = QR`$ where:
- $`Q`$ is $`m \times n`$ with orthonormal columns ($`Q^TQ = I_n`$) — the **reduced/thin QR**.
- $`R`$ is $`n \times n`$ upper-triangular with positive diagonal entries.

**Derivation:** Run Gram-Schmidt on the columns $`a_1, \ldots, a_n`$ of $`A`$. Define $`r_{ij} = q_i^T a_j`$ (for $`i \leq j`$) and $`r_{jj} = \|a_j - \sum_{i<j} r_{ij}q_i\|`$. Then:

```math
a_j = r_{1j}q_1 + r_{2j}q_2 + \cdots + r_{jj}q_j = \begin{bmatrix}q_1 & \cdots & q_j\end{bmatrix}\begin{pmatrix}r_{1j} \\ \vdots \\ r_{jj}\end{pmatrix}
```

In matrix form: $`A = QR`$.

**Modified Gram-Schmidt (numerically stable):**
```
for i = 1 to n:  v_i := a_i
for i = 1 to n:
    r_{ii} := ||v_i||
    q_i := v_i / r_{ii}
    for j = i+1 to n:
        r_{ij} := q_i^T v_j
        v_j := v_j - r_{ij} q_i
```
Removes $`q_i`$-component from all remaining $`v_j`$ immediately. Mathematically equivalent to classical GS but numerically stable.

**Two approaches to QR:**

| Method | How it works | Cost (square $`n \times n`$) |
|--------|-------------|----------------------------|
| Gram-Schmidt (modified) | **Triangular orthogonalisation**: $`A R^{-1} = Q`$ — left-multiply $`A`$ by upper-triangular matrices to orthogonalise columns | $`\approx 2n^3`$ flops |
| Householder reflections | **Orthogonal triangularisation**: $`Q^T A = R`$ — left-multiply $`A`$ by orthogonal (Householder) matrices to create upper-triangular form | $`\approx \tfrac{4}{3}n^3`$ flops |

Householder is cheaper and more numerically stable for large problems; both are mathematically equivalent.

**Cost:** $`\approx 2mn^2`$ flops (Modified Gram-Schmidt, $`m \times n`$ with $`m \geq n`$).

### Solving $`Ax = b`$ via QR

For square nonsingular $`A = QR`$:

```math
QRx = b \implies Rx = Q^Tb
```

Solve by back-substitution. More stable than LU for ill-conditioned systems.

**Principle: don't invert $`A`$ — QR-factorize instead.**

### Least Squares

**Problem:** Given $`A \in \mathbb{R}^{m \times n}`$ with $`m > n`$ (overdetermined), find $`x`$ minimising $`\|b - Ax\|_2`$.

**Normal Equations:** The minimiser satisfies $`A^T(b - Ax) = \mathbf{0}`$, giving:

```math
A^TAx = A^Tb
```

**Lemma:** If $`A`$ has full column rank $`n`$, then $`A^TA`$ is invertible.
**Proof:** Suppose $`A^TAz = \mathbf{0}`$. Then $`\|Az\|^2 = z^TA^TAz = 0 \Rightarrow Az = \mathbf{0}`$; since columns of $`A`$ are independent, $`z = \mathbf{0}`$. $`\square`$

**Unique solution:** $`x = (A^TA)^{-1}A^Tb`$.

**Pseudoinverse:** $`A^+ = (A^TA)^{-1}A^T`$, so $`x = A^+b`$.

### Least Squares via QR

Let $`A = QR`$ (reduced, $`Q^TQ = I`$, $`R`$ nonsingular). Then:

```math
A^TAx = A^Tb \implies R^TQ^TQRx = R^TQ^Tb \implies Rx = Q^Tb
```

**Algorithm:**
1. Compute reduced QR: $`A = QR`$.
2. Compute $`Q^Tb`$.
3. Solve $`Rx = Q^Tb`$ by back-substitution.

This is numerically more stable than solving the normal equations directly ($`A^TA`$ can be ill-conditioned).

**Cholesky factorisation:** When $`A`$ has full column rank, $`A^TA`$ is symmetric positive definite (SPD). An SPD matrix $`B`$ has a unique **Cholesky factorisation** $`B = R^T R`$ where $`R`$ is upper-triangular with positive diagonal. This gives an alternative path for the normal equations: factorise $`A^TA = R^TR`$, then solve $`R^Ty = A^Tb`$ (forward) and $`Rx = y`$ (backward). Cost: $`\tfrac{1}{3}n^3`$ (cheaper than LU for SPD matrices).

### Linear Regression Example

Fit $`y = c + mt`$ to data $`(t_i, y_i)`$. Set up:

```math
A = \begin{pmatrix}1 & t_1 \\ 1 & t_2 \\ \vdots & \vdots \\ 1 & t_m\end{pmatrix}, \quad x = \begin{pmatrix}c \\ m\end{pmatrix}, \quad b = \begin{pmatrix}y_1 \\ \vdots \\ y_m\end{pmatrix}
```

Normal equations give the least-squares line of best fit.

---

## 19. Norms

### Vector Norms

**Definition:** A **norm** $`\|\cdot\|: \mathbb{R}^n \to \mathbb{R}`$ satisfies:
1. $`\|x\| \geq 0`$, and $`\|x\| = 0 \iff x = \mathbf{0}`$
2. $`\|x + y\| \leq \|x\| + \|y\|`$ (triangle inequality)
3. $`\|rx\| = |r|\,\|x\|`$ (homogeneity)

**$`p`$-norms** (for $`p \geq 1`$):

```math
\|x\|_p = \left(\sum_{i=1}^n |x_i|^p\right)^{1/p}
```

| $`p`$ | Norm | Formula | Name |
|-----|------|---------|------|
| 1 | $`\|\cdot\|_1`$ | $`\sum \lvert x_i \rvert`$ | Manhattan / taxicab |
| 2 | $`\|\cdot\|_2`$ | $`\sqrt{\sum x_i^2}`$ | Euclidean |
| $`\infty`$ | $`\|\cdot\|_\infty`$ | $`\max_i \lvert x_i \rvert`$ | Chebyshev / max norm |

### Induced Matrix Norms

**Definition — Induced (operator) norm:** Given a vector norm $`\|\cdot\|`$, the induced matrix norm for $`A \in \mathbb{R}^{m \times n}`$:

```math
\|A\| = \sup_{x \neq \mathbf{0}} \frac{\|Ax\|}{\|x\|} = \sup_{\|x\|=1} \|Ax\|
```

**Interpretation:** Largest amplification factor $`A`$ applies to any unit vector.

**Key inequality:** $`\|Ax\| \leq \|A\|\,\|x\|`$ for all $`x`$.

**Submultiplicativity:** $`\|AB\| \leq \|A\|\,\|B\|`$.

**Proof:** $`\|ABx\| \leq \|A\|\,\|Bx\| \leq \|A\|\,\|B\|\,\|x\|`$; take supremum. $`\square`$

**Specific induced norms:**
- $`\|A\|_{1} = \max_j \sum_i \lvert a_{ij} \rvert`$ — maximum column sum (absolute values).
- $`\|A\|_{\infty} = \max_i \sum_j \lvert a_{ij} \rvert`$ — maximum row sum (absolute values).
- $`\|A\|_{2} = \sigma_1`$ — largest singular value.

**Frobenius norm** (not induced):

```math
\|A\|_F = \sqrt{\sum_i\sum_j a_{ij}^2}
```

**Orthogonal matrices:** $`\|Q\|_2 = 1`$ and $`\|QA\|_2 = \|A\|_2`$ (orthogonal multiplication is isometric in 2-norm).

---

## 20. Singular Value Decomposition (SVD)

### Definition

For $`A \in \mathbb{R}^{m \times n}`$ (assume $`m \geq n`$), the **SVD** is:

```math
A = U\Sigma V^T
```

where:
- $`U \in \mathbb{R}^{m \times m}`$ is **orthogonal** — columns $`u_1, \ldots, u_m`$ are **left singular vectors**.
- $`V \in \mathbb{R}^{n \times n}`$ is **orthogonal** — columns $`v_1, \ldots, v_n`$ are **right singular vectors**.
- $`\Sigma \in \mathbb{R}^{m \times n}`$ is "diagonal": $`\Sigma_{ii} = \sigma_i \geq 0`$, $`\Sigma_{ij} = 0`$ for $`i \neq j`$, with $`\sigma_1 \geq \sigma_2 \geq \cdots \geq \sigma_n \geq 0`$.

The $`\sigma_i`$ are the **singular values**.

### Fundamental Relations

$`AV = U\Sigma`$, giving for each $`i`$:

```math
Av_i = \sigma_i u_i
```

The matrix $`A`$ maps the orthonormal right singular vectors $`v_i`$ to scaled orthonormal left singular vectors $`\sigma_i u_i`$.

### Geometric Interpretation

$`A = U\Sigma V^T`$ decomposes the action of $`A`$ into three steps:
1. $`V^T`$: rotate/reflect the input (orthogonal transformation).
2. $`\Sigma`$: scale along coordinate axes by $`\sigma_1, \ldots, \sigma_n`$ (and pad/reduce dimension).
3. $`U`$: rotate/reflect the output.

$`A`$ maps the unit sphere in $`\mathbb{R}^n`$ to a hyperellipse in $`\mathbb{R}^m`$, with semi-axes $`\sigma_1 u_1, \ldots, \sigma_n u_n`$.

### SVD vs Eigenvalue Decomposition

| | Eigendecomposition | SVD |
|--|---|---|
| Form | $`A = P\Lambda P^{-1}`$ | $`A = U\Sigma V^T`$ |
| Left/right bases | Same (P) | Different (U, V) |
| Bases orthogonal? | Not generally | Always |
| Exists for all $`A`$? | No (may need $`\mathbb{C}`$, may not be diagonalisable) | Yes (always, over $`\mathbb{R}`$) |
| Useful for | $`A^k`$, stability | Analysis of $`A`$ itself |

### Properties via SVD

Let $`r`$ = number of nonzero singular values ($`\sigma_1 \geq \cdots \geq \sigma_r > 0 = \sigma_{r+1} = \cdots`$).

**Theorem:** $`\text{rank}(A) = r`$.
**Proof:** $`\text{rank}(A) = \text{rank}(U\Sigma V^T) = \text{rank}(\Sigma) = r`$ (since $`U`$, $`V`$ invertible). $`\square`$

**Theorem:** $`\{u_1, \ldots, u_r\}`$ is an orthonormal basis for $`\text{Col}(A)`$; $`\{v_{r+1}, \ldots, v_n\}`$ is an orthonormal basis for $`\text{Null}(A)`$.

**Theorem:** $`\|A\|_2 = \sigma_1`$.

**Theorem:** $`|\det(A)| = \prod_{i=1}^n \sigma_i`$ (for square $`A`$).
**Proof:** $`|\det(U\Sigma V^T)| = |\det(U)||\det(\Sigma)||\det(V^T)| = 1 \cdot \prod\sigma_i \cdot 1`$. $`\square`$

**Theorem:** The nonzero singular values of $`A`$ are the **square roots of the nonzero eigenvalues** of $`A^TA`$.
**Proof:** $`A^TA = V\Sigma^TU^TU\Sigma V^T = V\Sigma^T\Sigma V^T`$. So $`A^TA`$ and $`\Sigma^T\Sigma`$ are similar; eigenvalues of $`\Sigma^T\Sigma`$ are $`\sigma_1^2, \ldots, \sigma_n^2`$. $`\square`$

### Low-Rank Approximation

**Rank-$`k`$ truncated SVD:**

```math
A_k = \sum_{i=1}^k \sigma_i u_i v_i^T \quad (\text{rank } k \text{ matrix})
```

**Theorem (Eckart–Young):** For any $`k \leq r`$:

```math
\|A - A_k\|_2 = \min_{\text{rank}(B) \leq k} \|A - B\|_2 = \sigma_{k+1}
```

$`A_k`$ is the **best rank-$`k`$ approximation** to $`A`$ in the 2-norm.

**Proof sketch:**
- Upper bound: $`A - A_k = \sum_{i>k} \sigma_i u_i v_i^T`$, so $`\|A - A_k\|_{2} = \sigma_{k+1}`$.
- Lower bound: If $`\text{rank}(B) \leq k`$, then $`\text{Null}(B)`$ has dimension $`\geq n-k`$, which overlaps with $`\text{Span}\{v_1,\ldots,v_{k+1}\}`$. Pick $`w \neq \mathbf{0}`$ in the overlap; then $`Bw = \mathbf{0}`$ and $`\|Aw\|_{2} \geq \sigma_{k+1}\|w\|_{2}`$, so $`\|A - B\|_{2} \geq \sigma_{k+1}`$. $`\square`$

**Applications:** Data compression, noise reduction, principal component analysis (PCA).

**Example:** An image (pixel matrix $`A`$) can be stored as $`A_k`$ using only $`k`$ singular vectors + values — much smaller storage than the full matrix, with controlled approximation error $`\sigma_{k+1}`$.

---

## 21. Key Theorems Cheatsheet

### Core Theorems

| Theorem | Statement |
|---------|-----------|
| **Gauss's Method** | Row operations produce equivalent linear systems |
| **General = Particular + Homogeneous** | Solution set of $`Ax=b`$ is a coset: $`\{p + h \mid Ah = 0\}`$ |
| **Rank-Nullity** | $`\text{rank}(h) + \text{nullity}(h) = \dim(V)`$ |
| **Invariance of Basis Size** | All bases of a finite-dim. space have the same cardinality |
| **Isomorphism iff Same Dimension** | $`V \cong W \iff \dim V = \dim W`$ (finite-dimensional, over $`\mathbb{R}`$) |
| **Linear Maps Determined by Basis** | $`h`$ is determined by $`h(b_1), \ldots, h(b_n)`$ on any basis |
| **Eigenvectors for Distinct Eigenvalues** | Linearly independent |
| **Diagonalisability Criterion** | $`A`$ diag'able $`\iff`$ geom.mult. = alg.mult. for each eigenvalue |
| **Cayley-Hamilton** | $`p(A) = O`$ (characteristic polynomial evaluated at matrix) |
| **Jordan Canonical Form** | Every $`n\times n`$ matrix over $`\mathbb{C}`$ is similar to a Jordan matrix |
| **LU Factorisation** | $`A = LU`$ (no swaps) or $`PA = LU`$ (partial pivoting) |
| **QR Factorisation** | $`A = QR`$ if $`A`$ has independent columns |
| **SVD Existence** | Every $`A \in \mathbb{R}^{m \times n}`$ has $`A = U\Sigma V^T`$ |
| **Eckart-Young** | Best rank-$`k`$ approx. in 2-norm: $`A_k`$; error = $`\sigma_{k+1}`$ |
| **Normal Equations** | Least-squares solution satisfies $`A^TAx = A^Tb`$ |
| **det product rule** | $`\det(AB) = \det(A)\det(B)`$ |
| **det transpose** | $`\det(A^T) = \det(A)`$ |
| **$`(AB)^T = B^TA^T`$** | Transpose reverses the order |
| **$`(AB)^{-1} = B^{-1}A^{-1}`$** | Inverse reverses the order |

### Equivalences for an $`n \times n`$ Matrix $`A`$

The following are all equivalent:
- $`A`$ is invertible
- $`\det(A) \neq 0`$
- $`\text{rank}(A) = n`$
- $`\text{Null}(A) = \{\mathbf{0}\}`$
- Columns (rows) of $`A`$ are linearly independent
- $`Ax = b`$ has a unique solution for every $`b`$
- $`0`$ is not an eigenvalue of $`A`$
- All singular values are nonzero

### Proof Techniques

| Goal | Technique |
|------|-----------|
| Show a set is a subspace | Check: nonempty, closed under $`+`$ and scalar mult. |
| Show a set is independent | Assume $`\sum c_i v_i = 0`$; show all $`c_i = 0`$ |
| Show a set is a basis | Show independent + spans; or $`n`$ vectors in $`n`$-dim space |
| Extend set to basis | Add standard basis vectors one by one |
| Find eigenvalues | Solve $`\det(A - \lambda I) = 0`$ |
| Find eigenvectors | Row reduce $`(A - \lambda I)`$ |
| Show $`f`$ is linear | Show $`f(u+v) = f(u)+f(v)`$ and $`f(cv) = cf(v)`$ |
| Show $`f`$ is an isomorphism | Linear + bijective (or linear + same finite dim + injective) |
| Prove by rank-nullity | Write $`\dim V = \text{rank} + \text{nullity}`$; compare |
| Compute $`A^{-1}`$ | Row reduce $`[A\mid I]`$ to $`[I\mid A^{-1}]`$ |
| Solve $`Ax=b`$ (large) | LU or QR factorisation |
| Least squares | QR factorisation; solve $`Rx = Q^Tb`$ |
| Best low-rank approx | Truncated SVD $`A_k`$ |

### Dimension Counts

| Object | Dimension |
|--------|-----------|
| $`\text{Null}(A)`$ ($`m \times n`$, rank $`r`$) | $`n - r`$ |
| $`\text{Col}(A)`$ | $`r`$ |
| $`\text{Row}(A)`$ | $`r`$ |
| $`E_\lambda`$ (eigenspace) | geometric multiplicity of $`\lambda`$ |
| Generalised eigenspace $`V_\lambda`$ | algebraic multiplicity of $`\lambda`$ |
| Solution set of $`Ax=b`$ | $`n - r`$ (dimension of freedom) |

### Key Formulas

```math
\text{proj}_S(v) = \sum_{i=1}^k \frac{v \cdot b_i}{b_i \cdot b_i} b_i
```

*where* $`\{b_i\}`$ *is an orthogonal basis for* $`S`$

```math
P_S = A(A^TA)^{-1}A^T
```

*where* $`A`$ *has columns spanning* $`S`$

```math
x_{\mathrm{LS}} = (A^TA)^{-1}A^Tb = A^+b
```

*least squares solution*

```math
A^{-1} = \frac{1}{\det(A)}\,\mathrm{adj}(A), \qquad x_i = \frac{\det(A_i)}{\det(A)}
```

*Cramer's Rule*

```math
\det(A) = \sum_{\sigma \in S_n} \mathrm{sgn}(\sigma)\prod_{i=1}^n a_{i,\sigma(i)}
```

*permutation formula*

```math
A = U\Sigma V^T, \quad \mathrm{rank}(A) = r, \quad \|A\|_2 = \sigma_1, \quad |\det(A)| = \prod_i \sigma_i
```

*SVD key facts*

---

*Course: Linear Algebra MT, Stefan Kiefer, University of Oxford*  
*Textbook: "Linear Algebra" by Jim Hefferon (4th Edition, open source)*
