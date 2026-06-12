# IPS-02 — Structural Induction on Formulas

**Frequency:** IFP 2022 (3 marks), 2023 (2 marks); a standard 2–4-mark part in the new style.

---

## 1. The template (write this skeleton every time)

To prove a property P(φ) for all propositional formulas φ:

1. **Base case:** φ is a variable (or constant ⊤/⊥ if the syntax has them). Show P directly.
2. **Inductive step, one case per connective:** assume P(φ₁), P(φ₂) (induction hypothesis — *name it explicitly*) and show P for `¬φ₁`, `(φ₁ ∧ φ₂)`, `(φ₁ ∨ φ₂)`, `(φ₁ → φ₂)` — exactly the connectives the question's grammar allows. **Read the grammar:** 2022's formula "only uses ∨, ∧ and →" — then there is no ¬ case, and the claim may *depend* on that.
3. **Conclusion:** by structural induction, P holds for all formulas.

## 2. Worked classics

**Bracket counting (2023):** P(φ) = "φ has equally many `(` and `)`". Base: a variable has 0 of each. Step: `(φ₁ ∧ φ₂)` has L₁+L₂+1 left and R₁+R₂+1 right brackets; by IH L₁=R₁, L₂=R₂. ¬φ₁ adds none (or one pair, per the grammar — match the official syntax given).

**All-variables-true evaluation (2022):** P(φ) = "if every variable is 1 then ⟦φ⟧ = 1", for ∨/∧/→-formulas. Base: variable evaluates to 1. Step: 1∧1 = 1, 1∨1 = 1, 1→1 = 1. (Note ¬ would break it — worth a remark.)

**Other likely targets:** number of connectives vs subformulas; substitution lemma `⟦φS⟧_A = ⟦φ⟧_{A∘S}`; every formula has equivalent NNF; duality `⟦φ̄⟧_A = ¬⟦φ⟧_{¬A}`.

## 3. Phrases that earn the marks

- "By the induction hypothesis applied to φ₁ and φ₂ …"
- "The cases for ∨ and → are analogous: …" (allowed, but show at least one connective fully).
- "Hence by structural induction the property holds for all formulas of the given grammar."

## 4. Mark-scheme checklist

- [ ] Property P stated as a displayed predicate before the induction.
- [ ] Base case = variables, not "small formulas".
- [ ] One case per connective *in the given grammar*, IH cited.
- [ ] Closing sentence present.
