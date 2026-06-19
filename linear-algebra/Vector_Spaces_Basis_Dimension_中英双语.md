# Vector Spaces, Basis, Independence & Dimension | 向量空间、基、线性无关与维数

**Frequency: ★★★★** (18, 19, 22, 23, **24**)  
**Corresponding Sheets: 3, 4, 6** | **对应 Sheet: 3, 4, 6**

---

## 1. Definitions | 定义

### EN
- **Vector space**: a set V with addition and scalar multiplication satisfying 10 axioms (closure, associativity, commutativity, identity, inverse, distributivity, compatibility, 1·v = v).
- **Subspace**: a subset U ⊆ V that is non-empty (contains 0), closed under addition, and closed under scalar multiplication.
- **Linear independence**: {v₁,…,vₖ} is independent iff c₁v₁ + … + cₖvₖ = 0 ⇒ all cᵢ = 0.
- **Span**: span{v₁,…,vₖ} = {all linear combinations of v₁,…,vₖ}.
- **Basis**: a linearly independent spanning set. Every vector in V has a **unique** representation in a basis.
- **Dimension**: the number of vectors in any basis for V.
- **Direct sum**: V = U ⊕ W ⇔ V = U + W and U ∩ W = {0}. Equivalently, every v ∈ V has a unique decomposition v = u + w.

### 中文
- **向量空间**：带有加法和数乘的集合 V，满足 10 条公理（封闭性、结合律、交换律、零元、负元、分配律、相容性、1·v = v）。
- **子空间**：子集 U ⊆ V，非空（含 0），对加法封闭，对数乘封闭。
- **线性无关**：{v₁,…,vₖ} 线性无关 ⇔ c₁v₁ + … + cₖvₖ = 0 ⇒ 所有 cᵢ = 0。
- **张成（生成）**：span{v₁,…,vₖ} = {v₁,…,vₖ 的所有线性组合}。
- **基**：线性无关的张成集。V 中每个向量在基下有**唯一**表示。
- **维数**：V 的任意基中向量的个数。
- **直和**：V = U ⊕ W ⇔ V = U + W 且 U ∩ W = {0}。等价地，每个 v ∈ V 可唯一分解为 v = u + w。

---

## 2. Exam Template | 考试模板

### EN — "Prove U is a subspace of V"
1. Show 0 ∈ U (non-empty).
2. Let u₁, u₂ ∈ U be arbitrary. Show u₁ + u₂ ∈ U (closure under addition).
3. Let u ∈ U, c be a scalar. Show cu ∈ U (closure under scalar multiplication).
4. Conclude: by the subspace test, U is a subspace of V.

### 中文 — "证明 U 是 V 的子空间"
1. 证明 0 ∈ U（非空）。
2. 任取 u₁, u₂ ∈ U，证明 u₁ + u₂ ∈ U（对加法封闭）。
3. 任取 u ∈ U，c 为标量，证明 cu ∈ U（对数乘封闭）。
4. 结论：由子空间判别法，U 是 V 的子空间。

---

### EN — "Prove {v₁,…,vₖ} is a basis for V"
1. **Linear independence**: write c₁v₁ + … + cₖvₖ = 0; solve the system; show cᵢ = 0 for all i.
2. **Spanning**: let v ∈ V be arbitrary. Show v = a₁v₁ + … + aₖvₖ for some scalars aᵢ (solve for aᵢ).
3. **Alternatively**: if dim V = k, then independence alone implies spanning (and vice versa).
4. State dimension: dim V = k.

### 中文 — "证明 {v₁,…,vₖ} 是 V 的基"
1. **线性无关**：写 c₁v₁ + … + cₖvₖ = 0；解方程组；证明所有 cᵢ = 0。
2. **张成性**：任取 v ∈ V，证明 v = a₁v₁ + … + aₖvₖ 对某个标量 aᵢ 成立（解出 aᵢ）。
3. **替代方法**：若 dim V = k，则线性无关 alone 可推出张成性（反之亦然）。
4. 声明维数：dim V = k。

---

### EN — "Prove V = U ⊕ W"
1. Show V = U + W: every v ∈ V can be written as v = u + w with u ∈ U, w ∈ W.
2. Show U ∩ W = {0}: let x ∈ U ∩ W; use properties of U and W to show x = 0.
3. **Or**: show uniqueness of decomposition: if v = u₁ + w₁ = u₂ + w₂, then u₁ = u₂ and w₁ = w₂.

### 中文 — "证明 V = U ⊕ W"
1. 证明 V = U + W：每个 v ∈ V 可写成 v = u + w，其中 u ∈ U, w ∈ W。
2. 证明 U ∩ W = {0}：设 x ∈ U ∩ W；利用 U 和 W 的性质证明 x = 0。
3. **或者**：证明分解的唯一性：若 v = u₁ + w₁ = u₂ + w₂，则 u₁ = u₂ 且 w₁ = w₂。

---

## 3. Standard Proofs to Memorise | 必背标准证明

