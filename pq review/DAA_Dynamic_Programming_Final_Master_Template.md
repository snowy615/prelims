# DAA Dynamic Programming — Final Master Template

> **Use this as the single final version.** It merges the three uploaded notes into one exam-ready DP sheet: formal answer structure, Oxford-style wording, standard recurrences, reconstruction, complexity analysis, and marker traps.

---

## 0. What to Memorise Verbatim

### Dynamic Programming

Dynamic Programming solves a problem by identifying a collection of overlapping subproblems, solving each subproblem only once — either bottom-up in a table or top-down with memoisation — and combining stored answers to solve larger subproblems.

A problem is suitable for DP when it has:

1. **Optimal substructure**: an optimal solution is built from optimal solutions to smaller subproblems.
2. **Overlapping subproblems**: the same subproblems recur many times in a naive recursive solution.
3. **Natural subproblem parameters**: prefixes, capacities, intervals, subsets, tree nodes, or endpoints.
4. **A valid dependency order**: smaller subproblems can be solved before larger ones.

### Principle of Optimality

> The optimal solution to a problem contains optimal solutions to its subproblems.

Equivalent Oxford-style phrasing:

> The optimal solution to a problem is a function of optimal solutions to some of its subproblems.

### DP vs Divide-and-Conquer

| Aspect | Divide-and-Conquer | Dynamic Programming |
|---|---|---|
| Subproblems | Independent / disjoint | Overlapping |
| Split | Usually fixed, e.g. middle | Often explores many possible choices or split points |
| Repetition | No repeated subproblems | Naive recursion repeats subproblems |
| Storage | Usually no table needed | Stores subproblem answers |
| Direction | Often recursive top-down | Bottom-up table or top-down memoisation |
| Main use | Sorting, searching, multiplication | Optimisation and decision problems |
| Requirement | General decomposition | Principle of Optimality must hold |

Canonical 2-mark answer:

> Both techniques split a problem into smaller subproblems and combine their solutions. Divide-and-Conquer splits into independent subproblems, so recursion does not repeat work. Dynamic Programming is used when subproblems overlap and the Principle of Optimality holds; it stores subproblem answers in a table or memo table to avoid recomputation, usually at the cost of extra space.

---

## 1. Universal DP Exam Answer Template

When the question says **“Design a dynamic programming algorithm”**, write the answer in this order.

### Step 0 — Restate the Problem

Write one sentence saying what is being optimised or decided.

> We need to compute the maximum/minimum/boolean value for the original input under the given constraints.

This is not always worth many marks, but it prevents ambiguity.

---

### Step 1 — Define the Quantity / State

This is the most important step.

Write:

1. The name of the DP table.
2. The exact meaning of each entry in plain English.
3. The domain of all indices.
4. Where the final answer will be found.

Template:

```text
Let OPT(i, j) be [maximum/minimum/true iff ...] for [subproblem meaning],
where [index ranges].
The answer to the original problem is [OPT(n, W), OPT(1, n), max_i OPT(i), ...].
```

Common state patterns:

| Pattern | State | When to use |
|---|---|---|
| Prefix | `OPT(i)` | One sequence, decision up to position `i` |
| Two prefixes | `OPT(i, j)` | Two strings/sequences |
| Items + capacity | `OPT(i, w)` | 0/1 knapsack, subset sum, budgets |
| Capacity only | `OPT(w)` | Unbounded knapsack / coin change |
| Interval | `OPT(i, j)` | Palindromes, matrix-chain, string breaking |
| Tree node | `OPT(u)` or `OPT(u, state)` | Tree DP |
| Subset + endpoint | `OPT(S, v)` | Hamiltonian path / TSP-style bitmask DP |
| Ending at `i` | `OPT(i)` = best solution ending at `i` | LIS-style subsequence problems |

**Marker checklist:** the state must be self-contained. A reader should not need to guess what `i`, `j`, `w`, or `S` mean.

