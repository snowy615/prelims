# IPS-06 — Horn Formulas & The Compactness Theorem

**Frequency:** new with IPS — 2025 Q6(c) (7 marks) and Q6(d) (6 marks).

---

## 1. Horn formulas

**Definition:** a CNF formula in which every clause has **at most one positive literal**. Clause shapes: facts `{A}`, rules `{¬A₁,…,¬Aₙ, B}` (= A₁∧…∧Aₙ → B), goals `{¬A₁,…,¬Aₙ}`.

**Examples part (2 marks):** satisfiable: `{A}, {¬A, B}, {¬B, C}` (set everything true). Unsatisfiable: `{A}, {¬A, B}, {¬A, ¬B}` — adjust to meet the question's clause/literal counts exactly; **verify by exhibiting the assignment / short contradiction**.

**Closure under intersection (3 marks — the 2025 proof, reproduce):** define `(A∩B)(x) = min(A(x),B(x))`. Claim: A ⊨ F and B ⊨ F ⇒ A∩B ⊨ F. Per clause C: if C has no positive literal, some ¬x ∈ C with A(x)=0 or B(x)=0 ⇒ min = 0 ⇒ C satisfied. If C = {¬x₁,…,¬xₙ, y}: suppose A∩B falsifies C, i.e. min(A(xᵢ),B(xᵢ)) = 1 for all i and min(A(y),B(y)) = 0. Then A and B both have all xᵢ true; whichever of A,B has y = 0 falsifies C — contradiction. ∎

**Unique minimal model (2 marks):** finitely many variables ⇒ finitely many satisfying assignments; intersect them all (∩ is associative/commutative, preserves satisfaction by the above) to get A_min; A_min ≤ B for every satisfying B since min over a set is ≤ each member. Uniqueness: two minimal models A,A′ give A∩A′ ⊨ F below both, so A = A∩A′ = A′.

**Dual variant to expect:** clauses with at most one **negative** literal, `(A∪B)(x) = max(...)`, unique **maximal** model — same proof with 0/1 swapped.

**Marking-unit algorithm (possible add-on):** satisfiability of Horn formulas in polynomial time — repeatedly set true the head of any rule whose body is already all true (starting from facts); unsatisfiable iff a goal clause becomes all-true-body. The set marked = the minimal model.

## 2. Compactness

**Statement (1 mark — exact):** A (countably) infinite set S of propositional formulas is satisfiable **iff every finite subset** of S is satisfiable.

**Application template (5 marks) — the pattern is always the same:**

1. **Introduce variables** encoding the infinite object (e.g. `x_v` = "vertex v on the path" / "v gets colour i": `x_{v,i}`).
2. **Write clause groups** saying the object is locally correct, noting each clause is finite:
   - existence/uniqueness per node (e.g. exactly one colour: `x_{v,1} ∨ … ∨ x_{v,k}` and `¬x_{v,i} ∨ ¬x_{v,j}`);
   - local consistency (e.g. for each edge (u,v): `¬x_{u,i} ∨ ¬x_{v,i}`; for trees: the chosen vertex at depth d+1 is a child of the one at depth d).
3. **Finite subsets are satisfiable:** a finite subset mentions finitely many vertices/depths; use the hypothesis (every finite subgraph k-colourable / finitely-branching infinite tree has arbitrarily deep nodes — König-style argument or the given assumption) to satisfy it.
4. **Apply compactness:** the whole set is satisfiable; **read off the infinite object** from the satisfying assignment, checking it really is an infinite path / proper colouring.

**Worked instances to rehearse:** König's lemma (2025 Q6(d): infinite finitely-branching tree has an infinite path — for step 3 use: some child subtree is infinite, by pigeonhole, since the tree is finitely branching); de Bruijn–Erdős (finite k-colourability ⇒ k-colourability); extending partial orders; infinite Ramsey-flavoured statements.

## 3. Mark-scheme checklist

- [ ] Horn definition: "at most one positive literal per clause".
- [ ] Case split per clause type in closure proofs; contradiction explicit.
- [ ] Compactness stated with "every finite subset".
- [ ] Application: variables defined in words, clause groups displayed, finite-satisfiability argued (don't skip!), object recovered from the assignment.