### EN
- **Subspace test**: three conditions (non-empty, closed under +, closed under scalar ×).
- **Intersection of subspaces is a subspace**: if U, W are subspaces, U ∩ W is a subspace.
- **Union of subspaces is a subspace iff one contains the other**.
- **Dimension formula (Grassmann)**: dim(U + W) = dim U + dim W − dim(U ∩ W) (2022 — proved via the product space U × W and the linear map g(u₁,u₂) = u₁ + u₂).
- **Direct sum dimension**: dim(U ⊕ W) = dim U + dim W.
- **Tridiagonal matrices form a subspace of dimension 3n − 2** (2022).
- **Nontrivial vector spaces**: if V ≠ {0}, then r·v = 0 ⇒ r = 0; and r₁v = r₂v ⇒ r₁ = r₂.
- **Dimension comparison**: U ⊆ W ⇒ dim U ≤ dim W; equality (finite-dim) ⇒ U = W.
- **Subspace of infinite sequences (2024 Q7c)**: U = {(v₁,v₂,...) | vₖ₊₂ − 5vₖ₊₁ + 4vₖ = 0} is a subspace of V. Check 0 ∈ U, closure under addition and scalar multiplication using the recurrence relation.
- **Krylov subspace chain (2024 Q8)**: V(k) = [c, Mc, ..., Mᵏc]. Show dim V(k) ≤ min{k+1, n}; V(k*) = V(k*+1) = ...; k* < n. Key: dim V(k+1) ≤ dim V(k) + 1, and once V(k) = V(k+1), applying M preserves the space.

### 中文
- **子空间判别法**：三个条件（非空、对加法封闭、对数乘封闭）。
- **子空间的交仍是子空间**：若 U, W 是子空间，则 U ∩ W 是子空间。
- **子空间的并是子空间 ⇔ 其中一个包含另一个**。
- **维数公式 (Grassmann)**：dim(U + W) = dim U + dim W − dim(U ∩ W)（2022 — 通过积空间 U × W 和线性映射 g(u₁,u₂) = u₁ + u₂ 证明）。
- **直和维数**：dim(U ⊕ W) = dim U + dim W。
- **三对角矩阵构成维数为 3n − 2 的子空间**（2022）。
- **非平凡向量空间**：若 V ≠ {0}，则 r·v = 0 ⇒ r = 0；且 r₁v = r₂v ⇒ r₁ = r₂。
- **维数比较**：U ⊆ W ⇒ dim U ≤ dim W；等号成立（有限维）⇒ U = W。
- **无穷序列子空间 (2024 Q7c)**：U = {(v₁,v₂,...) | vₖ₊₂ − 5vₖ₊₁ + 4vₖ = 0} 是 V 的子空间。验证 0 ∈ U，利用递推关系验证加法和数乘封闭性。
- **Krylov 子空间链 (2024 Q8)**：V(k) = [c, Mc, ..., Mᵏc]。证明 dim V(k) ≤ min{k+1, n}；V(k*) = V(k*+1) = ...；k* < n。关键：dim V(k+1) ≤ dim V(k) + 1，且一旦 V(k) = V(k+1)，作用 M 保持空间不变。

---

## 4. Mark-Scheme Habits | 评分关键

- **Subspace test**: explicitly check all three conditions. Missing any one loses marks. | **子空间判别法**：显式检查三个条件。缺少任何一环都会被扣分。
- **For basis**: verify/assert both spanning and independence, and state the dimension. | **基**：必须同时验证/声明张成性和线性无关性，并写出维数。
- **For direct sum**: prove BOTH V = U + W AND U ∩ W = {0}. | **直和**：必须**同时证明** V = U + W 和 U ∩ W = {0}。
- **Dimension formula**: state "by the Grassmann formula" before writing it. | **维数公式**：写之前先点名 "由 Grassmann 公式"。
- **True/false counterexamples**: must be fully explicit (small 2×2 matrices suffice). | **是非题反例**：必须完全具体（小至 2×2 矩阵即可）。
- **Uniqueness in direct sum**: if asked, prove decomposition is unique. | **直和唯一性**：若被问到，证明分解唯一。

---

## 5. Common Pitfalls | 常见陷阱

1. **Checking only two of the three subspace conditions** | **只检查了三个子空间条件中的两个**
   - All three (non-empty, +, scalar ×) are required. | 三个条件（非空、加法、数乘）缺一不可。
2. **Assuming dim(U + W) = dim U + dim W** | **假设 dim(U + W) = dim U + dim W**
   - This is wrong unless U ∩ W = {0}. The correct formula subtracts dim(U ∩ W). | 除非 U ∩ W = {0}，否则错误。正确公式要减去 dim(U ∩ W)。
3. **Using the same set as a basis for both U and W** | **用同一集合作为 U 和 W 的基**
   - Bases of U and W are independent; only in direct sum do their union form a basis for V. | U 和 W 的基相互独立；只有在直和中它们的并才构成 V 的基。
4. **Forgetting to prove 0 ∈ U** | **忘记证明 0 ∈ U**
   - The subspace test explicitly requires non-emptiness. | 子空间判别法明确要求非空性。
5. **Confusing span with linear independence** | **混淆张成与线性无关**
   - Spanning means every vector is a combination; independence means only the trivial combination gives 0. | 张成意味着每个向量都是组合；无关意味着只有平凡组合得 0。

---

## 6. Self-Check Questions | 自检问题

- [ ] Can I list all 10 vector space axioms? | 我能列出全部 10 条向量空间公理吗？
- [ ] Can I prove the subspace test for an arbitrary subset? | 我能对任意子集证明子空间判别法吗？
- [ ] Do I know the Grassmann formula and its proof sketch? | 我知道 Grassmann 公式及其证明梗概吗？
- [ ] Can I prove that U ∩ W is always a subspace? | 我能证明 U ∩ W 总是子空间吗？
- [ ] Can I prove that U ∪ W is a subspace only if U ⊆ W or W ⊆ U? | 我能证明 U ∪ W 是子空间 ⇔ U ⊆ W 或 W ⊆ U 吗？
- [ ] Do I remember that in finite dimensions, U ⊆ W and dim U = dim W implies U = W? | 我记得有限维时，U ⊆ W 且 dim U = dim W 可推出 U = W 吗？
- [ ] Can I give a concrete counterexample for a false vector-space claim? | 我能为一个错误的向量空间命题给出具体反例吗？
