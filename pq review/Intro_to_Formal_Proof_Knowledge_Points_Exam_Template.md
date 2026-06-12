# Introduction to Formal Proof — Knowledge Points & Exam Template (CS4 / A10101W1)

Based on analysis of past papers 2013–2023 (Questions 7–8 of the CS4 paper; 2 questions × 20 marks).

---

## 1. Exam Format

- Q7 = propositional logic; Q8 = predicate (first-order) logic. 20 marks each.
- Proofs may be given as natural deduction trees, sequent trees, or indented linear form — **pick one style and use it consistently** (indented linear form is fastest and easiest to check).
- The rule sheets (connective rules + quantifier rules) are printed in the paper — you do not memorise their typesetting, but you must know exactly how each rule behaves.
- **Major trend break in 2022–23:** new style adds *semantics*: DNF/CNF, truth tables, structural induction, FO over structures (orders, graphs), satisfiability, free-for-substitution, negation normal form, finite vs infinite models. 2013–2021 is almost purely proof construction. Prepare both.

## 2. Topic Frequency Table

| Topic | Years examined | Frequency |
|---|---|---|
| Natural deduction propositional proofs (⊢ sequents) | every year 13–21, 22e, 23e | ★★★★★ |
| De Morgan-style proofs (¬(P∨Q) ⊢ ¬P∧¬Q, ¬(P∧Q) ⊢ ¬P∨¬Q) | 13, 14, 15, 17, 18, 20 | ★★★★★ |
| RAA / proof by contradiction: state, justify, use | 13, 14, 15, 16, 17, 18, 20 | ★★★★★ |
| Law of excluded middle ⊢ φ ∨ ¬φ and applications | 19, 20 | ★★★★ |
| Quantifier proofs (∀/∃ intro & elim) | every year 13–21 | ★★★★★ |
| Freshness of variables; justify ∃-elim / ∀-intro | 13, 14, 15, 17, 18, 20 | ★★★★★ |
| Quantifier swap: ∃y∀x R ⊢ ∀x∃y R (and why converse fails) | 16, 17, 20 | ★★★★ |
| Explain rules (∧-intro, ∨-elim, →-intro; boxes = subproofs) | 13, 14, 15, 16 | ★★★★ |
| "Two essentially different proofs" + compare | 13, 14, 16, 17 | ★★★★ |
| Equality rules (=-intro/elim); symmetry, transitivity | 13, 16, 21 | ★★★ |
| Design rules for new connectives (xor, nor, ↔) & derive them | 17, 18, 21 | ★★★ |
| Spot the flaw in a given (invalid) proof | 17, 20 | ★★★ |
| Derived rules (3-cases, →-distribution) justify & use | 19, 21 | ★★★ |
| Russell-paradox style: ¬∃b·∀p·(S(b,p) ↔ ¬S(p,p)) | 16, 19 | ★★★ |
| DNF / CNF construction & arguments | 22, 23 | ★★★ (new style) |
| Truth tables & validity | 22, 23 | ★★★ (new style) |
| Structural induction on formulas | 22, 23 | ★★★ (new style) |
| FO over structures: orders, graphs; distinguish models | 22, 23 | ★★★ (new style) |
| Substitution; free-for; renaming; negation normal form | 23 | ★★ (new style) |
| Finite vs infinite models | 23 | ★★ (new style) |

## 3. Core Knowledge Points & Answer Templates

### 3.1 The Rules — what you must be able to *explain*

- **∧-intro:** from φ and ψ conclude φ∧ψ. **∨-elim:** from φ∨ψ and proofs of κ from each disjunct, conclude κ. **→-intro:** a subproof assuming φ ending in ψ yields φ→ψ.
- **The boxes:** delimit hypothetical subproofs — the assumption is *local*, may only be used inside, and is *discharged* when the rule is applied. (This explanation is worth 1–2 marks nearly every pre-2017 year.)
- **¬ is defined via ⊥:** ¬-intro = a subproof from φ to ⊥ gives ¬φ; ¬-elim = φ, ¬φ ⊢ ⊥; ⊥-elim = anything follows; ¬¬-elim = classical.

### 3.2 RAA / Excluded Middle (memorise both proofs)

**RAA (derived rule):** assume ¬φ, derive ⊥, conclude φ.
Justification from base rules: the subproof ¬φ … ⊥ gives ¬¬φ by ¬-intro, then φ by ¬¬-elim.
Conversely, ¬¬-elim from RAA: given ¬¬φ, assume ¬φ; ¬-elim gives ⊥; RAA discharges to φ.

**⊢ φ ∨ ¬φ (the 2019 4-marker — know cold):**
Assume ¬(φ ∨ ¬φ). Assume φ → φ∨¬φ (∨-intro-L) → ⊥. So ¬φ (¬-intro). Then φ∨¬φ (∨-intro-R) → ⊥. So ¬¬(φ∨¬φ), and ¬¬-elim finishes.

**De Morgan ¬(P∧Q) ⊢ ¬P∨¬Q needs classical reasoning** (case split on P via LEM, or double-negation) — the intuitionistically-valid three (¬(P∨Q)⊢¬P∧¬Q, ¬P∧¬Q⊢¬(P∨Q), ¬P∨¬Q⊢¬(P∧Q)) do not.

### 3.3 Propositional Proof Strategy (the "backwards" method)

