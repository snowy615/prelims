# Eigenvalues, Eigenvectors & Diagonalization | 特征值、特征向量与对角化

**Frequency: ★★★★★** (13, 15, 16, 17, 21, 22, **24**)  
**Corresponding Sheets: 9, 10** | **对应 Sheet: 9, 10**

---

## 1. Definitions | 定义

### EN
- **Eigenvalue–eigenvector pair**: (λ, v) with Av = λv, v ≠ 0.
- **Characteristic polynomial**: p(λ) = det(A − λI).
- **Eigenspace E(λ)**: {v : (A − λI)v = 0} = N(A − λI).
- **Algebraic multiplicity (AM)**: multiplicity of λ as a root of p(λ).
- **Geometric multiplicity (GM)**: dim E(λ).
- **Diagonalizable**: A = SDS⁻¹ with D diagonal ⇔ GM(λ) = AM(λ) for all λ.
- **Cayley–Hamilton**: p(A) = 0.
- **Spectral theorem (real symmetric)**: A = PΛPᵀ with PᵀP = I, Λ diagonal.

### 中文
- **特征值-特征向量对**：(λ, v) 满足 Av = λv，且 v ≠ 0。
- **特征多项式**：p(λ) = det(A − λI)。
- **特征空间 E(λ)**：{v : (A − λI)v = 0} = N(A − λI)。
- **代数重数 (AM)**：λ 作为 p(λ) 根的重数。
- **几何重数 (GM)**：dim E(λ)。
- **可对角化**：A = SDS⁻¹，D 为对角阵 ⇔ 对所有 λ 都有 GM(λ) = AM(λ)。
- **Cayley–Hamilton 定理**：p(A) = 0。
- **谱定理（实对称）**：A = PΛPᵀ，PᵀP = I，Λ 对角。

---

## 2. Exam Template | 考试模板

### EN — "Diagonalise A" / "Find S, D with D = S⁻¹AS"
1. Characteristic polynomial p(λ) = det(A − λI); solve p(λ) = 0.
2. For each λᵢ solve (A − λᵢI)v = 0 for eigenvector.
3. S = [v₁ … vₙ], D = diag(λ₁, …, λₙ).
4. Check S invertible (distinct eigenvalues ⇒ independent eigenvectors).
5. If real symmetric: orthonormalise eigenvectors → P with PᵀP = I, then A = PΛPᵀ.

### 中文 — "对角化 A" / "求 S, D 使得 D = S⁻¹AS"
1. 计算特征多项式 p(λ) = det(A − λI)；令 p(λ) = 0 解出特征值。
2. 对每个 λᵢ 解 (A − λᵢI)v = 0 求特征向量。
3. S = [v₁ … vₙ]，D = diag(λ₁, …, λₙ)。
4. 验证 S 可逆（不同特征值 ⇒ 特征向量线性无关）。
5. 若 A 为实对称矩阵：将特征向量正交归一化 → P 满足 PᵀP = I，则 A = PΛPᵀ。

---

## 3. Standard Proofs to Memorise | 必背标准证明

### EN
- **Distinct eigenvalues ⇒ linearly independent eigenvectors** (the 2016 induction argument):
  - Base case: one eigenvector is trivially independent.
  - Inductive step: assume true for k distinct eigenvalues; for k+1, write c₁v₁ + … + cₖ₊₁vₖ₊₁ = 0, apply A, subtract λₖ₊₁ times the equation, use induction hypothesis.
- **Aᵏ = SDᵏS⁻¹** — used for matrix powers and Fibonacci.
- **GM(λ) ≤ AM(λ)** (2020 Q5):
  - Consider the restriction of A to the generalised eigenspace; use Jordan form or dimension argument.
- **Simultaneous diagonalisation** (2022):
  - If A, B simultaneously diagonalisable then AB = BA.
  - Conversely, if they commute and B has n distinct eigenvalues, every eigenvector of B is an eigenvector of A.
- **Inverse eigenvalues**: If T nonsingular with eigenvalues λᵢ, then T⁻¹ has eigenvalues 1/λᵢ.
- **Row sum eigenvalue**: If each row sums to c, then c is an eigenvalue (with eigenvector (1,1,…,1)ᵀ).
- **Basis change to diagonalise a linear map (2024 Q7g)**: Given T: U → U with matrix representation M w.r.t. basis B, find a new basis C such that the representation w.r.t. C is diagonal. This is equivalent to finding eigenvectors of M: if M = SDS⁻¹, then the columns of S are the coordinates of the new basis vectors in the old basis B.

