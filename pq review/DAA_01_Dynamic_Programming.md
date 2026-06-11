# DAA #1 — Dynamic Programming

**Frequency: 15/16 real papers (2010–2025). The single most reliable DAA topic — the 8-mark design part of compulsory Q4.**
Appearances: 2010 Q7, 2011 Q6, 2012 Q7, 2013 Q7, 2014 Q6, 2015 Q6, 2016 Q6, 2017 Q8, 2018 Q6, 2019 Q7/Q8, 2020 Q3, 2021 Q8, 2023 Q6(c), 2024 Q6(c), 2025 Q4(c); also 2026 mock Q4(c).

---

## How it is asked

1. **Definition warm-up (2–4 marks):** "Explain Dynamic Programming and the Principle of Optimality." / "Compare DP with Divide-and-Conquer."
2. **Standard problem (4–8 marks):** knapsack (with/without repetition), coin change / postage, edit distance / sequence alignment, all-pairs shortest paths.
3. **Novel problem (6–10 marks):** a story problem you must model yourself (min jumps 2014, string breaking 2012, 2-split 2021, concatenation 2023, longest valid subsequence 2024, longest palindromic subsequence 2026).
4. **Extension (3–7 marks):** modify your DP (two knapsacks, at most k coins, reconstruct the actual solution).

Since 2021 the rubric literally tells you the marking scheme: *define the quantity, write the recurrence, turn it into an algorithm, analyse running time.* Follow that order exactly.

---

## Memorise verbatim

**Dynamic Programming** solves a problem by identifying a collection of overlapping subproblems, solving each subproblem only once (bottom-up in a table, or top-down with memoisation), and combining stored answers; it applies when the problem has *optimal substructure* and *overlapping subproblems*.

**Principle of Optimality:** an optimal solution to a problem contains within it optimal solutions to its subproblems (any sub-solution of an optimal solution is itself optimal for the corresponding subproblem).

**DP vs Divide-and-Conquer:** D&C splits into *independent* subproblems and recurses; DP subproblems *overlap*, so naive recursion repeats work exponentially; DP stores answers in a table. D&C subproblems shrink by a constant factor; DP subproblems often shrink by 1 and are indexed by parameters.

---

## Answer template (use for every DP design question)

1. **Quantity (state).** "Let `OPT(i, j)` = the maximum/minimum … using only the first `i` items / the prefix `x[1..i]` / capacity `j`."
   - Choose indices = what the recursion needs to forget the past. Prefix index `i` almost always; add capacity/budget `j` if there is a resource; add a second sequence index for two-string problems; add "last element chosen" if validity depends on adjacency (2024 GCD subsequence).
2. **Recurrence.** Case split on the *last decision*:
   `OPT(i,j) = best( option not using item i , option using item i )`, e.g. knapsack without repetition:
   `K(i,j) = max( K(i-1,j), v_i + K(i-1, j - w_i) )` for `w_i ≤ j`.
   **Justify in one sentence:** "Either the optimal solution uses item i or it does not; in each case the rest must be optimal for the smaller subproblem (Principle of Optimality)."
3. **Base cases.** `OPT(0,j) = 0`, `OPT(i,0) = 0` (or ∞ for min problems with infeasibility — state which).
4. **Order of evaluation.** "Fill the table for increasing i (and j); each entry depends only on smaller indices." Mention table dimensions.
5. **Answer location.** "The answer is `OPT(n, W)`" (or `max over i` of `OPT(i)` — check this! For 'longest subsequence ending at i' states the answer is a max over all i).
6. **Running time.** (#states) × (time per state). State both factors: "O(nW) entries, O(1) each → O(nW)."
7. **Reconstruction (if asked).** "Keep a parallel array `choice(i,j)` recording which case achieved the optimum, then trace back from `OPT(n,W)`." Or recompute the argmax while walking back — no extra space.

---

## Standard recurrences to know cold

| Problem | State | Recurrence |
|---|---|---|
| Knapsack, no repetition | `K(i,j)`: items 1..i, cap j | `max(K(i-1,j), v_i + K(i-1,j-w_i))` |
| Knapsack, repetition | `K(j)`: cap j | `max over i with w_i≤j of v_i + K(j-w_i)` |
| Coin change (min coins) | `C(j)` | `1 + min over i, x_i≤j of C(j-x_i)`; `C(0)=0` |
| Subset sum / partition | `t(i,j)` boolean | `t(i-1,j) ∨ t(i-1, j-a_i)` |
| Edit distance | `E(i,j)` | `min(E(i-1,j)+1, E(i,j-1)+1, E(i-1,j-1)+diff(x_i,y_j))` |
| Alignment (max score) | `S(i,j)` | `max(S(i-1,j-1)+match, S(i-1,j)-g, S(i,j-1)-g)` |
| LIS-style ("ending at i") | `L(i)` | `1 + max over j<i, compatible(j,i) of L(j)`; answer `max_i L(i)` |
| Longest palindromic subsequence | `P(i,j)` on substring | `x_i=x_j: 2+P(i+1,j-1)`; else `max(P(i+1,j), P(i,j-1))` |
| Shortest common supersequence (2025) | `S(i,j)`: prefixes x[1..i], y[1..j] | `x_i=y_j: 1+S(i-1,j-1)`; else `1+min(S(i-1,j), S(i,j-1))`; base `S(i,0)=i, S(0,j)=j`; O(nm); equivalently n+m−LCS |
| Interval DP (string breaking 2012, matrix-chain) | `C(i,j)` | `min over k in (i,j) of C(i,k)+C(k,j)+cost(i,j)` — O(n³) |
| Floyd–Warshall APSP | `d_k(i,j)` | `min(d_{k-1}(i,j), d_{k-1}(i,k)+d_{k-1}(k,j))` — O(n³) |
| Bitmask DP (Hamiltonian path 2016) | `H(S,v)` | `∨ over u∈S\{v}, (u,v)∈E of H(S\{v}, u)` — O(2ⁿ n²) |

---

## Traps & marker's checklist

- **Always justify the recurrence** (one Principle-of-Optimality sentence per case). Recurrence without justification loses marks on "explain why correct".
- **Pseudo-polynomial honesty:** O(nW) is *not* polynomial in input size; say so if asked to "discuss".
- For "ending at i" formulations, the final answer is `max_i`, not `OPT(n)`.
- Min-problems: initialise infeasible states to ∞ and say how ∞ propagates; output "no solution" if the answer stays ∞ (2023 Q6c explicitly tests this).
- If greedy is offered first ("show greedy fails"), give the smallest counterexample you can, *compute both values* (greedy result vs optimal result) explicitly.
- Reconstruction costs no asymptotic extra time — say it.
- When asked to *show the working* on a small input, draw the table and fill it; never skip this, it is free marks.
