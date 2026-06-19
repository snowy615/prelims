# Elementary Matrices & LU / PLU Factorization | 初等矩阵与 LU / PLU 分解

**Frequency: ★★★★★** (14, 15, 17, 19, 21, **24**)  
**Corresponding Sheets: 1, 2** | **对应 Sheet: 1, 2**

---

## 1. Definitions | 定义

### EN
- **Elementary matrix**: the matrix obtained by performing a single elementary row operation on the identity matrix I.
- **Three types**:
  1. **Row scaling**: E = diag(1,…,k,…,1); E⁻¹ = diag(1,…,1/k,…,1).
  2. **Row addition**: E adds k·Rᵢ to Rⱼ; E⁻¹ subtracts k·Rᵢ from Rⱼ.
  3. **Row swap**: E = permutation matrix; E⁻¹ = Eᵀ = E.
- **LU factorisation**: A = LU where L is unit lower triangular and U is upper triangular.
- **PLU factorisation**: PA = LU where P is a permutation matrix (used when a pivot is zero).

### 中文
- **初等矩阵**：对单位矩阵 I 执行一次初等行变换所得的矩阵。
- **三种类型**：
  1. **行倍乘**：E = diag(1,…,k,…,1)；E⁻¹ = diag(1,…,1/k,…,1)。
  2. **行倍加**：E 将 k·Rᵢ 加到 Rⱼ；E⁻¹ 将 k·Rᵢ 从 Rⱼ 减去。
  3. **行交换**：E 为置换矩阵；E⁻¹ = Eᵀ = E。
- **LU 分解**：A = LU，L 为单位下三角矩阵，U 为上三角矩阵。
- **PLU 分解**：PA = LU，P 为置换矩阵（用于主元为零时）。

---

## 2. Exam Template | 考试模板

### EN — "PLU with partial pivoting (2024 Q5a)"
1. Scan the first column for the largest absolute value entry below and including the diagonal; swap that row to the top → record the permutation in P.
2. Use Rⱼ ← Rⱼ − kRᵢ (row addition only) to eliminate below the pivot. Record multipliers in L.
3. Move to the next column and repeat. **No row scaling** in elimination.
4. PA = LU where P is the product of all row-swap permutations, L is unit lower triangular, U is upper triangular.
5. **Verify**: compute PA and check it equals LU.

### 中文 — "带部分主元的 PLU 分解 (2024 Q5a)"
1. 在第 1 列中扫描对角线及以下绝对值最大的元素；将其所在行交换到当前主元行 → 将置换记录到 P。
2. 使用 Rⱼ ← Rⱼ − kRᵢ（仅行倍加）消去主元下方元素。在 L 中记录乘数。
3. 移至下一列重复。**消元过程中禁止行倍乘**。
4. PA = LU，其中 P 为所有行交换置换的乘积，L 为单位下三角，U 为上三角。
5. **验证**：计算 PA 检查是否等于 LU。

---

## 3. Solving Ax = b via LU | 用 LU 分解解 Ax = b

### EN
1. Factor A = LU (or PA = LU).
2. Forward substitution: solve Lc = b for c.
3. Back substitution: solve Ux = c for x.
4. **State explicitly**: no inverse of A is ever computed; this is the whole point of LU.

### 中文
1. 分解 A = LU（或 PA = LU）。
2. 前代：解 Lc = b 求 c。
3. 回代：解 Ux = c 求 x。
4. **显式声明**：整个过程中**不计算 A 的逆**；这正是 LU 分解的意义。

---

## 4. Standard Proofs to Memorise | 必背标准证明

### EN
- **Uniqueness of LU for invertible A** (2015):
  - Suppose L₁U₁ = L₂U₂. Then L₂⁻¹L₁ = U₂U₁⁻¹.
  - The left side is unit lower triangular; the right side is upper triangular.
  - The only matrix that is both unit lower and upper triangular is I.
  - Therefore L₂⁻¹L₁ = I and U₂U₁⁻¹ = I ⇒ L₁ = L₂, U₁ = U₂.
- **A invertible ⇔ A is a product of elementary matrices** (2018):
  - If A is invertible, row reduce to I; each step is an elementary matrix Eₖ…E₁A = I ⇒ A = E₁⁻¹…Eₖ⁻¹.
  - Conversely, product of elementary matrices is invertible.
