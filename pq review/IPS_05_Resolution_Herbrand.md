# IPS-05 — Resolution & Herbrand's Theorem

**Frequency:** new in 2025 reform, Q5(a)(b) and Q6(a). 7 marks total in the compulsory questions. Highly likely to recur.

---

## 1. Resolution — the definitions (1–2 marks)

**Resolvent of two clauses C₁ and C₂:** suppose C₁ contains a literal L and C₂ contains the complementary literal ¬L (or vice versa). Then the resolvent is (C₁ \ {L}) ∪ (C₂ \ {¬L}).
- If the resulting set is empty, the resolvent is the **empty clause □** (unsatisfiable).
- Resolution is a **refutation-complete** rule for propositional CNF: a CNF formula is unsatisfiable iff □ can be derived by repeatedly resolving clauses from it.

**Ground resolution:** resolution on ground (variable-free) clauses. For a first-order formula, first Skolemise / instantiate variables to ground terms, then resolve.

## 2. Herbrand's Theorem (3 marks — state & prove)

**Statement:** A set S of clauses is unsatisfiable iff there exists a finite unsatisfiable set of ground instances of clauses from S.

**Proof sketch (what the mark scheme wants):**
1. **(⇒)** If S is unsatisfiable, by the completeness of resolution there is a resolution refutation of S. Each resolution step uses only finitely many ground instances; collecting all ground instances used in the refutation yields a finite unsatisfiable set.
2. **(⇐)** If a finite set of ground instances is unsatisfiable, then any model of S would satisfy all their parent clauses, hence satisfy the ground instances — contradiction. Therefore S has no model.
- You may assume the **translation lemma** (if asked to prove): a first-order formula is satisfiable iff its Skolem form is satisfiable.

**Practical use:** to prove a first-order formula F is a tautology, negate it, convert to CNF clauses S, and derive □ from ground instances of S using ground resolution.

## 3. The ground-resolution proof template (4 marks)

**Question format:** “Using ground resolution, show that the following is a tautology: ∀x·∃y·¬(P(y,x) ↔ ¬P(y,y)).”

Step-by-step:
1. **Negate the formula** and push ¬ inward to get an equivalent existential formula.
2. **Skolemise:** replace ∃y by a Skolem function (or constant if no universal variables scope over it). Here ∃y becomes a function g(x).
3. **Instantiate the universal quantifier** to a ground term (often a constant or the Skolem function applied to a constant). For simplicity, instantiate x to a constant a.
4. **Convert the matrix to CNF clauses**.
5. **Resolve ground clauses** until □ is derived.
6. **Conclude:** since the negation leads to a contradiction, the original formula is a tautology.

**Example trace (2025 Q5(b)):**
- Formula: ∀x·∃y·¬(P(y,x) ↔ ¬P(y,y))
- Negate: ¬∀x·∃y·¬(P(y,x) ↔ ¬P(y,y)) ≡ ∃x·∀y·(P(y,x) ↔ ¬P(y,y))
- Skolemise x → a (constant): ∀y·(P(y,a) ↔ ¬P(y,y))
- Instantiate y to a: P(a,a) ↔ ¬P(a,a) which is (P(a,a) → ¬P(a,a)) ∧ (¬P(a,a) → P(a,a))
- CNF: (¬P(a,a) ∨ ¬P(a,a)) ∧ (P(a,a) ∨ P(a,a)) = {¬P(a,a)}, {P(a,a)}
- Resolve: □. Done.

## 4. Closure under resolvents (2025 Q6(a)(ii))

**Definition:** F is closed under resolvents if for any resolvent R of C₁, C₂ ∈ F, there exists D ∈ F with R ⊆ D.

**Claim:** if F is closed under resolvents, then so is F₀ obtained by fixing a variable's truth value.

**Proof:** every clause in F₀ is either a clause from F (with the fixed literal removed) or the fixed literal itself. Resolving two clauses from F₀: if the resolution variable is the fixed one, the resolvent is already in F₀ (or subsumed). If it is another variable, the resolvent corresponds to a ground resolvent in F, which by closure is subsumed by some D ∈ F; the corresponding D in F₀ subsumes the resolvent there.

## 5. Mark-scheme checklist

- [ ] Resolvent definition: mention the literal and its complement, and the union minus the pair.
- [ ] Herbrand statement: both directions, mention finite set of ground instances.
- [ ] Ground resolution proof: negation, Skolemisation, ground instantiation, CNF conversion, resolution tree to □.
- [ ] Closure proof: two cases (fixed variable vs other variable), subsumption used.
