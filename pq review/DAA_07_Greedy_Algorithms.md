# DAA #7 — Greedy Algorithms (incl. "show greedy fails")

**Frequency: 8/16 real papers and RISING — every year 2021–2025; Q5(a)(b) is now a fixed greedy slot (activity selection in 2025 and the mock).**
Appearances: 2011 Q6(a), 2016 Q8, 2018 Q6(c-iii), 2019 Q6(d), 2021 Q5(c), 2023 Q7(c)(d), 2024 Q6(b)/Q7(c), 2025 Q5(a)(b); also 2026 mock Q5(a)(b).

---

## How it is asked

1. **"Show this greedy fails" (3–5 marks):** given a plausible greedy rule, produce a counterexample. (Coin change 2011; pick-most-popular-time 2016; longest-increasing-subsequence removal 2023; left-to-right GCD 2024; earliest-start activity selection 2026.)
2. **"Give a greedy algorithm" (3–6 marks):** for a problem where greedy *does* work (earliest finish time; interval partitioning into rooms 2026; edge cover on trees 2024; coin change with denominations cⁱ 2018).
3. **"Prove the greedy is optimal" (4–6 marks):** exchange argument or "greedy stays ahead" (2016 Q8d is a full model answer pattern).
4. **Concept (2–5 marks):** "What is a greedy algorithm? Why is Kruskal greedy?"

---

## Memorise verbatim

**Greedy algorithm:** builds a solution step by step, at each step making the choice that looks best by a local criterion, never revisiting earlier choices. It is correct only when the problem has the *greedy-choice property* (some optimal solution contains the greedy first choice) and *optimal substructure*. Kruskal is greedy: it always adds the cheapest edge that keeps the solution a forest, and never removes an added edge; the Cut Lemma proves each choice safe.

---

## Template A — building a counterexample (do this in 2 minutes)

1. Identify *what the greedy ignores* (future conflicts, larger coins blocking exact change, order constraints).
2. Construct the smallest instance exploiting it — usually 3–5 elements.
3. **Compute both values explicitly:** "Greedy picks …, giving k = 3; but … achieves k = 2. Hence greedy is not optimal."

Stock counterexamples:
- **Coin change, largest-coin-first:** denominations {1, 3, 4}, target 6: greedy 4+1+1 (3 coins), optimal 3+3 (2 coins).
- **Activity selection by earliest start (2026 mock):** one long activity starting first blocks two disjoint short ones: (0,10), (1,2), (3,4).
- **Activity selection by shortest duration (2025):** a short activity straddling two long disjoint ones: (1,10), (9,12), (11,20) — greedy takes (9,12) (length 3) and blocks both others; optimal is the two long ones.
- **Most-popular-meeting-time (2016):** arrange intervals so the most-covered point t splits the rest badly; 5 people: [0,1],[0,1],[2,3],[2,3],[1,2] — picking t∈[1,2] covers only one person... construct so greedy needs 3 meetings, optimal 2.
- **Greedy LIS-removal (2023):** sequence where one choice of longest increasing subsequence destroys the partition: e.g. S = (1,3,5,2,4,6) — removing (1,3,5,6)-type LIS leaves more pieces than removing (1,2,4,6)... pick the LIS that interleaves.
- **Left-to-right add-if-valid (2024):** make the first element compatible-poor: 35, 21, 22, 10, 33 — taking 35 early blocks the longer tail.

## Template B — proving greedy correct (exchange argument)

> Let G = g₁,…,g_k be greedy's solution and O = o₁,…,o_m an optimal one, both sorted. We show G is optimal by proving m = k.
> **Step 1 (greedy stays ahead):** by induction on i: g_i finishes no later than o_i (greedy chose the earliest finishing / cheapest feasible option among those compatible with g₁..g_{i−1}, and o_i is one such option).
> **Step 2:** if m < k, then o_m covers everything after g_m's choice — but greedy only continued because something was uncovered after g₁..g_m, and since g_m ≤ o_m that thing is also uncovered by O — contradiction. ∎

(2016 Q8d hint walks exactly this structure: assume u₁<…<u_j with j<k, prove u_i ≤ t_i, derive contradiction.)

## Template C — greedy algorithms that work (know these)

- **Activity selection (max #disjoint):** sort by finish time; take each activity compatible with the last taken. O(n log n).
- **Interval partitioning / rooms (2026 mock Q5b):** sort by start time; assign each activity to any free room, opening a new room if none free (min-heap of room finish times). #rooms = max overlap (depth). O(n log n).
- **Two identical rooms, maximise #activities (2025 Q5b):** sort by finish time; for each activity, schedule it in a free room if one exists; *if both rooms are free, use the one that became free LATER* (keeps the earlier-free room available for tighter activities — this is the hinted subtlety). Trace the given example showing room assignments.
- **Meeting-times version (2016):** pick t = earliest end time e_i; remove all who can attend; repeat.
- **Coin change with v_i = cⁱ (2018):** largest coin first is optimal — prove: fewer than c of each smaller coin is ever needed (c smaller coins exchange for one bigger), so the greedy count matches base-c representation.
- **Edge cover on a tree (2024):** repeatedly take an edge incident to a leaf, remove covered vertices.
- **Huffman-style min-cost addition (2021 Q5c):** repeatedly add the two smallest numbers (min-heap), like Huffman merging; cost analysis O(n log n); correctness: exchange argument on the deepest leaves.

---

## Traps

- A counterexample must show greedy's output is **strictly worse** — compute both numbers.
- "Describe in words" ≠ skip precision: state the sorting key and the feasibility test.
- When the question pairs greedy with DP ("show greedy fails, then design DP"), reuse the counterexample's structure to motivate the DP state.
- In proofs, never say "greedy is obviously optimal"; the marks are in the induction/exchange.