- **A invertible ⇔ rank(A) = n**.

### 中文
- **可逆矩阵 LU 唯一性**（2015）：
  - 设 L₁U₁ = L₂U₂。则 L₂⁻¹L₁ = U₂U₁⁻¹。
  - 左边是单位下三角；右边是上三角。
  - 既是单位下三角又是上三角的矩阵只能是 I。
  - 故 L₂⁻¹L₁ = I 且 U₂U₁⁻¹ = I ⇒ L₁ = L₂, U₁ = U₂。
- **A 可逆 ⇔ A 是初等矩阵的乘积**（2018）：
  - 若 A 可逆，行化简到 I；每一步对应一个初等矩阵 Eₖ…E₁A = I ⇒ A = E₁⁻¹…Eₖ⁻¹。
  - 反之，初等矩阵的乘积可逆。
- **A 可逆 ⇔ rank(A) = n**。

---

## 5. Mark-Scheme Habits | 评分关键

- **Record multipliers as you go**: write ℓ₂₁ = 2, ℓ₃₁ = −1, etc., next to each row operation. | **边做边记录乘数**：在每次行变换旁写下 ℓ₂₁ = 2, ℓ₃₁ = −1 等。
- **L must be unit lower triangular**: diagonal entries are all 1. | **L 必须是单位下三角**：对角线元素全为 1。
- **No row swaps or scaling in pure LU** — if you need them, switch to PLU. | **纯 LU 中禁止行交换和缩放** — 若需要，改用 PLU。
- **State "forward substitution" and "back substitution" by name** when solving. | 求解时**点名** "前代 (forward substitution)" 和 "回代 (back substitution)"。
- **For uniqueness proof**: explicitly note "unit lower triangular" and "upper triangular". | **唯一性证明**：显式指出 "单位下三角" 和 "上三角"。

---

## 6. Common Pitfalls | 常见陷阱

1. **Using row scaling during LU elimination** | **LU 消元过程中使用了行倍乘**
   - Row scaling changes the determinant and destroys the unit-lower-triangular property of L. | 行倍乘改变行列式，且破坏 L 的单位下三角性质。
2. **Putting multipliers in the wrong positions in L** | **乘数在 L 中位置放错**
   - ℓᵢⱼ goes in row i, column j. It is the multiplier used to eliminate the (i, j) entry. | ℓᵢⱼ 放在第 i 行第 j 列。它是用来消去 (i, j) 位置的乘数。
3. **Forgetting to verify LU = A** | **忘记验证 LU = A**
   - A quick check saves marks if arithmetic errors occurred. | 快速验证可以在计算错误时保住分数。
4. **Confusing L and U** | **混淆 L 和 U**
   - L is lower triangular with 1s on diagonal; U is upper triangular with pivots on diagonal. | L 是下三角、对角线为 1；U 是上三角、对角线为主元。
5. **Not explicitly stating that no inverse is computed** | **未显式声明未计算逆矩阵**
   - Oxford examiners love this sentence for 1 mark. | 牛津考官喜欢这句话，值 1 分。

---

## 7. Self-Check Questions | 自检问题

- [ ] Can I perform LU decomposition on a 3×3 matrix in under 4 minutes? | 我能在 4 分钟内完成 3×3 矩阵的 LU 分解吗？
- [ ] Do I know the three types of elementary matrices and their inverses? | 我知道三种初等矩阵及其逆矩阵吗？
- [ ] Can I write the uniqueness proof of LU for invertible matrices? | 我能写出可逆矩阵 LU 唯一性的证明吗？
- [ ] Do I remember why row swaps require PLU instead of LU? | 我记得为什么行交换需要 PLU 而不是 LU 吗？
- [ ] Can I solve Ax = b using forward and back substitution without computing A⁻¹? | 我能用前代和回代解 Ax = b 而不计算 A⁻¹ 吗？
- [ ] Do I remember that A = E₁⁻¹…Eₖ⁻¹ when Eₖ…E₁A = I? | 我记得 Eₖ…E₁A = I 时 A = E₁⁻¹…Eₖ⁻¹ 吗？