---

### Step 2 — Base Cases

State all smallest subproblems explicitly.

Common examples:

```text
OPT(0, j) = 0                  // no items / empty prefix
OPT(i, 0) = 0                  // zero capacity / empty string
OPT(0) = 0                     // starting position or empty amount
OPT(i, j) = ∞                  // impossible min-problem state
OPT({v}, v) = true             // singleton subset path
```

For minimisation problems, say how infeasible states are represented:

```text
Set impossible states to ∞. If the final answer remains ∞, no feasible solution exists.
```

---

### Step 3 — Recurrence

Case split on the **last decision** or **final structure** of the optimal solution.

Generic template:

```text
OPT(i, j) = best over:
  1. Do not use the last item / character / edge / split.
  2. Use it, then add the optimal solution of the remaining smaller subproblem.
```

Always include a one-sentence correctness justification:

> The cases are exhaustive. In each case, after fixing the last decision, the remaining part must be optimal for the corresponding smaller subproblem by the Principle of Optimality.

---

### Step 4 — Filling Direction / Algorithm

State the dependency order and, if useful, give loop structure.

Examples:

```text
Fill by increasing i, and for each i by increasing j.
Each entry depends only on entries from row i-1 or smaller j.
```

```text
Fill by increasing interval length ℓ = j - i.
for length = 0 to n-1:
    for i = 1 to n - length:
        j = i + length
        compute OPT(i, j)
```

```text
For bitmask DP, fill subsets S in increasing order of |S|.
```

**Do not write only “fill in the right order.”** Say what increases: item count, capacity, prefix length, interval length, tree postorder, or subset size.

---

### Step 5 — Answer Location

Be precise:

```text
The answer is OPT(n, W).
The answer is OPT(m, n).
The answer is max_i OPT(i).
The answer is max over all table entries.
The answer is OR_v H(V, v).
```

Common trap: if `OPT(i)` means “best solution ending at `i`”, then the answer is usually `max_i OPT(i)`, not `OPT(n)`.

---

### Step 6 — Complexity

Use the formula:

```text
Time = number of states × time per state.
Space = table size + auxiliary arrays.
```

Example:

```text
There are O(nW) states and each state is computed in O(1), so the time is O(nW).
The table has O(nW) entries, so the space is O(nW).
```

For pseudo-polynomial algorithms, be honest:

> `O(nW)` is pseudo-polynomial, not polynomial in the bit-length of `W`.

---

### Step 7 — Reconstruction, If Asked

Use a parallel choice/backpointer table.

```text
Maintain choice(i, j), recording which recurrence case achieved OPT(i, j).
After filling the table, start from the final answer cell and follow choice pointers backwards until reaching a base case.
The reversed sequence of choices gives one optimal solution.
```

If no choice table is stored, reconstruction can often be done by recomputing which recurrence branch matches the stored value while walking backwards.

---

## 2. The 10-Mark DP Answer Skeleton

For a full exam answer, write this exact structure:

1. **Problem restatement:** what is being optimised/decided.
2. **State definition:** `OPT(...)` with clear semantics and index ranges.
3. **Base cases:** all boundary/impossible cases.
4. **Recurrence:** case split with maths.
5. **Correctness justification:** one sentence per case, citing Principle of Optimality.
6. **Evaluation order:** increasing index / capacity / length / subset size / postorder.
7. **Answer location:** exact table entry or max/OR over entries.
8. **Complexity:** number of states × work per state; include space.
9. **Reconstruction:** choice/backpointer table if actual solution is requested.
10. **Edge cases:** impossible states, empty input, zero capacity, or local vs global objective.

---

## 3. Standard Recurrences to Know Cold

### 3.1 0/1 Knapsack

**Problem:** items `1..n`, value `v_i`, weight `w_i`, capacity `W`; each item can be used at most once.

**State:**

