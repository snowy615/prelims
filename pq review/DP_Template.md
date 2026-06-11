# Dynamic Programming — Formal Template (Exam-Ready)

> **Purpose**: A reusable, step-by-step blueprint for writing DP solutions in exams. Every step is mandatory. Omitting any step risks losing marks.

---

## Step 0: Problem Restatement (1 sentence)

Restate the optimisation/decision problem in your own words. This shows the examiner you understood the question.

> **Example (LPS)**: Given a sequence $x = (x_1, \dots, x_n)$, find the maximum length of a subsequence that is a palindrome.

---

## Step 1: Define the Quantity to be Optimised

### What to write
1. **Name** your DP table.
2. **State the exact meaning** of each entry in plain English.
3. **Specify the domain** (indices/ranges) of every variable.
4. **State the connection** between the table entry and the final answer.

### Template

> Let $\text{dp}[i][j]$ denote **the maximum length of a palindromic subsequence** within the contiguous subsequence $x_i, x_{i+1}, \dots, x_j$, where $1 \leq i \leq j \leq n$.
>
> **Final answer**: The value $\text{dp}[1][n]$ is the optimum for the original problem.

### Checklist
- [ ] Is the definition **self-contained**? (A reader can understand it without re-reading the question.)
- [ ] Are all index constraints explicit?
- [ ] Is the final answer clearly identified?

---

## Step 2: Write the Recurrence Relation

### What to write
1. **Base cases** (smallest subproblems).
2. **General case** (how larger subproblems depend on smaller ones).
3. **Justification** (1–2 sentences explaining *why* the recurrence is correct).

### Template

> **Base cases**:
> - If $i = j$: $\text{dp}[i][i] = 1$ (a single element is a palindrome of length 1).
> - If $i > j$: $\text{dp}[i][j] = 0$ (empty interval; convention for convenience).
>
> **General case** ($i < j$):
> $$
> \text{dp}[i][j] =
> \begin{cases}
> \text{dp}[i+1][j-1] + 2 & \text{if } x_i = x_j \\
> \max\bigl(\text{dp}[i+1][j],\; \text{dp}[i][j-1]\bigr) & \text{if } x_i \neq x_j
> \end{cases}
> $$
>
> **Justification**:
> - If $x_i = x_j$, both ends can be matched and added to the optimal solution of the inner interval $(i+1, j-1)$.
> - If $x_i \neq x_j$, at least one of them cannot be part of the optimal palindrome, so we take the better of discarding $x_i$ (interval $i+1 \dots j$) or discarding $x_j$ (interval $i \dots j-1$).

### Checklist
- [ ] Are all base cases covered?
- [ ] Is the recurrence **exhaustive**? (No missing branches.)
- [ ] Is the justification tied directly to the problem structure (not hand-waving)?

---

## Step 3: Filling Direction (Order of Computation)

### What to write
1. **Dependency graph**: Which entries does $\text{dp}[i][j]$ depend on?
2. **Filling order**: In what sequence must entries be computed so that every dependency is already resolved?
3. **Loop structure** (pseudo-code or explicit nested loops).

### Template

> **Dependencies**:
> $\text{dp}[i][j]$ depends on:
> - $\text{dp}[i+1][j-1]$ (shorter interval, inner)
> - $\text{dp}[i+1][j]$   (shorter interval, left-shifted)
> - $\text{dp}[i][j-1]$   (shorter interval, right-shifted)
>
> All dependencies have a **smaller interval length** than $(i, j)$.
>
> **Filling order**:
> Iterate by increasing interval length $\ell = j - i$:
> ```
> for length = 0 to n-1:
>     for i = 1 to n - length:
>         j = i + length
>         compute dp[i][j] using the recurrence
> ```
> This guarantees that every required subproblem has already been solved.

### Checklist
- [ ] Did you explicitly state the **monotonic measure** that increases? (e.g., interval length, index sum, item count.)
- [ ] Is the loop structure unambiguous?
- [ ] Would a reader be able to code it from your description alone?

---

## Step 4: Backtracking (Reconstructing the Solution)

### What to write
1. **Decision record** (if needed): How to remember which branch of the recurrence was taken.
2. **Traceback procedure**: Starting from the final answer, follow the choices back to base cases.
3. **Output format**: What the reconstructed solution looks like.

### Template

> **Decision array** (optional but recommended for reconstruction):
> Maintain $\text{choice}[i][j] \in \{\text{MATCH}, \text{SKIP-LEFT}, \text{SKIP-RIGHT}\}$ to record which case achieved the max.
>
> **Traceback algorithm**:
> ```
> function RECONSTRUCT(i, j):
>     if i > j: return empty sequence
>     if i == j: return (x_i)
>     if choice[i][j] == MATCH:
>         return (x_i) ++ RECONSTRUCT(i+1, j-1) ++ (x_j)
>     else if choice[i][j] == SKIP-LEFT:
>         return RECONSTRUCT(i+1, j)
>     else:
>         return RECONSTRUCT(i, j-1)
> ```
> The call `RECONSTRUCT(1, n)` returns one optimal palindromic subsequence.
>
> **Time for backtracking**: $O(n)$, since each call strictly reduces $j - i$.

### Checklist
- [ ] Is the traceback clearly a reverse walk of the recurrence?
- [ ] Is its complexity stated?
- [ ] If the question only asks for the *value* (not the sequence), did you still mention that backtracking is possible?

---

## Step 5: Complexity Analysis

