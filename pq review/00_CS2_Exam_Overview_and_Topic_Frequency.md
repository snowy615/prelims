# CS2 (Reformed 2025–2026) — Topic Frequency & Exam Priority Summary

Compiled from: IP past papers 2010–2023 (8 questions/year), Intro to Formal Proof (IFP) past papers 2013–2023 (Q7–8), and the 2025 reformed paper (A18603H1).

---

## 1. The 2025/2026 Exam Format

- **Duration:** 3 hours. **Six questions, 20 marks each** (total attempted: 100).
- **Compulsory:** Q1, Q3, Q5. Choose **2 optional** from Q2, Q4, Q6.
- **Bundles:** Q1–2, Q3–4, Q5–6 submitted separately.
- **Language:** Scala for programming; printed rule sheets for ND & sequent calculus.
- **Major change:** Old Part 3 (OOP, inheritance, polymorphism, generics, design patterns, GUI) has been **removed**. No OOP questions in 2025.

---

## 2. Imperative Programming Part 1 (Q1–Q2) — Topic Frequency

| Topic | Historical Hits | 2025 Presence | Marks if hit | Priority |
|---|---|---|---|---|
| Loop invariants / variants, pre/post, correctness proofs | **Q1 every year** (2010–2023) | Q1 (a,b,c) — 11 marks | 3–11 | ★★★★★ |
| Testing: equivalence partitioning, unit tests, boundary cases | historically rare (~5) | **Q1 (d,e,f,g) — 7 marks** | 2–7 | ★★★★★ |
| Array algorithms (scan, search, two-pointer, write + invariant) | ~40 questions | Q1, Q2 | 4–9 | ★★★★★ |
| Binary search & variants (ternary, rotated, perturbed sortedness) | 9 questions | Q2 (ii, iii) — 8 marks | 2–8 | ★★★★ |
| Complexity / running-time analysis (O-notation, justify) | 26 questions | Q2 (implicit) | 2–4 | ★★★★ |
| Sorting (quicksort, mergesort, partition, invariants) | 29 questions | implicit (c-almost sorted) | 2–10 | ★★★ |
| Floating point / overflow pitfalls | 17 questions | Q3(d) — 4 marks | 2–4 | ★★★★ |

## 3. Imperative Programming Part 2 (Q3–Q4) — Topic Frequency

| Topic | Historical Hits | 2025 Presence | Marks if hit | Priority |
|---|---|---|---|---|
| ADT spec: abstract state, DTI, post-conditions, Abs function | **28 questions** | **Q3 whole — 20 marks** | 6–7 | ★★★★★ |
| ADT implementation (mutable.Map, linked list, array) | ~25 questions | Q3 (b,c) — 10 marks | 6–7 | ★★★★★ |
| Factory methods / companion object with assertions | ~10 questions | Q3(c) — 4 marks | 2–4 | ★★★★ |
| Hashing / hash tables (properties, chained table, collision) | 5 historically | **Q4 whole — 17 marks** | 3–17 | ★★★★ |
| Trees (recursive structure, traversal, subtree search, hashing) | 20 questions | Q4(b) — 17 marks | 2–8 | ★★★★ |
| Linked lists (singly, circular, dummy header, reversal) | 17 questions | — | 3–7 | ★★★ |
| Recursion (structural induction, correctness, complexity) | 26 questions | Q4(b) | 2–6 | ★★★★ |
| Stacks / queues as ADT examples | 17 questions | — | 5–7 | ★★★ |

## 4. Introduction to Proof Systems (Q5–Q6) — Topic Frequency