```text
K(i, w) = maximum value using only items 1..i with capacity w.
```

**Base cases:**

```text
K(0, w) = 0 for all w
K(i, 0) = 0 for all i
```

**Recurrence:**

```text
If w_i > w:
    K(i, w) = K(i-1, w)
Otherwise:
    K(i, w) = max(K(i-1, w), v_i + K(i-1, w - w_i))
```

**Justification:** either item `i` is excluded, or it is included and the remaining capacity is solved optimally using only earlier items.

**Answer:** `K(n, W)`.

**Complexity:** `O(nW)` time, `O(nW)` space; reducible to `O(W)` for value-only if capacities are iterated backwards.

**Trap:** because the item can be used once, the include case uses row `i-1`.

---

### 3.2 Unbounded Knapsack / Coin Change / Stamp Problem

**Problem:** unlimited copies of each denomination/item. Minimise number of coins/stamps or maximise value.

#### Min coins, 1D version

**State:**

```text
C(p) = minimum number of coins needed to make amount p.
```

**Base cases:**

```text
C(0) = 0
C(p) = ∞ if p < 0 or impossible
```

**Recurrence:**

```text
C(p) = 1 + min over denominations d_i ≤ p of C(p - d_i)
```

**Answer:** `C(p)`, or “no solution” if it remains `∞`.

**Complexity:** `O(np)` time, `O(p)` space.

#### Stamp-style 2D version

**State:**

```text
S(i, a) = minimum number of stamps to make amount a using denominations d_1..d_i.
```

**Recurrence:**

```text
If d_i > a:
    S(i, a) = S(i-1, a)
Otherwise:
    S(i, a) = min(S(i-1, a), 1 + S(i, a - d_i))
```

**Trap:** because copies are unlimited, the use case stays in row `i`, not `i-1`.

---

### 3.3 Subset Sum / Partition

**Problem:** decide whether a subset of `a_1..a_n` sums to target `T`. Partition asks whether target `T = total/2` is possible.

**State:**

```text
t(i, s) = true iff some subset of a_1..a_i has sum s.
```

**Base cases:**

```text
t(0, 0) = true
t(0, s) = false for s > 0
```

**Recurrence:**

```text
t(i, s) = t(i-1, s) OR t(i-1, s - a_i)   if s ≥ a_i
t(i, s) = t(i-1, s)                      if s < a_i
```

**Answer:** `t(n, T)`. For partition, first check that the total sum is even.

**Complexity:** `O(nT)` time and space.

---

### 3.4 Edit Distance

**State:**

```text
E(i, j) = minimum edit distance between prefixes x[1..i] and y[1..j].
```

**Base cases:**

```text
E(i, 0) = i
E(0, j) = j
```

**Recurrence:**

```text
E(i, j) = min(
    E(i-1, j) + 1,                    // delete x_i
    E(i, j-1) + 1,                    // insert y_j
    E(i-1, j-1) + diff(x_i, y_j)      // substitute or match
)
```

where `diff(a, b) = 0` if `a = b`, otherwise substitution cost.

**Answer:** `E(m, n)`.

**Complexity:** `O(mn)` time and space.

---

### 3.5 Sequence Alignment

**State:**

```text
S(i, j) = maximum score for aligning x[1..i] with y[1..j].
```

**Global alignment base cases:**

```text
S(i, 0) = -i·g
S(0, j) = -j·g
```

**Global recurrence:**

```text
S(i, j) = max(
    S(i-1, j-1) + score(x_i, y_j),
    S(i-1, j) - g,
    S(i, j-1) - g
)
```

**Answer:** `S(m, n)`.

**Local alignment difference:** add `0` as an option and take the final answer as the maximum over all table entries.

```text
S(i, j) = max(0,
    S(i-1, j-1) + score(x_i, y_j),
    S(i-1, j) - g,
    S(i, j-1) - g
)
```

