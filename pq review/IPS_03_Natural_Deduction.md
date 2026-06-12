# IPS-03 — Natural Deduction Proofs (Propositional & First-Order)

**Frequency:** examined in every IFP year (2013–2021) and 2025 Q5(c). 4–6 marks per proof. Compulsory Q5 guarantees at least one ND proof.

---

## 1. The backwards strategy (write this on your rough paper)

1. **Goal is φ → ψ** → assume φ, prove ψ (→-intro).
2. **Goal is φ ∧ ψ** → prove φ and ψ separately (∧-intro).
3. **Goal is ¬φ** → assume φ, derive ⊥ (¬-intro).
4. **Goal is φ ∨ ψ** → often easier via RAA (assume ¬(φ ∨ ψ), get ⊥) or prove one disjunct directly.
5. **Goal is atomic / stuck** → RAA (assume ¬goal, derive ⊥) or look for an elimination rule from a premiss.
6. **Premiss is φ ∨ ψ** → do ∨-elim immediately (case split); this is the most common source of “two essentially different proofs”.

## 2. The most rehearsed proofs (know cold)

**De Morgan — intuitionistically valid:**
- ¬(P ∨ Q) ⊢ ¬P ∧ ¬Q
- ¬P ∧ ¬Q ⊢ ¬(P ∨ Q)
- ¬P ∨ ¬Q ⊢ ¬(P ∧ Q)

**De Morgan — needs classical reasoning:**
- ¬(P ∧ Q) ⊢ ¬P ∨ ¬Q (needs LEM or ¬¬-elim). Standard proof: assume ¬(P ∧ Q). By LEM on P, case P gives ¬Q from the assumption (since P ∧ Q would be provable), hence ¬P ∨ ¬Q by ∨-intro-R; case ¬P gives it by ∨-intro-L.

**RAA (derived rule, 2 marks):**
> Assume ¬φ, derive ⊥, conclude φ. Justification: the subproof ¬φ … ⊥ gives ¬¬φ by ¬-intro, then φ by ¬¬-elim. Conversely, from RAA derive ¬¬-elim: given ¬¬φ, assume ¬φ, ¬-elim gives ⊥, RAA discharges to φ.

**⊢ φ ∨ ¬φ (LEM, 4 marks — 2019):**
> Assume ¬(φ ∨ ¬φ). Assume φ → φ ∨ ¬φ (∨-intro-L) → ⊥. So ¬φ (¬-intro). Then φ ∨ ¬φ (∨-intro-R) → ⊥. So ¬¬(φ ∨ ¬φ), and ¬¬-elim finishes.

**Quantifier dualities (predicate ND):**
- ∀x·¬P(x) ⊢ ¬∃x·P(x): assume ∃x·P(x), ∃-elim with fresh witness c, get P(c), but ∀x·¬P(x) gives ¬P(c), ⊥.
- ∃x·(P(x) → Q(x)), ∀x·P(x) ⊢ ∃x·Q(x): ∃-elim with fresh c, get P(c) → Q(c), ∀-elim gives P(c), →-elim gives Q(c), ∃-intro.

**Russell / Barber (2016, 2019):**
⊢ ¬∃b·∀p·(S(b,p) ↔ ¬S(p,p)). Strategy: assume it, ∃-elim with witness b₀, ∀-elim at p := b₀, derive S(b₀,b₀) ↔ ¬S(b₀,b₀), get ⊥ by case analysis on S(b₀,b₀).

## 3. Quantifier proofs — freshness is the examiner trap

**Freshness (1–3 marks every year):** a variable v is fresh in a context if it occurs free in no premiss, undischarged assumption, nor in the conclusion.
- **Why it matters:** without it, from ∃x·P(x) one could “prove” ∀x·P(x) by ∃-elim then ∀-intro.
- **Informal justification over finite domain {a₁,…,aₙ}:** ∃x·φ(x) behaves as φ(a₁) ∨ … ∨ φ(aₙ), and ∃-elim is n-fold ∨-elim where the proof of κ from φ(v) is uniform in v.

**∀-intro:** from φ(c) with c fresh, conclude ∀x·φ(x).
**∃-elim:** from ∃x·φ(x) and a subproof from φ(c) (c fresh) to κ, conclude κ. The subproof must be *uniform* — it cannot mention c in the final conclusion κ.

## 4. Layout and mark-scheme habits

- **Number every line.** Cite rule and line numbers: "→-elim 4, 6".
- **Mark assumptions and where they are discharged.** Use indentation / boxes; close every box.
- **Check freshness before ∀-intro / ∃-elim** and write "where c is fresh".
- **Justify derived rules** with a schematic proof using only base rules (if asked).
- **State lemmas you reuse** (e.g. LEM) and prove them once if the rubric requires.
- **Budget:** 4–6 marks ≈ 7–10 minutes; do not write 20 lines for a 4-mark proof.

## 5. Mark-scheme checklist

- [ ] Backwards rule applied at each step; no unnecessary detours.
- [ ] Every line numbered; every rule named with line references.
- [ ] Boxes / assumptions clearly shown and discharged.
- [ ] Freshness explicitly stated for ∀-intro and ∃-elim.
- [ ] Classical steps (LEM, RAA, ¬¬-elim) identified when used.
- [ ] Two essentially different proofs differ in the *order* of backwards rule application (typically ∨-elim first vs last).