### 中文
- **不同特征值 ⇒ 特征向量线性无关**（2016 归纳法证明）：
  - 基例：一个特征向量显然线性无关。
  - 归纳步：假设对 k 个不同特征值成立；对 k+1，写 c₁v₁ + … + cₖ₊₁vₖ₊₁ = 0，两边作用 A，再减去 λₖ₊₁ 倍原式，用归纳假设。
- **Aᵏ = SDᵏS⁻¹** — 用于矩阵幂和 Fibonacci 数列。
- **GM(λ) ≤ AM(λ)**（2020 Q5）：
  - 考虑 A 在广义特征空间上的限制；用 Jordan 型或维数论证。
- **同时对角化**（2022）：
  - 若 A, B 可同时对角化，则 AB = BA。
  - 反之，若它们可交换且 B 有 n 个不同特征值，则 B 的每个特征向量也是 A 的特征向量。
- **逆矩阵特征值**：若 T 可逆，特征值为 λᵢ，则 T⁻¹ 的特征值为 1/λᵢ。
- **行和特征值**：若每行和为 c，则 c 是特征值（特征向量为 (1,1,…,1)ᵀ）。
- **换基对角化线性映射 (2024 Q7g)**：给定 T: U → U 关于基 B 的矩阵表示 M，求新基 C 使得关于 C 的表示为对角阵。等价于求 M 的特征向量：若 M = SDS⁻¹，则 S 的列即为新基向量在旧基 B 下的坐标。

---

## 4. Mark-Scheme Habits | 评分关键

- **State the definition of eigenvalue/eigenvector verbatim** for 2-mark bookwork parts. | **特征值/特征向量的定义必须一字不差写出**，这是 2 分送分题。
- **Show that det(A − λI) = 0** before concluding λ is an eigenvalue. | 在得出 λ 是特征值之前，**必须展示 det(A − λI) = 0**。
- **For each eigenvector, write (A − λI)v = 0 explicitly** and solve the system. | 对每个特征向量，**显式写出 (A − λI)v = 0 并求解**。
- **When checking diagonalisability**: explicitly compare GM and AM for each eigenvalue. | **检查可对角化时**：对每个特征值显式比较 GM 和 AM。
- **Cayley–Hamilton**: explicitly compute p(A) and verify it equals 0. | **Cayley–Hamilton**：显式计算 p(A) 并验证等于 0。
- **Real symmetric**: state "A is real symmetric, therefore orthogonally diagonalisable by the Spectral Theorem." | **实对称矩阵**：必须写明 "A 是实对称矩阵，故由谱定理可正交对角化"。

---

## 5. Common Pitfalls | 常见陷阱

1. **Forgetting to check v ≠ 0** | **忘记验证 v ≠ 0**
   - The zero vector is NEVER an eigenvector by definition. | 零向量按定义不是特征向量。
2. **Using the same eigenvector for repeated eigenvalues** | **对重特征值使用相同特征向量**
   - For AM = 2, you must find TWO linearly independent eigenvectors (GM = 2) to diagonalise. | AM = 2 时，必须找到两个线性无关特征向量 (GM = 2) 才能对角化。
3. **Confusing AM and GM** | **混淆代数重数与几何重数**
   - AM ≥ GM always. Diagonalisable iff AM = GM for ALL eigenvalues. | AM ≥ GM 恒成立。可对角化 ⇔ 所有特征值 AM = GM。
4. **Computing S⁻¹ incorrectly** | **S⁻¹ 计算错误**
   - For 2×2: S⁻¹ = (1/det S) · adj(S). For 3×3, use row reduction or adjugate. | 2×2 用 S⁻¹ = (1/det S)·adj(S)；3×3 用行化简或伴随矩阵。
5. **Not normalising eigenvectors for orthogonal diagonalisation** | **正交对角化时未归一化特征向量**
   - P must have orthonormal columns: PᵀP = I. | P 的列必须是标准正交的：PᵀP = I。

---

## 6. Self-Check Questions | 自检问题

- [ ] Can I compute the characteristic polynomial of a 3×3 matrix without errors? | 我能无误地计算 3×3 矩阵的特征多项式吗？
- [ ] Do I remember the formula for the inverse of a 2×2 matrix? | 我记得 2×2 矩阵的逆公式吗？
- [ ] Can I prove that distinct eigenvalues give independent eigenvectors? | 我能证明不同特征值给出线性无关特征向量吗？
- [ ] Do I know the Cayley–Hamilton theorem statement and a quick application? | 我知道 Cayley–Hamilton 定理的表述和一个快速应用吗？
- [ ] Can I tell immediately whether a matrix with eigenvalues 1, 1, 2 is diagonalisable? | 我能立即判断特征值为 1, 1, 2 的矩阵是否可对角化吗？
- [ ] Do I remember the spectral theorem requires real symmetric, not just symmetric? | 我记得谱定理要求实对称，而不只是对称吗？
