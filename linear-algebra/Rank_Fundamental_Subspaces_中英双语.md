# Rank & Four Fundamental Subspaces | 秩与四大基本子空间

**Frequency: ★★★★★** (13, 14, 15, 16, 17, 18, 20, 21, 23, **24**, **25**)  
**Corresponding Sheets: 1, 2, 3, 4, 6, 8** | **对应 Sheet: 1, 2, 3, 4, 6, 8**

---

## 1. Definitions | 定义

### EN
- **Row space C(Aᵀ)**: span of the rows of A. Dimension = rank(A).
- **Column space C(A)**: span of the columns of A; equivalently {Ax : x ∈ ℝⁿ}. Dimension = rank(A).
- **Null space N(A)**: {x : Ax = 0}. Dimension = nullity(A).
- **Left null space N(Aᵀ)**: {y : Aᵀy = 0}.
- **Rank**: dim of row space = dim of column space = number of pivots.
- **Rank–Nullity Theorem**: for A m×n, rank(A) + dim N(A) = n.

### 中文
- **行空间 C(Aᵀ)**：A 的各行张成的空间。维数 = rank(A)。
- **列空间 C(A)**：A 的各列张成的空间；等价于 {Ax : x ∈ ℝⁿ}。维数 = rank(A)。
- **零空间 N(A)**：{x : Ax = 0}。维数 = nullity(A)。
- **左零空间 N(Aᵀ)**：{y : Aᵀy = 0}。
- **秩**：行空间维数 = 列空间维数 = 主元个数。
- **秩-零化度定理**：对 m×n 矩阵 A，rank(A) + dim N(A) = n。

---

## 2. Exam Template | 考试模板

### EN — "Find a basis for the row/column/null space"
1. Row reduce A to echelon form U.
2. Row space basis = non-zero rows of U. rank = number of pivots.
3. Column space basis = columns of the **original** A corresponding to pivot columns of U.
4. Null space: solve Ux = 0; assign free variables parameters t₁, t₂, …; express x as linear combination — those vectors are the basis; dim = n − r.
5. Always state the dimension and that the vectors are linearly independent and span.

### 中文 — "求行空间/列空间/零空间的一组基"
1. 将 A 行化简为阶梯形 U。
2. 行空间基 = U 的非零行。rank = 主元个数。
3. 列空间基 = **原矩阵** A 中对应 U 主元列的那些列。
4. 零空间：解 Ux = 0；令自由变量为参数 t₁, t₂, …；将 x 写成线性组合 — 这些向量就是基；维数 = n − r。
5. 必须写出维数，并说明这些向量线性无关且张成该空间。

---

## 3. Standard Proofs to Memorise | 必背标准证明

### EN
- **Range inclusion chain (2025 Q5b)**: For commuting transformations f₁,...,fₙ₊₁ with f₁∘...∘fₙ₊₁ = 0, show R(f₁∘...∘fₖ₊₁) ⊆ R(f₁∘...∘fₖ) for k = 1,...,n.
  - Proof: write f₁∘...∘fₖ₊₁ = (f₁∘...∘fₖ)∘fₖ₊₁; since fₖ₊₁ maps V → V, the image of the composition is contained in the image of f₁∘...∘fₖ.
- **Range meets null space (2025 Q5d)**: R(f₁∘...∘fₖ) ⊆ N(fₖ₊₁∘...∘fₙ₊₁).
  - Proof: compose both sides with fₖ₊₁∘...∘fₙ₊₁; the total product is 0, so every vector in the range is sent to 0.
- **Strict inclusion implies isomorphism (2025 Q5c)**: If all inclusions are strict, then dim R(f₁∘...∘fₖ₊₁) < dim R(f₁∘...∘fₖ) for each k. Since dimensions are integers, after n strict drops we reach 0, forcing f₁ to be onto (rank = dim V) and the full product to be 0.

### 中文
- **像空间包含链 (2025 Q5b)**：对可交换变换 f₁,...,fₙ₊₁ 且 f₁∘...∘fₙ₊₁ = 0，证明 R(f₁∘...∘fₖ₊₁) ⊆ R(f₁∘...∘fₖ)（k = 1,...,n）。
  - 证明：写 f₁∘...∘fₖ₊₁ = (f₁∘...∘fₖ)∘fₖ₊₁；fₖ₊₁ 将 V → V，故复合的像包含于 f₁∘...∘fₖ 的像。
- **像空间与零空间相交 (2025 Q5d)**：R(f₁∘...∘fₖ) ⊆ N(fₖ₊₁∘...∘fₙ₊₁)。
  - 证明：两边复合 fₖ₊₁∘...∘fₙ₊₁；总乘积为 0，故像中每个向量都被映为 0。
- **严格包含推出同构 (2025 Q5c)**：若所有包含都是严格的，则 dim R(f₁∘...∘fₖ₊₁) < dim R(f₁∘...∘fₖ)。维数为整数，n 次严格下降后达到 0，迫使 f₁ 为满射（rank = dim V）且全乘积为 0。

---

## 4. Mark-Scheme Habits | 评分关键

- **State the theorem by name before using it**: "By the rank–nullity theorem, n = r + dim N(A)." | **使用定理前先点名**："由秩-零化度定理，n = r + dim N(A)。"
- **For every basis**: verify/assert both spanning and independence, and state the dimension. | **每个基**：必须同时验证/声明张成性和线性无关性，并写出维数。
- **Show row-reduction steps** — arithmetic slips lose method marks only if work is visible. | **展示行化简步骤** — 只有过程可见，计算失误才不丢方法分。
- **Do not confuse pivot columns of U with columns of U** for the column space basis. | **列空间基不能直接用 U 的列**，必须用原矩阵 A 的对应列。

---

## 5. Common Pitfalls | 常见陷阱

1. **Using columns of U instead of A for C(A)** | **列空间基误用 U 的列**
   - The column space of U is NOT the same as the column space of A. | U 的列空间 ≠ A 的列空间。
2. **Forgetting to state the dimension** | **忘记声明维数**
   - Oxford mark schemes explicitly award marks for "dim = …". | 牛津评分标准中 "dim = …" 有单独分值。
3. **Mixing up rank and nullity** | **混淆秩与零化度**
   - rank(A) = dim C(A) = dim C(Aᵀ); nullity(A) = dim N(A). | rank(A) 是列/行空间维数；nullity(A) 是零空间维数。
4. **Not parameterising free variables** | **自由变量未参数化**
   - Write x = t₁v₁ + t₂v₂ explicitly; do not leave free variables implicit. | 必须显式写成 x = t₁v₁ + t₂v₂。

---

## 6. Self-Check Questions | 自检问题

- [ ] Can I compute RREF of a 3×3 or 4×4 matrix reliably? | 我能可靠地计算 3×3 或 4×4 矩阵的 RREF 吗？
- [ ] Do I know which columns of the **original** A form the column space basis? | 我知道该取原矩阵 A 的哪几列作为列空间基吗？
- [ ] Can I write the rank–nullity theorem statement verbatim? | 我能一字不差地写出秩-零化度定理吗？
- [ ] Can I prove that row space and null space are orthogonal? | 我能证明行空间与零空间正交吗？
- [ ] Do I remember that dim N(A) = number of free variables? | 我记得 dim N(A) = 自由变量个数吗？
