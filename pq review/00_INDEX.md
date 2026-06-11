# CS1 (FP + DAA) Past-Paper Analysis & Exam Templates — INDEX

Built from all real past-paper questions, **2010–2025 (16 papers)**. The "Trinity 2026" PDF in this folder is a **self-made mock** (closely modelled on the real 2025 paper) — cited only as practice material, excluded from frequency counts.

**Format of the current exam (confirmed by the real 2025 paper):** one 3-hour paper; FP = Questions 1–3, DAA = Questions 4–6; **Q1 and Q4 are compulsory**; answer 5 of 6 in total. Q1 = unfoldr drills + fold-expressibility + a 10-mark induction proof + 2-mark complexity. Q4 = three recurrences + a shortest-path trace with a twist + an 8-mark DP design with an explicit marking checklist. Papers 2010–2020 used the old format (FP Q1–Q4, DAA Q5–Q8, no compulsory questions).

---

## Ranking — Functional Programming (out of 16 real papers)

| Rank | File | Topic | Frequency | Notes |
|---|---|---|---|---|
| 1 | FP_01 | fold/foldr/foldl, fusion, "expressible as a fold?" | **16/16** | Core of compulsory Q1 (2025: takeWhile/dropWhile as folds) |
| 2 | FP_02 | Structural induction, equational proofs, take lemma | **16/16** | The 10-mark proof in Q1 (2025: map(foldl f x)·inits = scanl f x) |
| 3 | FP_04 | Program-design question (tables, games, matrices, IO) | 14/16 | Q3 every recent year (2025: determinant/Laplace) |
| 4 | FP_03 | Trees, algebraic datatypes & their folds | 12/16 | Q2 every recent year (2025: red-black tree invariants) |
| 5 | FP_05 | Lazy evaluation, infinite lists/trees | 11/16 | Now appears as "build an infinite tree/matrix" (2025 Q2c/Q3e) |
| 6 | FP_06 | unfold / unfoldr | 7/16 | Opens compulsory Q1 in BOTH 2024 and 2025 — learn cold |
| 7 | FP_07 | Type inference, classes, data/newtype/type/deriving | 9/16 | 2025 Q2(a): explain `deriving` |
| 8 | FP_08 | Functions as data (sets/sequences as functions) | 3/16 | Full 20-mark question when it appears (2010/2011/2023) |

## Ranking — Design & Analysis of Algorithms (out of 16 real papers)

| Rank | File | Topic | Frequency | Notes |
|---|---|---|---|---|
| 1 | DAA_01 | Dynamic programming | **15/16** | 8-mark design in compulsory Q4 (2025: shortest common supersequence) |
| 2 | DAA_02 | Divide & conquer, recurrences, Master Theorem | **15/16** | Recurrence drill opens compulsory Q4 every year since 2021 |
| 3 | DAA_03 | DFS, SCCs, topological sort, BFS | 13/16 | DFS trace + one SCC reduction every year 2021–2025 |
| 4 | DAA_04 | MST: Kruskal, Prim, Cut Lemma | 10/16 | Trace + variant design every year 2022–2025 |
| 5 | DAA_05 | Shortest paths: Dijkstra, Bellman–Ford, DAGs | 9/16 | Trace in compulsory Q4 in 2023, 2024(Q6), 2025 |
| 6 | DAA_08 | Sorting, comparison lower bounds, selection | 9/16 | Ω(n log n) reduction proof in 2023, 2024, 2025 — now standard |
| 7 | DAA_07 | Greedy (counterexamples + exchange proofs) | 8/16 | Every year 2021–2025; activity selection twice (2025 + mock) |
| 8 | DAA_06 | Heaps & priority queues | 8/16 | Absent 2023–2025 — lower priority now, but cheap to revise |

---

## Coverage map (every question → template)