1. Work **backwards from the goal**: → goal ⇒ assume antecedent; ∧ goal ⇒ split; ¬ goal ⇒ assume and aim for ⊥; atomic goal ⇒ look for →-elim / ∨-elim / RAA.
2. Work **forwards from premisses**: break down ∧ and instantiate →.
3. ∨ in premisses ⇒ case analysis (∨-elim) — do this early.
4. Stuck on an atomic/negative goal ⇒ RAA.
5. "Two essentially different proofs": typically (i) ∨-elim outermost vs ∧-intro outermost — the order of backwards rule application; explain that doing ∨-elim first duplicates the remaining goal in each branch (more steps), doing it last does not.

### 3.4 Quantifier Proofs

**Freshness (asked nearly every year, 1–3 marks):** a variable v is fresh in a context if it occurs free in no premiss, undischarged assumption, nor in the conclusion. The proviso prevents inferring a universal/general fact from a particular witness — e.g. without it, from ∃x·P(x) one could "prove" ∀x·P(x). Justify ∃-elim informally over a finite domain {a₁,…,aₙ}: ∃x·φ(x) behaves as φ(a₁)∨…∨φ(aₙ), and the rule is n-fold ∨-elim where the proof of κ from φ(v) is *uniform* in v.

**Standard sequents to have rehearsed:**
- ∀x·¬P(x) ⊢ ¬∃x·P(x), and the other three quantifier-negation dualities.
- ∃y·∀x·R(x,y) ⊢ ∀x·∃y·R(x,y); converse not provable — countermodel: R(x,y) = "y = x" over a domain with ≥ 2 elements ("everyone has a birthday" vs "one day is everyone's birthday").
- ∀x·(P(x)→Q(x)), ∃x·P(x) ⊢ ∃x·Q(x) — **know the valid shape**: ∃-elim must scope over the whole derivation, and the witness must be fresh; the classic invalid 2017 proof reuses bound x / breaks scoping.
- Russell/barber: ⊢ ¬∃b·∀p·((S(b,p)→¬S(p,p)) ∧ (¬S(p,p)→S(b,p))). Strategy: assume it, ∃-elim with witness b₀, ∀-elim at p := b₀, derive S(b₀,b₀) ↔ ¬S(b₀,b₀), get ⊥ by LEM/case analysis.

**Equality:** =-intro: ⊢ T = T. =-elim: from T₁ = T₂ and φ(T₁) infer φ(T₂). Symmetry: apply =-elim to φ(z) := (z = T₁). Transitivity: substitute in T₁ = T₂ using T₂ = T₃.

### 3.5 Designing Rules for New Connectives (2017 ↔, 2018 xor, 2021 nor)

Template: give one intro rule per way of making it true, elim rule(s) letting you extract exactly what it guarantees; then show soundness by *deriving* the rules from the definition (e.g. φ n̂or ψ ≝ ¬φ∧¬ψ) using the base system. For "Professor Dee Duction"-style questions (define ∨ as ¬(¬φ∧¬ψ)): derive ∨-intro-L/R by ¬-intro, and ∨-elim by RAA + case-style reasoning.

### 3.6 New-Style Semantics (2022–23) — must prepare

**DNF/CNF template:**
1. Build truth table; DNF = one conjunct per satisfying row (conjunction of literals); CNF = one clause per falsifying row (disjunction of negated literals).
2. Argument questions (2022 "no single-literal conjunct"): a single literal would make φ true in half of all assignments regardless of other variables — check φ's truth table denies this.

**Structural induction template:** base case = atomic formulas/variables; inductive step = one case per connective; state the induction hypothesis explicitly. (2022: all-variables-1 ⇒ formula evaluates to 1 for {∨,∧,→}; 2023: balanced brackets.)

**FO over structures:** practise writing sentences distinguishing (0,1), [0,1], Z with <: existence of least/greatest element (∃x∀y(x = y ∨ x < y) true in [0,1] not (0,1)); density ∀x∀y(x<y → ∃z(x<z∧z<y)) true in intervals, false in Z; discreteness/successors for Z; gaps for [0,1]∪[2,3].
Graphs: vertex covers, independent sets — bounded-size statements via ∃x₁∃x₂∃x₃ + ∀-clauses with equality disjunctions.

**Finite model arguments (2023):** an irreflexive transitive relation with ∀x∃y·Q(x,y) has no finite model — follow the chain x₁ Q x₂ Q x₃…; transitivity + irreflexivity force all distinct; infinite model: (N, <).

**Substitution:** T is free for x in φ if no free occurrence of x lies in the scope of a quantifier binding a variable of T. If not free-for: rename bound variables, then substitute. Negation normal form: push ¬ through with De Morgan + ¬∀ = ∃¬, ¬∃ = ∀¬, ¬(φ→ψ) = φ∧¬ψ.

## 4. Mark-Scheme Habits

1. Number every line; cite the rule and the lines it uses ("→-elim 4, 6").
2. Mark assumptions and where they are discharged (indentation/boxes must close).
3. Check freshness *before* using ∀-intro/∃-elim, and say "where v is fresh".
4. If asked to "justify" a derived rule, give a schematic proof using only base rules.
5. For "comment on the possibility of proving" — give a *countermodel* (small finite domain, explicit relation) — never just "it's not provable".
6. State lemmas you reuse (e.g. LEM) and prove them once if the rubric requires.
7. Budget: 20 marks ≈ 36 minutes; bank the bookwork (rule explanations, freshness) first.

## 5. Predicted Hot Topics for 2026

Following 2022–23 examiners: one DNF/CNF + truth-table/validity part; one structural-induction part; natural deduction proofs kept but shorter (5–6 marks); FO model-distinguishing over orders or graphs; substitution / negation normal form; a finite-vs-infinite model argument. Still rehearse classic LEM/RAA and quantifier-swap proofs — they reappear inside new-style parts.