**Local-alignment base cases:** `S(i, 0) = 0` and `S(0, j) = 0`.
**Answer:** `max` over all table entries, not `S(m, n)`.

**Complexity:** `O(mn)` time and space.

---

### 3.6 LIS-Style “Ending at i” DP

**State:**

```text
L(i) = length/value of the best valid subsequence ending exactly at position i.
```

**Base case:**

```text
L(i) = 1 initially
```

**Recurrence:**

```text
L(i) = 1 + max over j < i and compatible(j, i) of L(j)
```

If no compatible `j` exists, `L(i) = 1`.

**Answer:**

```text
max_i L(i)
```

**Complexity:** usually `O(n^2)` unless a data structure improves the max query.

**Trap:** the answer is not necessarily `L(n)`.

---

### 3.7 Longest Palindromic Subsequence

**State:**

```text
P(i, j) = length of the longest palindromic subsequence in x[i..j].
```

**Base cases:**

```text
P(i, i) = 1
P(i, j) = 0 if i > j
```

**Recurrence:**

```text
If x_i = x_j:
    P(i, j) = 2 + P(i+1, j-1)
Otherwise:
    P(i, j) = max(P(i+1, j), P(i, j-1))
```

**Filling order:** increasing interval length `j - i`.

**Answer:** `P(1, n)`.

**Complexity:** `O(n^2)` time and space; value-only space can be reduced.

---

### 3.8 Shortest Common Supersequence

**Problem:** shortest string that contains both `x[1..m]` and `y[1..n]` as subsequences.

**State:**

```text
S(i, j) = length of the shortest common supersequence of x[1..i] and y[1..j].
```

**Base cases:**

```text
S(i, 0) = i
S(0, j) = j
```

**Recurrence:**

```text
If x_i = y_j:
    S(i, j) = 1 + S(i-1, j-1)
Otherwise:
    S(i, j) = 1 + min(S(i-1, j), S(i, j-1))
```

**Answer:** `S(m, n)`.

**Complexity:** `O(mn)` time and space.

Alternative identity: `SCS length = m + n - LCS length`.

---

### 3.9 Interval DP / String Breaking / Matrix Chain

**State:**

```text
C(i, j) = optimum cost for solving the interval from i to j.
```

**Base cases:**

```text
C(i, i+1) = 0       // if no split point lies inside
```

**Recurrence:**

```text
C(i, j) = min over k with i < k < j of C(i, k) + C(k, j) + cost(i, j, k)
```

Often `cost(i, j, k)` is simply `cost(i, j)`, e.g. the length of the interval being cut.
Exception: for matrix-chain multiplication the cost depends on `k`, namely `p_{i-1}·p_k·p_j`.

**Filling order:** increasing interval length.

**Complexity:** usually `O(n^3)` time and `O(n^2)` space.

---

### 3.10 Floyd–Warshall All-Pairs Shortest Paths

**State:**

```text
d_k(i, j) = length of the shortest path from i to j using only intermediate vertices from {1, ..., k}.
```

**Base cases:**

```text
d_0(i, j) = 0            if i = j
          = weight(i, j) if edge (i, j) exists
          = ∞            otherwise
```

**Recurrence:**

```text
d_k(i, j) = min(d_{k-1}(i, j), d_{k-1}(i, k) + d_{k-1}(k, j))
```

**Justification:** the shortest path either does not use vertex `k`, or it uses `k` and splits into an optimal path from `i` to `k` and from `k` to `j`.

**Complexity:** `O(n^3)` time. Space can be `O(n^2)` using in-place updates.

---

### 3.11 Minimum Jumps to End

**Problem:** array `A[0..n-1]`, where `A[j]` is the maximum jump length from position `j`. Find the minimum jumps from `0` to `n-1`.

**State:**

```text
D(i) = minimum number of jumps needed to reach position i from position 0.
```

**Base case:**

```text
D(0) = 0
```

