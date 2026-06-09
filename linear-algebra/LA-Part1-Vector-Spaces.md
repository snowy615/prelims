# Linear Algebra — Part 1 · Foundations: Vector Spaces
*Stefan Kiefer / Jim Hefferon, University of Oxford.*

[**Index**](LA-Concepts.md) · **Part 1** · [Part 2](LA-Part2-Linear-Maps-and-Matrices.md) · [Part 3](LA-Part3-Determinants-Eigenvalues.md) · [Part 4](LA-Part4-Factorisations-and-SVD.md)

---

## Contents
1. [Linear Systems and Gauss's Method](#1-linear-systems-and-gausss-method)
2. [Solution Set Structure: General = Particular + Homogeneous](#2-solution-set-structure-general--particular--homogeneous)
3. [Vector Spaces](#3-vector-spaces)
4. [Linear Independence](#4-linear-independence)
5. [Basis and Dimension](#5-basis-and-dimension)
6. [Rank and the Rank-Nullity Theorem](#6-rank-and-the-rank-nullity-theorem)

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

---

[**Index**](LA-Concepts.md) · **Part 1** · [Part 2](LA-Part2-Linear-Maps-and-Matrices.md) · [Part 3](LA-Part3-Determinants-Eigenvalues.md) · [Part 4](LA-Part4-Factorisations-and-SVD.md)

---

*Course: Linear Algebra MT, Stefan Kiefer, University of Oxford*  
*Textbook: "Linear Algebra" by Jim Hefferon (4th Edition, open source)*
