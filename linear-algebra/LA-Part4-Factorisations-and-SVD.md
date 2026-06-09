# Linear Algebra — Part 4 · Matrix Factorisations & SVD
*Stefan Kiefer / Jim Hefferon, University of Oxford.*

[**Index**](LA-Concepts.md) · [Part 1](LA-Part1-Vector-Spaces.md) · [Part 2](LA-Part2-Linear-Maps-and-Matrices.md) · [Part 3](LA-Part3-Determinants-Eigenvalues.md) · **Part 4**

---

## Contents
17. [LU and PLU Factorisation](#17-lu-and-plu-factorisation)
18. [QR Factorisation and Least Squares](#18-qr-factorisation-and-least-squares)
19. [Norms](#19-norms)
20. [Singular Value Decomposition (SVD)](#20-singular-value-decomposition-svd)
21. [Key Theorems Cheatsheet](#21-key-theorems-cheatsheet)

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

[**Index**](LA-Concepts.md) · [Part 1](LA-Part1-Vector-Spaces.md) · [Part 2](LA-Part2-Linear-Maps-and-Matrices.md) · [Part 3](LA-Part3-Determinants-Eigenvalues.md) · **Part 4**

---

*Course: Linear Algebra MT, Stefan Kiefer, University of Oxford*  
*Textbook: "Linear Algebra" by Jim Hefferon (4th Edition, open source)*