**Recurrence:**

```text
D(i) = min over 0 ≤ j < i and j + A[j] ≥ i of D(j) + 1
```

If no such `j` exists, set `D(i) = ∞`.

**Answer:** `D(n-1)` or “unreachable” if `∞`.

**Complexity:** `O(n^2)` time, `O(n)` space.

---

### 3.12 Maximum Independent Set on a Tree

**Problem:** find the largest set of tree vertices with no adjacent pair.

**State:**

```text
L(u) = size of the largest independent set in the subtree rooted at u.
```

**Recurrence:**

```text
L(u) = max(
    1 + sum over grandchildren w of u of L(w),
    sum over children v of u of L(v)
)
```

**Justification:** either include `u`, which forbids all children but allows grandchildren, or exclude `u`, which allows optimal choices in all child subtrees.

**Alternative (two-state formulation, often easier to justify in exams):**

```text
incl(u) = 1 + Σ over children v of excl(v)
excl(u) = Σ over children v of max(incl(v), excl(v))
```

**Answer:** `max(incl(root), excl(root))`.

**Evaluation order:** postorder from leaves to root.

**Complexity:** `O(|V| + |E|) = O(|V|)` for a tree.

**Trap:** this works for trees. Maximum independent set in a general graph is NP-hard.

---

### 3.13 Hamiltonian Path via Bitmask DP

**Problem:** decide whether a directed graph has a path visiting every vertex exactly once.

**State:**

```text
H(S, v) = true iff there is a path that visits exactly the vertices in S and ends at v.
```

**Base cases:**

```text
H({v}, v) = true for every vertex v
```

**Recurrence:**

```text
H(S, v) = OR over u in S \ {v} such that (u, v) is an edge of H(S \ {v}, u)
```

**Filling order:** increasing subset size `|S|`.

**Answer:**

```text
OR over v of H(V, v)
```

**Complexity:** `O(2^n n^2)` time, `O(2^n n)` space.

---

### 3.14 Two-Bag Knapsack Variant

**State:**

```text
K(i, x, y) = maximum value using items 1..i with remaining capacities x and y in the two bags.
```

**Recurrence:**

```text
K(i, x, y) = max(
    K(i-1, x, y),                         // do not take item i
    v_i + K(i-1, x - w_i, y),              // put item i in bag 1, if w_i ≤ x
    v_i + K(i-1, x, y - w_i)               // put item i in bag 2, if w_i ≤ y
)
```

**Answer:** `K(n, W_1, W_2)`.

**Complexity:** `O(n W_1 W_2)` time and space.

---

## 4. Quick Recognition Table

| Problem shape | Likely DP state | Filling order | Typical time |
|---|---|---|---|
| Choose subset under capacity | `OPT(i, w)` | increasing `i`, `w` | `O(nW)` |
| Unlimited coins/items | `OPT(w)` or `OPT(i, w)` | increasing amount | `O(nW)` |
| Two strings | `OPT(i, j)` | prefix grid | `O(mn)` |
| Subsequence ending at position | `OPT(i)` | increasing `i` | `O(n^2)` |
| Palindrome / interval | `OPT(i, j)` | increasing interval length | `O(n^2)` or `O(n^3)` |
| Split interval at `k` | `OPT(i, j)` | increasing interval length | `O(n^3)` |
| Tree | `OPT(u, state)` | postorder DFS | often `O(n)` or `O(nk^2)` |
| Visit subset ending at vertex | `OPT(S, v)` | increasing `|S|` | `O(2^n n^2)` |
| All-pairs paths | `d_k(i, j)` | increasing allowed vertex `k` | `O(n^3)` |

---

## 5. Worked Example — Longest Palindromic Subsequence

### Problem

Input: sequence `x = (x_1, ..., x_n)`. Output the maximum length of a subsequence of `x` that is a palindrome.

### State

