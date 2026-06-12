# IPS-04 — Sequent Calculus (LK) Proofs

**Frequency:** brand new in 2025 reform, Q6(b) 4 marks. Expect 1–2 sequent parts in every future paper.

---

## 1. What the sequent calculus is

A sequent has the form **Γ ⇒ Δ** where Γ (left) and Δ (right) are finite multisets of formulas. Intuition: the conjunction of Γ entails the disjunction of Δ. A proof is a tree rooted at the goal sequent, with axioms (A ⇒ A) at the leaves.

**Key difference from ND:** every rule is *local* — no assumption boxes, no discharge. The structural rules (WL, WR, CL, CR, IL, IR) let you weaken, contract, and permute formulas on either side.

## 2. The rules you must know (reform 2025 printed them)

**Structural:**
- WL: weaken left; WR: weaken right.
- CL: contract left; CR: contract right.
- IL/IR: interchange (permute) on left / right.

**Logical — left rules (decompose a formula on the left):**
- ∧L: from A, B, Γ ⇒ Δ infer A ∧ B, Γ ⇒ Δ
- ∨L: from A, Γ ⇒ Δ and B, Γ ⇒ Δ infer A ∨ B, Γ ⇒ Δ
- →L: from Γ ⇒ Δ, A and B, Γ ⇒ Δ infer A → B, Γ ⇒ Δ
- ¬L: from Γ ⇒ Δ, A infer ¬A, Γ ⇒ Δ

**Logical — right rules (decompose a formula on the right):**
- ∧R: from Γ ⇒ Δ, A and Γ ⇒ Δ, B infer Γ ⇒ Δ, A ∧ B
- ∨R: from Γ ⇒ Δ, A infer Γ ⇒ Δ, A ∨ B (or B)
- →R: from A, Γ ⇒ Δ, B infer Γ ⇒ Δ, A → B
- ¬R: from A, Γ ⇒ Δ infer Γ ⇒ Δ, ¬A

**Quantifier:**
- ∀L: from A(t), Γ ⇒ Δ infer ∀x·A(x), Γ ⇒ Δ (any term t)
- ∀R: from Γ ⇒ Δ, A(c) infer Γ ⇒ Δ, ∀x·A(x) (**c fresh**)
- ∃L: from A(c), Γ ⇒ Δ infer ∃x·A(x), Γ ⇒ Δ (**c fresh**)
- ∃R: from Γ ⇒ Δ, A(t) infer Γ ⇒ Δ, ∃x·A(x) (any term t)

## 3. The backwards sequent strategy

1. **Apply invertible rules first** (work from the bottom up). ∧R, ∨L, →R, ∀R, ∃L are invertible — safe to apply anytime.
2. **Non-invertible rules** (∨R, ∧L, →L, ∃R, ∀L) may require choosing a term or splitting; apply them when the goal is obvious or when the other side is an axiom.
3. **Axiom hunt:** whenever a formula appears on both sides, use the axiom A ⇒ A plus structural rules to bring it to the front.

## 4. Worked template (the 2025 style)

**Goal:** ⊢ ∃x·∀y·(P(x) ∨ P(y))
Sequent form: ⇒ ∃x·∀y·(P(x) ∨ P(y))

Backwards:
- ∃R with witness c: ⇒ ∀y·(P(c) ∨ P(y))
- ∀R with fresh d: ⇒ P(c) ∨ P(d)
- ∨R: ⇒ P(c) — wait, need both sides. Actually ∨R lets us pick one side. Since c is fixed and d is fresh, we can use ∨R on the right disjunct with d = c? No — need to get it to work for *all* d.
- Better: from axiom P(c) ⇒ P(c), weaken to P(c) ⇒ P(c), P(d) (WR), then ∨R gives P(c) ⇒ P(c) ∨ P(d). But we need empty left side. Use ∀L on the left with x = c? Wait, we have no ∀ on the left.
- Actually, the 2025 proof: start with axiom P(c) ⇒ P(c), weaken to P(c) ⇒ P(c), P(d), ∨R to get P(c) ⇒ P(c) ∨ P(d), then ∀R (d fresh) gives P(c) ⇒ ∀y·(P(c) ∨ P(y)), then ∃R with witness c gives the goal.

## 5. Common mistakes

- **Applying ∀R / ∃L with a non-fresh variable** — same proviso as ND; the fresh variable must not appear in the conclusion sequent.
- **Forgetting the structural rules** — you may need WL or IL to bring a formula to the front so a logical rule applies.
- **Confusing left and right** — on the left, ∧ is easy (one premise), ∨ is hard (two premises); on the right, ∨ is easy, ∧ is hard.

## 6. Mark-scheme checklist

- [ ] Every inference is named with the rule (WL, ∧R, ∀L, etc.).
- [ ] Axioms are A ⇒ A, not just “obvious”.
- [ ] Freshness stated for ∀R and ∃L.
- [ ] Tree drawn with conclusion below, premises above (or indented linear form with line numbers).
- [ ] Invertible rules applied before non-invertible ones where possible.
