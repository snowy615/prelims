# Linear Algebra — Part 2 · Linear Maps & Matrices
*Stefan Kiefer / Jim Hefferon, University of Oxford.*

[**Index**](LA-Concepts.md) · [Part 1](LA-Part1-Vector-Spaces.md) · **Part 2** · [Part 3](LA-Part3-Determinants-Eigenvalues.md) · [Part 4](LA-Part4-Factorisations-and-SVD.md)

---

## Contents
7. [Isomorphisms](#7-isomorphisms)
8. [Linear Maps (Homomorphisms)](#8-linear-maps-homomorphisms)
9. [Representing Linear Maps with Matrices](#9-representing-linear-maps-with-matrices)
10. [Matrix Algebra](#10-matrix-algebra)
11. [Change of Basis and Similar Matrices](#11-change-of-basis-and-similar-matrices)

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

---

[**Index**](LA-Concepts.md) · [Part 1](LA-Part1-Vector-Spaces.md) · **Part 2** · [Part 3](LA-Part3-Determinants-Eigenvalues.md) · [Part 4](LA-Part4-Factorisations-and-SVD.md)

---

*Course: Linear Algebra MT, Stefan Kiefer, University of Oxford*  
*Textbook: "Linear Algebra" by Jim Hefferon (4th Edition, open source)*