| Topic | Historical Years | 2025 Presence | Marks if hit | Priority |
|---|---|---|---|---|
| Natural deduction, propositional (prove ⊢ φ) | **every IFP year + 2025** | Q5(c) — 4 marks | 4–6 | ★★★★★ |
| Natural deduction, predicate / quantifier proofs | every IFP ≤2021 | — | 4–6 | ★★★★ |
| Sequent calculus (LK) proofs | — | **Q6(b) — 4 marks** | 4 | ★★★★★ |
| Resolution: definition of resolvent, propositional refutations | — | **Q6(a) — 3 marks** | 1–3 | ★★★★★ |
| Ground resolution + Herbrand's theorem (state & prove) | — | **Q5(a,b) — 7 marks** | 3–4 | ★★★★★ |
| Horn formulas (closure, minimal models, satisfiability) | — | **Q6(c) — 7 marks** | 2–7 | ★★★★ |
| Compactness theorem + infinite combinatorics application | — | **Q6(d) — 6 marks** | 1–6 | ★★★★ |
| FO semantics: structures, models, expressibility, spectra | 2022, 2023, 2025 | Q5(d) — 9 marks | 2–5 | ★★★★★ |
| Truth tables, DNF/CNF | 2022, 2023 | — | 2–5 | ★★★★ |
| Structural induction on formulas | 2022, 2023 | — | 2–4 | ★★★★ |
| Substitution, free-for, negation normal form | 2023 | — | 2–4 | ★★★ |
| RAA / LEM, De Morgan proofs, derived rules, new connectives | 2013–2021 (recurring) | — | 2–6 | ★★★ |

## 5. Deprioritised Topics (Old Syllabus, Removed in Reform)

| Topic | Historical Hits | 2025 Presence | Priority |
|---|---|---|---|
| OOP inheritance / polymorphism / patterns / generics | 53/20/18 | **absent** | ★ |
| GUI / MVC / Observer / Memento / design patterns | ~15 | **absent** | ★ |
| Mixins / variance / subtype relationships | ~10 | **absent** | ★ |
| Iterators / cursors over mutable collections | ~5 | **absent** | ★ |

---

## 6. Template Index (CS2_2026_Templates/)

| File | Topic | Priority |
|---|---|---|
| `00_CS2_Exam_Overview_and_Topic_Frequency.md` | This overview | — |
| `IP_01_Loop_Invariants_Variants_Correctness.md` | Loop invariants, variants, correctness | ★★★★★ |
| `IP_02_Testing.md` | Equivalence partitioning, unit tests, boundaries | ★★★★★ |
| `IP_03_Arrays_Search_Sort_Complexity.md` | Array algos, binary search, complexity | ★★★★★ |
| `IP_04_ADT_Specification_and_Implementation.md` | ADT state, DTI, Abs, factories, FP pitfalls | ★★★★★ |
| `IP_05_Linked_Lists_Trees_Recursion.md` | Lists, trees, recursion, structural induction | ★★★★ |
| `IP_06_Hashing.md` | Hash properties, chained tables, structural hashing | ★★★★ |
| `IPS_01_Propositional_Logic_NormalForms_Substitution.md` | Truth tables, DNF/CNF, substitution | ★★★★ |
| `IPS_02_Structural_Induction.md` | Structural induction on formulas | ★★★★ |
| `IPS_03_Natural_Deduction.md` | ND proofs, strategy, freshness | ★★★★★ |
| `IPS_04_Sequent_Calculus.md` | Sequent calculus (LK) proofs | ★★★★★ |
| `IPS_05_Resolution_Herbrand.md` | Resolution, ground resolution, Herbrand | ★★★★★ |
| `IPS_06_Horn_Formulas_Compactness.md` | Horn formulas, compactness, infinite paths | ★★★★ |
| `IPS_07_FO_Semantics_Spectra_Models.md` | FO semantics, spectra, finite/infinite models | ★★★★★ |

---

## 7. Predicted Hot Topics for 2026

Following the 2025 examiners' signals:
- **Q1 (compulsory):** invariant + variant + testing on a given array-scan function (same format as 2025 Q1).
- **Q3 (compulsory):** specify-then-implement an ADT with mutable.Map (same format as 2025 Q3).
- **Q5 (compulsory):** state-and-prove Herbrand + ground resolution + ND proof + FO spectra (same format as 2025 Q5).
- **Q2 (optional):** harder array algorithm with a binary/ternary search twist on a perturbed-sorted array.
- **Q4 (optional):** hashing scenario (tree hashing + subtree search + collision analysis).
- **Q6 (optional):** resolution theory / sequent / Horn / compactness (the 2025 Q6 format).
