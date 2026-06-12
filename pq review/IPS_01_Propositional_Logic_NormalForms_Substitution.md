# IPS-01 — Propositional Logic: Truth Tables, DNF/CNF, Equivalence, Substitution

**Frequency:** IFP 2022 & 2023 (both questions), background for everything in IPS. Short parts (2–5 marks) at the start of Q5/Q6.

---

## 1. Truth tables (2–4 marks)

Columns: one per variable, one per relevant subformula, one for the whole formula; 2ⁿ rows in a fixed order (count down from all-1s or up from all-0s — be consistent). A formula is **valid (a tautology)** iff its column is all 1; **satisfiable** iff some row gives 1; two formulas are **equivalent** iff their columns agree.

## 2. DNF / CNF construction (2–5 marks)

- **DNF from truth table:** one conjunct (minterm) per satisfying row: the conjunction of each variable or its negation as in that row; disjoin them. If no satisfying rows, take `A ∧ ¬A`.
- **CNF from truth table:** one clause per *falsifying* row: disjunction of the **negations** of the row's literals.
- Quicker when possible: rewrite with equivalences (eliminate →/↔, push ¬ in by De Morgan, distribute). State which method you use; simplify if asked.
- **Argument parts** (2022): e.g. "no single-literal conjunct in any DNF of φ" — argue semantically: a single-literal conjunct `L` would make every assignment satisfying `L` satisfy φ; exhibit an assignment satisfying `L` but falsifying φ.

## 3. Standard equivalences (quote freely)

De Morgan; double negation; `φ→ψ ≡ ¬φ∨ψ`; `φ↔ψ ≡ (φ→ψ)∧(ψ→φ)`; distributivity both ways; absorption; `φ→(ψ→χ) ≡ (φ∧ψ)→χ`.

## 4. Substitution (2023; 3 marks)

A substitution `S` maps variables to formulas; `φS` replaces **every occurrence of each variable simultaneously** (no cascading: occurrences of variables introduced by S are not substituted again). Compute mechanically, bracket generously, then simplify with §3 equivalences. Key fact worth quoting: substitution preserves validity — if φ is a tautology then so is φS (substitution lemma).

## 5. "Construct a formula with exactly k satisfying assignments" (2023, 7 marks — counting style)

Write k in binary: `k = Σ 2^{i_1} + ... `. Over variables x₁..xₙ, the formula `x_1 ∧ ... ∧ x_j` has exactly `2^{n−j}` satisfying assignments; combine *disjoint* such blocks with ∨ (make them disjoint by prefixing complementary literals, mirroring binary expansion). Total length O(n²). Justify: blocks are pairwise unsatisfiable together, so counts add.

## 6. Mark-scheme checklist

- [ ] Truth table has subformula columns (method marks).
- [ ] DNF/CNF: say which rows generated which terms.
- [ ] Substitution simultaneous, then explicit simplification steps.
- [ ] Counting constructions: disjointness argued, length bound stated.
