# DAA #2 — Divide & Conquer, Recurrences and the Master Theorem

**Frequency: 15/16 real papers. Since 2021, "solve these three recurrences" appears every single year, and since 2023 it OPENS the compulsory Q4 (2025 Q4a). Guaranteed marks — make them automatic. A D&C design also appears in Q6(c) (2025: undominated points).**
Appearances: 2010 Q6, 2011 Q5, 2012 Q8, 2013 Q5/Q6, 2014 Q5, 2015 Q5, 2016 Q5, 2017 Q5, 2018 Q5, 2019 Q5, 2021 Q5/Q6, 2022 Q5, 2023 Q8, 2024 Q7/Q8, 2025 Q4(a)/Q6(c); also 2026 mock Q4(a)/Q6(c).

---

## How it is asked

1. **Recurrence drill (6 marks, compulsory):** solve 3 recurrences in Θ-notation "with short rigorous justification (e.g. Master Theorem with constants …)". Always one Master-Theorem case, plus one trick recurrence (`T(n)=T(n-1)+f(n)`, `T(n)=2T(√n)+1`, or `T(n)=T(n/2)+T(n/4)+n`).
2. **Define / state (2–5 marks):** "What is a divide-and-conquer algorithm?" "State the Master Theorem." Occasionally *prove* a case of it (2016 Q5c).
3. **Known algorithms:** Karatsuba O(n^log₂3) (2011, 2017, 2024), Strassen O(n^log₂7) (2011, 2023), mergesort + analysis (2010, 2013, 2018, 2021 FP Q2), binary search and variants (2014, 2018, 2019, 2024).
4. **Design a new D&C algorithm (6–10 marks):** local maximum / peak finding in O(log n) (2016, 2021), maximum of circularly shifted array (2017), majority element (2012), min+max in 3n/2 comparisons (2016), three largest (2015), kth-largest in two sorted arrays (2019), longest path in binary tree (2024), count pairs x_i > 2x_j (2026).

---

## Memorise verbatim

**Divide and conquer:** split the problem into a number of *independent* subproblems of smaller size, solve them recursively, and combine their solutions; the recursion bottoms out at constant-size cases solved directly.

**Master Theorem.** If `T(n) = a·T(⌈n/b⌉) + O(n^d)` with `a > 0, b > 1, d ≥ 0`, then

- `T(n) = O(n^d)` if `d > log_b a`
- `T(n) = O(n^d log n)` if `d = log_b a`
- `T(n) = O(n^{log_b a})` if `d < log_b a`

Intuition (useful for the proof question): level i of the recursion tree has `a^i` subproblems of size `n/b^i`, costing `a^i (n/b^i)^d = n^d (a/b^d)^i`; the series is geometric — dominated by the root (case 1), equal levels summing to log n terms (case 2), or the leaves `n^{log_b a}` (case 3).

---

## Recurrence drill — worked patterns

| Recurrence | Answer | Justification |
|---|---|---|
| `T(n)=8T(n/2)+1` | Θ(n³) | a=8,b=2,d=0; d<log₂8=3 |
| `T(n)=4T(n/2)+n²` | Θ(n² log n) | d=2=log₂4 |
| `T(n)=7T(n/3)+n²` | Θ(n²) | d=2>log₃7≈1.77 |
| `T(n)=9T(n/3)+n` | Θ(n²) | d=1<log₃9=2 |
| `T(n)=6T(n/3)+n²` | Θ(n²) | d=2>log₃6≈1.63 |
| `T(n)=T(2n/3)+1` | Θ(log n) | a=1,b=3/2,d=0; d=log 1 |
| `T(n)=T(n-1)+n²` (or 2^n, n) | Θ(n³) (Θ(2ⁿ), Θ(n²)) | unfold the sum: Σ f(i) |
| `T(n)=2T(√n)+1` / `3T(n^{1/3})+log n` | Θ(log n) / Θ(log n·loglog n)-style | substitute `m=log n`, get `S(m)=2S(m/2)+1=Θ(m)` |
| `T(n)=T(n/2)+T(n/4)+n` (2024, 2026) | Θ(n) | costs shrink geometrically: n(1/2+1/4)<n; root dominates (verify by induction T(n)≤cn) |
| `T(n)=3T(n/2+1)+n` | Θ(n^log₂3) | the +1 does not change the answer; say "absorb into ⌈·⌉" |
| `T(n)=2T(n-1)+c` | Θ(2ⁿ) | unfolds to geometric series |
| `T(n)=T(7n/9)+n` (2025) | Θ(n) | a=1, b=9/7, d=1; d>log 1=0 |
| `T(n)=T(n−√n)+1` (2025) | Θ(√n) | halving n takes ≤ √n/... steps: each step in [n/2,n] subtracts ≥ √(n/2), so ≤ √2·√n steps per halving phase; Σ over phases √(n/2ᵏ) is geometric → Θ(√n) |

**Justification format that earns the marks:** "Master Theorem with a=__, b=__, d=__; since d ⋛ log_b a, T(n)=Θ(__)." For non-Master ones: unfold/substitute, then one-line induction check.

---

## Design-question template

1. **Divide:** say exactly how (halves, around a probe index, around a pivot).
2. **Conquer:** what the recursive call returns (be precise about the *returned information* — often you must strengthen it, e.g. return (min,max), or (max, second-set) (2015), or (height, longest path) for trees (2024)).
3. **Combine:** how answers merge, and its cost.
4. **Recurrence + Master Theorem** for the running time.
5. **Correctness:** one invariant sentence, e.g. for peak finding: "if `A[mid] < A[mid+1]` then the right half contains a local maximum (the array rises and must eventually fall or hit the boundary)."

**Classic combine trick (count inversions / pairs x_i > 2x_j, 2026 mock Q6c):** sort-and-count à la mergesort; while merging the two sorted halves count cross pairs with a two-pointer scan in O(n); `T(n)=2T(n/2)+O(n)=O(n log n)`.

**Undominated points / maxima (2025 Q6c):** sort by x once (O(n log n)), or split by median x: recursively find maxima of the right half; a left-half point survives iff its y exceeds the max y of the right half; combine O(n) → `T(n)=2T(n/2)+O(n)=O(n log n)`. (Equivalent linear scan from largest x keeping running max y — mention the D&C version since that's what's asked.)

---

## Traps

- State the Master Theorem *before* using it when the question says so — "(If you use the Master Theorem, you should state a version precisely)" appears repeatedly.
- `T(n)=T(n-1)+…` is NOT Master Theorem (b=1). Unfold it.
- Karatsuba: 3 multiplications of n/2-digit numbers + O(n) additions → `T(n)=3T(n/2)+O(n)=O(n^log₂3)`. Strassen: `7T(n/2)+O(n²)=O(n^log₂7)`. The hypothetical "9 blocks of n/3" gives `9T(n/3)+O(n²)=O(n² log n)` — they love this comparison.
- Binary search variants: quaternary search saves comparisons only by a constant factor — still Θ(log n); exponential search (probe 1,2,4,8,… then binary search) gives Θ(log k) (2024 Q8b).
- For comparison-counting questions (min+max in ⌈3n/2⌉−2), count comparisons in the recurrence itself, not just asymptotics.
