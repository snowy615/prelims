# Linear Algebra — Part 3 · Determinants, Eigenvalues & Diagonalisation
*Stefan Kiefer / Jim Hefferon, University of Oxford.*

[**Index**](LA-Concepts.md) · [Part 1](LA-Part1-Vector-Spaces.md) · [Part 2](LA-Part2-Linear-Maps-and-Matrices.md) · **Part 3** · [Part 4](LA-Part4-Factorisations-and-SVD.md)

---

## Contents
12. [Orthogonality, Projection, and Gram-Schmidt](#12-orthogonality-projection-and-gram-schmidt)
13. [Determinants](#13-determinants)
14. [Eigenvalues and Eigenvectors](#14-eigenvalues-and-eigenvectors)
15. [Diagonalisability](#15-diagonalisability)
16. [Jordan Canonical Form](#16-jordan-canonical-form)

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

---

[**Index**](LA-Concepts.md) · [Part 1](LA-Part1-Vector-Spaces.md) · [Part 2](LA-Part2-Linear-Maps-and-Matrices.md) · **Part 3** · [Part 4](LA-Part4-Factorisations-and-SVD.md)

---

*Course: Linear Algebra MT, Stefan Kiefer, University of Oxford*  
*Textbook: "Linear Algebra" by Jim Hefferon (4th Edition, open source)*