| Year | FP | DAA |
|---|---|---|
| 2010 | FP_08; FP_01; FP_04 (interpreter); FP_04 (table) | DAA_04; DAA_02; DAA_01; DAA_08+DAA_03 |
| 2011 | FP_01+FP_06; FP_08; FP_07+FP_04; FP_04 | DAA_02; DAA_01; DAA_06+DAA_05; DAA_03 |
| 2012 | FP_01; FP_03+FP_02; FP_05; FP_04 | DAA_05; DAA_04; DAA_01; DAA_02 |
| 2013 | FP_07+FP_05; FP_01+FP_02; FP_04; FP_03 (BST) | DAA_01 (closure); DAA_02+DAA_08; DAA_01; DAA_03 |
| 2014 | FP_03 (Natural); FP_04 (relations); FP_06+FP_03; FP_01+FP_02 | DAA_02+DAA_08; DAA_01; DAA_06; DAA_04 |
| 2015 | FP_01+FP_07; FP_04 (Polish); FP_04; FP_05 | DAA_02; DAA_01; DAA_03; DAA_05 |
| 2016 | FP_01+FP_02; FP_03; FP_03 (Zig/Zag); FP_04 (matrices) | DAA_02; DAA_01+DAA_03; DAA_06; DAA_07 |
| 2017 | FP_07+FP_05; FP_05 (hamming); FP_04 (Sudoku); FP_01 (scanl)+FP_02 | DAA_02; DAA_03; DAA_04; DAA_01 |
| 2018 | FP_03+FP_01; FP_05 (series); FP_03 (CatList); FP_04 | DAA_02+DAA_08; DAA_01+DAA_07; DAA_03; DAA_04 |
| 2019 | FP_01+FP_02; FP_03+FP_06; FP_04 (game); FP_05 | DAA_02; DAA_06; DAA_01; DAA_05+DAA_01 |
| 2020 | FP_01+FP_02; FP_02 (take lemma); — | (Q3) DAA_01; (Q4) DAA_03 |
| 2021 | FP_01; FP_03+FP_06; FP_04 (bells); FP_07+FP_03 | DAA_02+DAA_06+DAA_07; DAA_02 (peaks); DAA_03; DAA_01 |
| 2022 | FP_01; FP_04 (runs/sort); FP_03 (search); FP_05 (numerics) | DAA_02+DAA_06; DAA_03; DAA_05; DAA_04 |
| 2023 | FP_01+FP_02; FP_08; FP_04 (Wordle); FP_03 (BST) | DAA_05; DAA_03+DAA_08+DAA_01; DAA_04+DAA_07; DAA_02+DAA_08 |
| 2024 | FP_06+FP_02; FP_04 (election); FP_04 (matrices); FP_07+FP_03 | DAA_04; DAA_05+DAA_07+DAA_01; DAA_02+DAA_03+DAA_07; DAA_02+DAA_08 |
| **2025** | Q1: FP_06+FP_01+FP_02; Q2: FP_07+FP_03+FP_05; Q3: FP_04+FP_01+FP_05 | Q4: DAA_02+DAA_05+DAA_01; Q5: DAA_07+DAA_04; Q6: DAA_03+DAA_02+DAA_08 |
| *2026 mock* | *same shape as 2025* | *same shape as 2025* |

(FP 2020 Q3/Q4 in your folder are actually DAA content and are counted as such.)

**The 2025 paper maps question-for-question onto the 2026 mock** — the new format is highly templated: Q1 (unfoldr → fold-expressibility → big proof → complexity), Q2 (datatype keyword → three linear-time invariant checkers → infinite structure), Q3 (recursive program → fold-expressibility → bigger program → infinite structure), Q4 (recurrences → trace+twist → DP design), Q5 (greedy fail → greedy variant → MST trace → MST variant design), Q6 (DFS trace → SCC reduction → D&C design → lower bound). Revise to this template.

---

## Suggested priority order for final revision

1. **DAA_02 recurrence drill + DAA_01 DP template** — compulsory Q4 is exactly these plus a trace.
2. **FP_01 + FP_02 + FP_06** — compulsory Q1 is unfoldr + fold-expressibility + one 10-mark induction proof (with auxiliary lemma!).
3. **DAA_03 + DAA_05 traces** — DFS d/f times, Dijkstra/Bellman–Ford tables; mechanical marks, practise to speed.
4. **FP_03 invariant checkers** — "three Tree → Bool functions, linear time" appeared in 2025 AND the mock; master the richer-return-value pattern.
5. **DAA_04 Cut Lemma + MST variant designs** (e-in-some-MST 2025, {1,2}-weights mock).
6. **DAA_07 greedy + DAA_08 lower-bound reductions** — both in Q5/Q6 every recent year.
7. **FP_04 program design** — one timed practice (2025 Q3 determinant is the latest style).
8. **FP_05, FP_07, FP_08, DAA_06** — fill remaining gaps.

Each template file contains: how the topic is asked (with marks), statements to memorise verbatim, a step-by-step answer template, worked standard answers, and the traps that lose marks.
