# IPS-07 — First-Order Semantics: Structures, Models, Expressibility, Spectra

**Frequency:** IFP 2022/2023 (structures over orders, expressibility) + 2025 Q5(d) (spectra, 9 marks). A signature topic of the new course.

---

## 1. Vocabulary

A **structure** A = (U_A, interpretations of the signature σ). `A ⊨ F` / `⟦F⟧_A = 1`: A is a **model** of F. Valid = true in all structures; satisfiable = true in some. **Finite model**: |U_A| finite.

## 2. "Write a sentence distinguishing structures" template (2022 Q8 style, 2–5 marks)

Given structures over σ = {<} (e.g. (0,1), [0,1], ℤ): find a property true in one, false in the other, expressible in FO(σ):

- **Endpoints:** [0,1] has a least element: `∃x∀y (x = y ∨ x < y)`; (0,1) does not.
- **Discreteness (ℤ):** every element has an immediate successor: `∀x∃y (x < y ∧ ¬∃z (x < z ∧ z < y))`; fails in dense orders.
- **Density:** `∀x∀y (x < y → ∃z (x < z ∧ z < y))` — separates (0,1)/[0,1] from ℤ.
- **Gaps** ([0,1]∪[2,3]): "there exist a < b with nothing between, but a is not the maximum and b not the minimum" — combine the pieces above.

Method: list order-theoretic features (min/max, density, discreteness, gaps, bounds), pick one that differs, formalise. **Always sanity-check your sentence in *both* structures.**

**Cardinality forcing (2022 Q8(e)):** with only unary predicates and no `=`, force ≥ n elements by making n predicate-combinations simultaneously inhabited: `∃x(P₁(x) ∧ ¬P₂(x)) ∧ ∃x(¬P₁(x) ∧ P₂(x)) ∧ …` — distinct cells must contain distinct elements.

## 3. Spectra (2025 Q5(d) — learn this cold)

**Definition:** `S(F) = { |U_A| : A ⊨ F, U_A finite }` ⊆ ℕ. M is a spectrum if M = S(F) for some FO sentence F (equality allowed).

**Toolkit sentences:**
- `λ≥n := ∃x₁…∃xₙ ⋀_{i<j} xᵢ ≠ xⱼ` ("at least n elements"); `λ=n := λ≥n ∧ ¬λ≥(n+1)`.
- **Finite M = {n₁,…,n_k}:** F = `λ=n₁ ∨ … ∨ λ=n_k`.
- **Co-finite M:** complement is {m₁,…,m_j}: F = `¬λ=m₁ ∧ … ∧ ¬λ=m_j`.
- **Even numbers:** axiomatise a perfect pairing: binary f (or relation) with `∀x (f(x) ≠ x ∧ f(f(x)) = x)` — models are exactly the sets partitioned into 2-cycles ⇒ spectrum = positive evens (add `∨ λ=0`-style adjustments as the question's convention on 0 requires).
- **Multiples of k:** f with f^k(x) = x and f^i(x) ≠ x for 0 < i < k.
- **Squares {n²}:** axiomatise a grid: two equivalence relations R (rows) and C (columns) such that every R-class meets every C-class in **exactly one** element; then |U| = (#rows)·(#cols); force #rows = #cols with a bijection between R-classes and C-classes (e.g. a function g pairing each row with a column via the diagonal of a chosen element). Conclude |U| = n².
- **Factorials {n!} (2025, hint-driven):** the number of linear orders on n elements is n!. Encode: U = a set of "tables" each of which is a linear order over a common base set... Exam answer: take a structure with a base set B (a unary predicate), and let the universe be the set of all linear orderings of B — formalised by a ternary relation L(o,x,y) "in ordering o, x < y" with axioms: for each o, L(o,·,·) is a strict linear order on B; orderings with the same order relation are equal (extensionality); **every** linear order on B occurs (a completeness axiom via: for every o and adjacent pair, the swapped order exists — generating all n! permutations). Then |U| = |B|!. Sketch at this level of detail scores; full formalisation is long — present axioms in words + key formulas.

**Closure facts quotable:** spectra are closed under ∪ and ∩ (disjoin/conjoin with disjoint signatures); whether closed under complement is open (Asser's problem) — nice final remark, not required.

## 4. Mark-scheme checklist

- [ ] `λ≥n` written out once, reused.
- [ ] For each "M is a spectrum" claim: give F, prove **both** directions — every model has size in M, every size in M has a model (construct it!).
- [ ] Sentences checked against each given structure.
- [ ] Equality used only if the question permits it.
