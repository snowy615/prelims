# DAA #8 — Sorting, Comparison Lower Bounds, Selection

**Frequency: 9/16 real papers. An Ω(n log n) lower-bound proof has closed Q6 in 2023, 2024 AND 2025 — the reduction-from-distinctness argument is now a fixed slot.**
Appearances: 2010 Q8(a)(b), 2013 Q6, 2014 Q7(d), 2018 Q5(d), 2022 FP Q2 (complexity), 2023 Q6(b)/Q8(c)(d), 2024 Q8(c)(d), 2025 Q6(d); also 2026 mock Q6(d).

---

## How it is asked

1. **Describe + trace a sort (4–6 marks):** counting sort step-by-step (2010); mergesort + Master Theorem (2010, 2013, 2018).
2. **Lower bound (4–5 marks):** prove comparison sorting needs Ω(n log n); adapt the proof to a variant (duplicates 2024, distinctness/intervals 2023, duplicate elimination 2026).
3. **Selection (4–7 marks):** find the kth smallest / i smallest — compare sort-based, heap-based and selection-based approaches with tight bounds; order statistics that are multiples of k (2023 Q8d).
4. **Comparison counting (4–8 marks):** binary vs ternary comparisons, membership in log₂n + O(1) (2014), weighted median in O(n) (2018).

---

## Memorise verbatim

**Mergesort:** split in half, sort recursively, merge in O(n). `T(n)=2T(n/2)+O(n)=O(n log n)` (Master Theorem, d=1=log₂2).

**Lower bound (decision tree):** a comparison-based sorting algorithm is a binary decision tree; it must distinguish all n! input orders, so it has ≥ n! leaves; a binary tree of height h has ≤ 2^h leaves, hence `h ≥ log₂(n!) = Ω(n log n)` (using log n! ∈ Ω(n log n), e.g. n! ≥ (n/2)^{n/2}).

**Variant with k copies of each element (2024):** number of distinguishable inputs is `n!/(k!)^{n/k}`; `log` of it is `Ω(n log(n/k))`; matching algorithm: O(n log(n/k)) — e.g. mergesort with run detection, or recurse but stop when subproblems have ≤ k equal elements.

**Reduction lower bounds (2023 Q6b, 2025 Q6d, 2026 mock Q6d):** to lower-bound problem P, reduce a known-hard problem to it in o(n log n): *"If P could be solved in o(n log n) comparisons, then distinctness could too (run P, then check …), contradicting the Ω(n log n) bound for distinctness."* — Intervals non-intersecting ⟸ distinctness; duplicate elimination ⟹ distinctness (eliminate duplicates, compare sizes); undominated points ⟸ distinctness (2025: map each number a to the point (a, −a); all points are undominated iff all aᵢ distinct — compute the undominated set, compare its size to n).

**Counting sort:** for keys in {0..k}: count occurrences, prefix-sum the counts, place elements stably. **O(n + k)**, not comparison-based — hence no contradiction with the lower bound (say this).

**Worst cases:** counting sort O(n+k); heapsort O(n log n); quicksort **O(n²)** worst, O(n log n) expected; mergesort O(n log n).

**Selection (kth smallest):**
- Sort then index: O(n log n).
- Min-heap + k extractions: O(n + k log n).
- Max-heap of size k: O(n log k) (see DAA_06).
- Quickselect / median-of-medians: **O(n)** worst case (quote it; details rarely required).
- "i smallest in order" three-way comparison (2013): sort O(n log n); heap O(n + i log n); select+partition+sort O(n + i log i). Know which wins for small i.

---

## Rehearsed answers

- **Counting sort trace (A=[2,7,3,2,8,1]):** counts → cumulative → stable placement; show all three arrays.
- **Order statistics at multiples of k (2023 Q8):** repeated select-and-discard gives Σ O(n−ik) = O(n²/k); better: select the median (n/2-th), recurse on both halves until blocks of size k — `T(n)=2T(n/2)+O(n)` stopping at depth log(n/k) → **O(n log(n/k))**.
- **Two elements summing to x in sorted S (2014):** two pointers from both ends, move left if sum < x else right — O(n); invariant: any pair outside the pointers is already excluded.
- **A[j] = 2A[i] (2018):** sort, then for each i binary-search 2A[i] — O(n log n); or two-pointer scan.
- **Weighted median (2018):** sort by value, prefix-sum the weights, find the crossing point O(n log n); O(n) via quickselect-style partitioning on weights — recurse on the side whose weight exceeds 1/2, carrying the discarded weight.
- **Membership in log₂n + O(1) ternary comparisons (2014):** binary search; each ternary (<,=,>) probe halves the range and can terminate early.
- **Unbounded search for unknown m (2014, 2024 Q8b):** probe at growing positions (powers: 1,2,4,8,… or (1+ε)-powers), overshoot, then binary search inside the last interval — Θ(log k)/(1+ε)·log m + O(1) comparisons.

---

## Traps

- The lower bound applies only to **comparison-based** algorithms — counting sort is the standard "loophole" follow-up.
- log n! = Θ(n log n): justify with n! ≥ (n/2)^{n/2} or Stirling; questions often give you the fact — then cite it, don't reprove.
- In reduction arguments the direction matters: reduce the *known hard* problem TO the new one.
- "Stable" matters for counting sort and for 2022 FP Q2 (stable mergesort) — define it if you use it: equal elements keep their input order.