```text
dp(i, j) = maximum length of a palindromic subsequence in x_i, ..., x_j,
for 1 ≤ i ≤ j ≤ n.
```

The answer is `dp(1, n)`.

### Base cases

```text
dp(i, i) = 1
```

For convenience:

```text
dp(i, j) = 0 if i > j
```

### Recurrence

```text
If x_i = x_j:
    dp(i, j) = dp(i+1, j-1) + 2
Otherwise:
    dp(i, j) = max(dp(i+1, j), dp(i, j-1))
```

### Justification

If the two endpoints match, they can be used as the first and last characters of a palindrome, plus the optimal palindromic subsequence of the inner interval. If they do not match, at least one endpoint must be excluded, so we take the better of excluding the left endpoint or excluding the right endpoint.

### Filling order

All dependencies have smaller interval length, so fill by increasing `j - i`:

```text
for length = 0 to n-1:
    for i = 1 to n - length:
        j = i + length
        compute dp(i, j)
```

### Reconstruction

Store `choice(i, j)` as `MATCH`, `SKIP_LEFT`, or `SKIP_RIGHT`. Start from `(1, n)` and follow the recorded choices until reaching a base case.

### Complexity

There are `Θ(n^2)` intervals and each recurrence takes `O(1)`, so time is `Θ(n^2)`. Space is `Θ(n^2)` for the table and choice array, reducible if only the value is required.

---

## 6. Common Traps and How to Avoid Them

| Trap | Correct response |
|---|---|
| Confusing 0/1 and unbounded knapsack | 0/1 include uses `i-1`; unbounded use may stay at `i` or use capacity-only recurrence. |
| Forgetting base cases | Always include empty prefix, zero capacity, singleton interval, or impossible `∞` states. |
| Vague state definition | Define exactly what every index means and where the final answer is. |
| Wrong final answer for “ending at i” states | Use `max_i OPT(i)`, not automatically `OPT(n)`. |
| Missing recurrence justification | Add one Principle-of-Optimality sentence per case. |
| Wrong filling order | Name the monotonic measure: `i`, `w`, `i+j`, interval length, postorder, or `|S|`. |
| Not handling infeasible min states | Initialise to `∞`; say final `∞` means no solution. |
| Claiming `O(nW)` is polynomial | It is pseudo-polynomial in input size when `W` is numeric. |
| Forgetting reconstruction | Store `choice`/backpointers or recompute branches while tracing back. |
| Applying tree DP to general graph | Tree recurrences rely on subtrees being independent. |
| Local vs global alignment | Local alignment includes option `0` and final answer is max over all cells. |
| Greedy counterexample too vague | Give a concrete input and compute greedy value vs optimal value. |

---

## 7. Final Exam Checklist

Before finishing a DP answer, check that you have written:

- [ ] State definition with precise index ranges.
- [ ] Base cases, including impossible states if relevant.
- [ ] Recurrence in mathematical form.
- [ ] One-sentence justification using the Principle of Optimality.
- [ ] Filling order and table dimensions.
- [ ] Final answer location.
- [ ] Time complexity as states × work per state.
- [ ] Space complexity.
- [ ] Reconstruction method if an actual solution is requested.
- [ ] Edge cases: empty input, zero capacity, unreachable state, local-vs-global objective.

---

## 8. Ultra-Compact DP Writing Formula

When under time pressure, write this skeleton and fill in the blanks:

```text
Let OPT(...) be ... .
Base cases: ... .
For the recurrence, consider the last decision: either ... or ... . Therefore,
OPT(...) = ... .
The recurrence is correct because after fixing the last decision, the remaining part must be optimal for the corresponding subproblem by the Principle of Optimality.
Compute entries in increasing ... so all dependencies are already available.
The answer is ... .
There are ... states and each takes ... time, so total time is ... and space is ... .
To reconstruct a solution, store a choice/backpointer for each state and trace back from ... .
```

---

*End of final master template.*
