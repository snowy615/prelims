# IPS-05 — Resolution, Ground Resolution & Herbrand's Theorem

**Frequency:** new with IPS — 2025 Q5(a)(b) (7 marks) and Q6(a) (3 marks). Likely every year.

---

## 1. Propositional resolution

**Resolvent (definition, 1 mark — verbatim):** Let C₁, C₂ be clauses (sets of literals) with L ∈ C₁ and ¬L ∈ C₂ for some literal L. Then `R = (C₁ \ {L}) ∪ (C₂ \ {¬L})` is a resolvent of C₁ and C₂.

**Soundness of the step:** any assignment satisfying C₁ and C₂ satisfies R (case split on L). **Refutation completeness:** F (in CNF) is unsatisfiable iff the empty clause □ is derivable by resolution.

**Refutation layout:** number the clauses; each new line `Rᵢ = resolvent of (j),(k) on variable X`; end at □. For "F is a tautology" questions: **negate first**, convert ¬F to CNF, refute.

**Theory-lemma style (2025 Q6(a)(ii) pattern):** properties preserved when fixing variable values — if x is set true, clauses containing x disappear, ¬x is deleted from clauses; argue the property clause-by-clause on what remains, taking subsumption (R ⊆ D) into account.

## 2. Herbrand machinery (state-and-prove, 3 marks)

Setting: closed formula in **Skolem form** `F = ∀x₁…∀xₙ F*` with quantifier-free F*.

- **Herbrand universe T(F):** all variable-free terms over the function symbols/constants of F (add a constant c if none). Note: infinite as soon as any function symbol is present.
- **Herbrand structure:** universe T(F), each term interprets as itself; **Herbrand model:** a Herbrand structure satisfying F.
- **Herbrand expansion:** `E(F) = { F*[x₁/t₁,…,xₙ/tₙ] : tᵢ ∈ T(F) }` — a set of propositional (ground) formulas.

**Herbrand's theorem:** F is satisfiable **iff** F has a Herbrand model. Equivalently (Gödel–Herbrand–Skolem): F is satisfiable iff E(F) is (propositionally) satisfiable; and by compactness: **F is unsatisfiable iff some *finite* subset of E(F) is unsatisfiable.**

**Proof sketch to reproduce (assuming the translation lemma):** (⇐) a Herbrand model is a model. (⇒) Let A ⊨ F. Define the Herbrand structure B by interpreting each predicate on ground terms via their values in A (`P^B(t₁,…,tₖ) :⇔ A ⊨ P(t₁,…,tₖ)`, using A's evaluation of terms). The translation lemma gives B ⊨ F. ∎

## 3. Ground resolution template (4 marks, 2025 Q5(b))

Task: show a first-order sentence φ is a tautology (valid).

1. **Negate:** consider ¬φ; convert to **Skolem form** (push ¬ in, replace ∃ by Skolem functions/constants over the universal prefix). Say: "Skolemisation preserves satisfiability".
2. **Clause form:** write the quantifier-free matrix in CNF as clauses.
3. **Choose ground instances** from the Herbrand universe (the art: pick instantiations making complementary literals appear).
4. **Resolve to □.** Conclude: a finite unsatisfiable subset of the Herbrand expansion exists, so ¬φ is unsatisfiable, so φ is valid (Herbrand + ground resolution completeness).

**Worked shape (2025):** φ = ∀x∃y¬(P(y,x) ↔ ¬P(y,y)). ¬φ ⇒ ∃x∀y(P(y,x) ↔ ¬P(y,y)) ⇒ Skolem constant a: ∀y(P(y,a) ↔ ¬P(y,y)). Clauses: `{¬P(y,a), ¬P(y,y)}` and `{P(y,y), P(y,a)}`. Ground at y := a: `{¬P(a,a)}` (both literals collapse) and `{P(a,a)}`; resolve → □.

## 4. Mark-scheme checklist

- [ ] Resolvent definition set-theoretic and exact.
- [ ] Negate-then-refute direction stated for tautology questions.
- [ ] Skolemisation: existentials → functions of the *preceding universals*; "preserves satisfiability".
- [ ] Ground instances written explicitly (y := a etc.).
- [ ] Final sentence connecting □ to validity of the original formula.
