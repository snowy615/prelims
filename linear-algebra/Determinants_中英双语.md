# Determinants | 行列式

**Frequency: ★★★★★** (13, 14, 15, 16, 21, 22, **24**, **25**)  
**Corresponding Sheets: 9, 10** | **对应 Sheet: 9, 10**

---

## 1. Definitions & Facts | 定义与性质

### EN
- **Determinant**: a scalar function det: Mₙₙ → ℝ that is multilinear, alternating, and normalised (det I = 1).
- **Key properties**:
  - det of triangular = product of diagonal entries.
  - Row operation Rⱼ ← Rⱼ − kRᵢ leaves det unchanged.
  - Row swap flips sign: det(multiplied by swap matrix) = −det.
  - Scaling a row scales det by the same factor.
  - det(AB) = det(A)det(B).
  - det(Aᵀ) = det(A).
  - det(−A) = (−1)ⁿ det(A).
  - A invertible ⇔ det(A) ≠ 0.

### 中文
- **行列式**：一个标量函数 det: Mₙₙ → ℝ，满足多重线性、交错性，且 det I = 1。
- **核心性质**：
  - 三角矩阵的行列式 = 对角线元素之积。
  - 行变换 Rⱼ ← Rⱼ − kRᵢ 不改变行列式。
  - 交换两行变号。
  - 某行乘以 k，行列式也乘以 k。
  - det(AB) = det(A)det(B)。
  - det(Aᵀ) = det(A)。
  - det(−A) = (−1)ⁿ det(A)。
  - A 可逆 ⇔ det(A) ≠ 0。

---

## 2. Exam Template | 考试模板

### EN — "Determinant of a block matrix |A B; C D|"
- **Block-triangular**: |A B; 0 D| = |A|·|D| (factorisation via elementary matrices).
- **Schur complement** (when D invertible): |A B; C D| = |A − BD⁻¹C|·|D|.
- **Sylvester identity**: |Iₘ + UV| = |Iₙ + VU| for U ∈ ℝᵐˣⁿ, V ∈ ℝⁿˣᵐ (form block matrix).
- **2024 Q5(c)**: O(n³) det algorithm via PLU: det(A) = det(P)⁻¹·det(U) = (−1)^{#swaps}·∏ uᵢᵢ.

### 中文 — "分块矩阵的行列式 |A B; C D|"
- **分块三角矩阵**：|A B; 0 D| = |A|·|D|（通过初等矩阵分解证明）。
- **Schur 补**（D 可逆时）：|A B; C D| = |A − BD⁻¹C|·|D|。
- **Sylvester 恒等式**：|Iₘ + UV| = |Iₙ + VU|（U ∈ ℝᵐˣⁿ, V ∈ ℝⁿˣᵐ）（构造分块矩阵证明）。
- **2024 Q5(c)**：通过 PLU 求行列式的 O(n³) 算法：det(A) = det(P)⁻¹·det(U) = (−1)^{交换次数}·∏ uᵢᵢ。

---

## 3. Standard Proofs to Memorise | 必背标准证明

### EN
- **Odd-dimension skew-symmetric matrix is singular**:
  - det(A) = det(Aᵀ) = det(−A) = (−1)ⁿ det(A) = −det(A) ⇒ det(A) = 0.
- **det(Aᵀ) = det(A)**: every term in the permutation expansion matches its transpose.
- **det(AB) = det(A)det(B)**: use elementary matrices or product formula.

### 中文
- **奇数维反对称矩阵必奇异**：
  - det(A) = det(Aᵀ) = det(−A) = (−1)ⁿ det(A) = −det(A) ⇒ det(A) = 0。
- **det(Aᵀ) = det(A)**：排列展开中每一项与其转置对应相等。
- **det(AB) = det(A)det(B)**：用初等矩阵或乘积公式证明。

---

## 4. Geometry | 几何应用

### EN
- Area of parallelogram = |det| of matrix of edge vectors.
- Triangle area = ½|det| of 2×2 matrix of two edge vectors.
- Tetrahedron volume = 1/6|det| of 3×3 matrix of three edge vectors.
- Line through two points: can be written as a determinant = 0.

### 中文
- 平行四边形面积 = 边向量矩阵的 |det|。
- 三角形面积 = ½|det|（两个边向量构成的 2×2 矩阵）。
- 四面体体积 = 1/6|det|（三个边向量构成的 3×3 矩阵）。
- 过两点的直线方程：可写成行列式 = 0 的形式。

---

## 5. Mark-Scheme Habits | 评分关键

- **Track every row operation**: if you swap rows, write "det → −det"; if you scale a row by k, write "det → k·det". | **追踪每一步行变换**：交换行要写 "det → −det"；某行乘 k 要写 "det → k·det"。
- **State the final product explicitly** before setting to zero. | 在令其为零之前，**显式写出最终乘积**。
- **For geometry**: explicitly state the formula (e.g., "Area = ½|det([…])|") before substituting numbers. | **几何题**：代入数字前先写出公式（如 "Area = ½|det([…])|"）。
- **Skew-symmetric proof**: write out the chain det(A) = det(Aᵀ) = det(−A) = (−1)ⁿdet(A) step by step. | **反对称证明**：一步一步写出 det(A) = det(Aᵀ) = det(−A) = (−1)ⁿdet(A)。

---

## 6. Common Pitfalls | 常见陷阱

1. **Forgetting the sign flip on row swaps** | **交换行忘记变号**
   - Each swap multiplies det by −1; two swaps bring it back. | 每次交换乘 −1；两次交换复原。
2. **Scaling a row and forgetting to divide det by the scale factor later** | **某行缩放后忘记处理行列式**
   - If you factor out k from a row, you must factor k out of det. | 从某行提出因子 k，必须从行列式中也提出 k。
3. **Using row-addition that changes det untrackably** | **使用了不可追踪的行列式变化行变换**
   - Only Rⱼ ← Rⱼ − kRᵢ is safe without tracking. | 只有 Rⱼ ← Rⱼ − kRᵢ 是安全且无需追踪的。
4. **Confusing det(A+B) with det(A)+det(B)** | **混淆 det(A+B) 与 det(A)+det(B)**
   - det is NOT linear over matrix addition! | 行列式对矩阵加法不是线性的！

---

## 7. Self-Check Questions | 自检问题

- [ ] Can I compute a 3×3 or 4×4 determinant by hand in under 3 minutes? | 我能在 3 分钟内手算 3×3 或 4×4 行列式吗？
- [ ] Do I remember the exact sign rule for row swaps? | 我记得交换行的精确符号规则吗？
- [ ] Can I write the skew-symmetric singularity proof without looking? | 我能不看书写出反对称矩阵奇异性的证明吗？
- [ ] Do I know det(Aᵀ) = det(A) and det(−A) = (−1)ⁿdet(A)? | 我知道 det(Aᵀ) = det(A) 和 det(−A) = (−1)ⁿdet(A) 吗？
- [ ] Can I convert a triangle area problem into a determinant formula? | 我能把三角形面积问题转化为行列式公式吗？