### What to write
1. **Time complexity**: Number of subproblems × work per subproblem.
2. **Space complexity**: Size of the DP table + auxiliary structures.
3. **Optimisation note** (optional but impressive): Can space be reduced? State the reduced bound.

### Template

> **Time complexity**:
> - There are $\binom{n}{2} + n = \Theta(n^2)$ subproblems (all pairs $1 \leq i \leq j \leq n$).
> - Each subproblem requires $O(1)$ time to evaluate the recurrence (constant number of table lookups and a max operation).
> - **Total**: $T(n) = \Theta(n^2)$.
>
> **Space complexity**:
> - The table $\text{dp}[1..n][1..n]$ stores $\Theta(n^2)$ integers.
> - The $\text{choice}$ table also stores $\Theta(n^2)$ entries (if reconstruction is required).
> - **Total**: $S(n) = \Theta(n^2)$.
>
> **Space optimisation**:
> Since computing entries of length $\ell$ only requires entries of length $\ell - 1$ and $\ell - 2$, we can reduce to two 1D arrays of size $n$.
> - **Reduced space**: $S(n) = O(n)$.

### Checklist
- [ ] Did you count subproblems explicitly?
- [ ] Is the work-per-subproblem stated?
- [ ] Are both time and space given?
- [ ] (Bonus) Did you mention any space optimisation?

---

## Complete Example: Longest Palindromic Subsequence (LPS)

Below is a fully worked example that follows the template exactly. Use it as a reference for formatting.

---

### Problem
Input: sequence $x = (x_1, \dots, x_n)$.  
Output: maximum length of a subsequence of $x$ that is a palindrome.

---

### Step 1 — Quantity
> Let $\text{dp}[i][j]$ be the maximum length of a palindromic subsequence of the subsequence $(x_i, \dots, x_j)$, for $1 \leq i \leq j \leq n$.  
> The answer is $\text{dp}[1][n]$.

---

### Step 2 — Recurrence
> **Base cases**:
> - $\text{dp}[i][i] = 1$ for all $i$.
> - $\text{dp}[i][j] = 0$ if $i > j$.
>
> **Recurrence** ($i < j$):
> $$
> \text{dp}[i][j] =
> \begin{cases}
> \text{dp}[i+1][j-1] + 2 & x_i = x_j \\
> \max(\text{dp}[i+1][j],\; \text{dp}[i][j-1]) & x_i \neq x_j
> \end{cases}
> $$
>
> **Justification**: If the two ends match, they can both be appended to the optimal palindrome inside. If they do not match, the optimal solution must exclude at least one end; we take the better exclusion.

---

### Step 3 — Filling Direction
> $\text{dp}[i][j]$ depends only on entries with a smaller interval length $j - i$.  
> Therefore, fill by increasing length:
> ```
> for l = 0 to n-1:
>     for i = 1 to n - l:
>         j = i + l
>         compute dp[i][j]
> ```

---

### Step 4 — Backtracking
> Store $\text{choice}[i][j]$ indicating which branch achieved the maximum.  
> Traceback from $(1, n)$:
> - $\text{MATCH}$: output $x_i$ and $x_j$, recurse on $(i+1, j-1)$.
> - $\text{SKIP-LEFT}$: recurse on $(i+1, j)$.
> - $\text{SKIP-RIGHT}$: recurse on $(i, j-1)$.  
> Backtracking cost: $O(n)$.

---

### Step 5 — Complexity
> - **Subproblems**: $\Theta(n^2)$.
> - **Work per subproblem**: $O(1)$.
> - **Time**: $\Theta(n^2)$.
> - **Space**: $\Theta(n^2)$ for the table; reducible to $O(n)$ if only the value is required.

---

## Quick Reference: Common DP Patterns

| Pattern | Quantity | Filling Order | Typical Complexity |
|---------|----------|---------------|------------------|
| **Sequence** (e.g., LIS) | $\text{dp}[i]$ = optimum ending at $i$ | $i = 1 \dots n$ | $O(n^2)$ or $O(n \log n)$ |
| **Interval** (e.g., LPS, Matrix Chain) | $\text{dp}[i][j]$ = optimum on $i \dots j$ | by increasing length | $O(n^3)$ or $O(n^2)$ |
| **Knapsack 0/1** | $\text{dp}[i][w]$ = max value using first $i$ items, weight $w$ | $i = 1 \dots n$, $w = 0 \dots W$ | $O(nW)$ |
| **Knapsack Unbounded** | $\text{dp}[w]$ = max value for weight $w$ | $w = 0 \dots W$ | $O(nW)$ |
| **Tree DP** | $\text{dp}[v][k]$ = optimum in subtree of $v$ with $k$ selected | post-order DFS | $O(n \cdot k^2)$ |
| **Edit Distance** | $\text{dp}[i][j]$ = distance between prefixes $i$ and $j$ | $i = 0 \dots n$, $j = 0 \dots m$ | $O(nm)$ |

---

## Exam Writing Tips

1. **Never skip Step 1**. Examiners award marks for a clear, correct definition even if the recurrence is slightly wrong.
2. **Write the recurrence in cases** (piecewise notation) rather than prose. It is clearer and harder to misinterpret.
3. **Always justify the recurrence**. One sentence per case is enough.
4. **Explicitly state the filling order**. "Fill by increasing $j - i$" is better than "fill in the right order."
5. **State both time and space**. Many students forget space complexity and lose easy marks.
6. **If the question asks only for the value**, still mention that backtracking is possible in $O(n)$ additional time — it shows completeness of thought.

---

*Template compiled for Oxford CS1 DAA examination style. Adapt index conventions (0-based vs 1-based) to match the question.*
