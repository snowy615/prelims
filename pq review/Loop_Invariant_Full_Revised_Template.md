# Loop Invariant Proof — Full Revised Template

> **Purpose**: A complete CS2 loop-invariant proof normally has four parts: invariant statement, initialisation, maintenance, and termination. Use this full version when learning or writing a detailed proof.
>
> **Important scoring note**: A typical invariant question is marked out of 5 overall: statement ≈ 1 mark, initialisation ≈ 1 mark, maintenance ≈ 2 marks, termination ≈ 1 mark. The examples below are labelled **complete/incomplete**, not “5/5 for each subsection”.

---

## 0. The Four-Part Proof Structure

```text
1. Invariant statement:
   State I(...) formally: bounds + processed part + current/frontier state.

2. Initialisation:
   Prove I(...) holds before the first loop iteration.

3. Maintenance:
   Assume I(...) holds at the start of an iteration.
   Prove it still holds after the body and update, usually for i+1, i-1, or the new interval.

4. Termination:
   Define a loop variant V, prove it strictly decreases, and prove it is bounded below.
   Then combine invariant + exit condition to get the postcondition.
```

**Golden rule**: Every variable changed inside the loop must appear somewhere in the invariant or the variant with a precise meaning.

---

# Part 1 — Invariant Statement

## 1.1 What a Complete Invariant Statement Contains

A strong invariant usually has these clauses:

```text
Invariant I(...):
  (1) Bounds:        concrete range for loop index variables
  (2) Processed:     what is already correct for the processed prefix/suffix/region
  (3) Current:       exact meaning of accumulator/frontier variables
  (4) Remaining:     what remains unexamined, optional but often useful
```

Use formal notation where helpful:

- prefix: `a[0..i)` means indices `0, ..., i-1`
- suffix: `a[i+1..n)` means indices `i+1, ..., n-1`
- half-open interval: `[i, j)` includes `i` but excludes `j`
- closed interval: `[i, j]` includes both ends
- quantifier form: `∀k. 0 ≤ k < i ⇒ P(k)`

---

## 1.2 Left-to-Right Accumulation

### Example: longest consecutive run `lp(a)`

This version assumes the first element has already been processed, so the loop starts at `i = 1` and requires `a.length ≥ 1`.

```text
Invariant I(i):
  (1) Bounds:     1 ≤ i ≤ a.length
  (2) Processed:  maxlen = max { run length of consecutive equal elements in a[0..i) }
  (3) Current:    curlen = length of the consecutive run ending at a(i-1)
  (4) Remaining:  a[i..a.length) is unexamined
```

### Empty-prefix accumulation template

Use this when the loop starts at `i = 0`.

```text
Invariant I(i):
  (1) Bounds:     0 ≤ i ≤ a.length
  (2) Processed:  acc = F(a[0..i))
  (3) Current:    if a frontier variable exists, define it without using a(i-1) when i = 0
  (4) Remaining:  a[i..a.length) is unexamined
```

**Important boundary rule**: If the loop starts at `i = 0`, do **not** write `a(i-1)` in the invariant unless you guard it with a condition. `a(-1)` is not a valid array access.

---

## 1.3 Right-to-Left Scan

For right-to-left algorithms, the processed region is the **suffix**, not the prefix.

### Sentinel version, loop starts at `i = n - 1`

This version uses `curmax = -∞` before the first iteration.

```text
Invariant I(i):
  (1) Bounds:     -1 ≤ i < n
  (2) Processed:  curmax = max({ a(k) | i < k < n } ∪ { -∞ })
  (3) Current:    ∀j. i < j < n ⇒
                    (b(j) = true ⇔ ∀k. j < k < n ⇒ a(j) > a(k))
  (4) Remaining:  a[0..i] is unexamined; b[0..i] is garbage/unprocessed
```

At termination, `i = -1`, so `∀j. 0 ≤ j < n ⇒ b(j)` is correct.

### Preprocessed-last-element version

If the code first sets `b(n-1) = true`, `curmax = a(n-1)`, and starts the loop at `i = n-2`, use:

```text
Invariant I(i):
  (1) Bounds:     -1 ≤ i ≤ n-2
  (2) Processed:  curmax = max { a(k) | i < k < n }
  (3) Current:    ∀j. i < j < n ⇒
                    (b(j) = true ⇔ ∀k. j < k < n ⇒ a(j) > a(k))
  (4) Remaining:  a[0..i] is unexamined; b[0..i] is garbage/unprocessed
```

**Avoid this mistake**: Do not write `∀j ∈ (i..n)` if that might include `j = n`; `a(n)` and `b(n)` are out of bounds. Prefer `∀j. i < j < n ⇒ ...`.

---

## 1.4 Binary Search

There are two common binary-search invariants. Do not mix them.

### A. Lower-bound style binary search

This finds the first index where `a(index) ≥ v`.

```text
Invariant I(l, r):
  (1) Bounds:     0 ≤ l ≤ r ≤ a.length
  (2) Processed:  ∀k. 0 ≤ k < l ⇒ a(k) < v
                  ∀k. r ≤ k < a.length ⇒ a(k) ≥ v
  (3) Current:    if the lower-bound answer exists, it lies in [l, r]
                  equivalently, the first possible answer has not been eliminated
  (4) Remaining:  [l, r) is the active search interval
```

Equivalent shorthand:

```text
a[0..l) < v ≤ a[r..n)
```

This shorthand is correct only if you mean the lower-bound variant, where the right side means `a(k) ≥ v`, not necessarily `a(k) > v`.

### B. Exact-search style binary search

This searches for a value `v` and usually returns immediately when `a(m) == v`.

```text
Invariant I(l, r):
  (1) Bounds:     0 ≤ l ≤ r ≤ a.length
  (2) Processed:  ∀k. 0 ≤ k < l ⇒ a(k) < v
                  ∀k. r ≤ k < a.length ⇒ a(k) > v
  (3) Current:    if v occurs in a, then some occurrence of v lies in a[l..r)
  (4) Remaining:  [l, r) is the active search interval
```

---

## 1.5 Rotation Point / Minimum in Rotated Sorted Array

Use a **closed interval** invariant here. The common half-open version `[i, j)` is easy to get wrong because the update `j = m` must keep `m` inside the candidate interval.

Assume `a` is non-empty and is a rotation of a strictly increasing array. Let:

```text
last = a(n-1)
rot = index of the minimum = first index k such that a(k) ≤ last
```

Correct invariant:

```text
Invariant I(i, j):
  (1) Bounds:     0 ≤ i ≤ rot ≤ j < n
  (2) Processed:  ∀k. 0 ≤ k < i ⇒ a(k) > last
                  ∀k. j < k < n ⇒ a(k) ≤ last
  (3) Current:    rot ∈ [i, j]
  (4) Remaining:  [i, j] is the active candidate interval
```

Typical loop:

```scala
var i = 0
var j = a.length - 1
while (i < j) {
  val m = i + (j - i) / 2
  if (a(m) <= a(a.length - 1)) j = m
  else i = m + 1
}
// i == j == rot
```

---

## 1.6 Sliding Window / Two Pointers

```text
Invariant I(left, right):
  (1) Bounds:     0 ≤ left ≤ right ≤ a.length
  (2) Processed:  result = optimal value among all fully considered windows ending before right
  (3) Current:    a[left..right) is the current candidate window satisfying property P
  (4) Remaining:  a[right..a.length) has not yet been extended into the window
```

Be careful: the exact processed clause depends on the algorithm. For example, sometimes not all windows ending before `right` have been fully considered until the inner loop finishes.

---

## 1.7 Partition / Dutch National Flag

```text
Invariant I(lt, i, gt):
  (1) Bounds:     0 ≤ lt ≤ i ≤ gt ≤ n
  (2) Processed:  ∀k. 0 ≤ k < lt ⇒ a(k) < pivot
                  ∀k. lt ≤ k < i ⇒ a(k) = pivot
                  ∀k. gt ≤ k < n ⇒ a(k) > pivot
  (3) Current:    a[i..gt) is unprocessed
  (4) Remaining:  when i = gt, the whole array is partitioned
```

---

## 1.8 Single-Pass Index Tracking

Example: `lastIndex` array/map.

```text
Invariant I(i):
  (1) Bounds:     0 ≤ i ≤ a.length
  (2) Processed:  ∀v. b(v) = max { j | 0 ≤ j < i ∧ a(j) = v }, or -1 if no such j exists
  (3) Current:    b records the rightmost occurrence of every value seen so far
  (4) Remaining:  a[i..a.length) may contain newer rightmost occurrences
```

If `b` is an array indexed by values, also state the allowed value range, e.g. `0 ≤ a(k) < b.length`.

---

# Part 2 — Initialisation Proof

## 2.1 Universal Initialisation Template

```text
Initialisation: Before the first iteration, the variables are:
  [i = initial value], [accumulator = initial value], ...

We verify I(initial) component by component.

Bounds:
  Show the initial index values satisfy the invariant bounds.

Processed part:
  Identify the initially processed region.
  Usually it is empty, e.g. a[0..0), or a singleton, e.g. a[0..1).
  Prove the accumulator/result variables match that region by definition.

Current/frontier state:
  If there is a frontier element, prove the frontier variable matches it.
  If the processed region is empty, do not refer to a(i-1); instead say there is no frontier yet
  or use a sentinel/base value.

Conclusion:
  Therefore I(initial) holds before the first loop iteration. □
```

---

## 2.2 Empty-Prefix Initialisation

Example: summing an array.

```text
Before the loop: i = 0 and sum = 0.

Invariant I(i):
  Bounds:     0 ≤ i ≤ n
  Processed:  sum = Σ { a(k) | 0 ≤ k < i }

Bounds:
  i = 0, so 0 ≤ i ≤ n.

Processed:
  The processed prefix is a[0..0), which is empty.
  The sum of an empty set of terms is 0 by definition.
  Since sum = 0, the processed clause holds.

Therefore I(0) holds. □
```

---

## 2.3 Non-Empty Base Case Initialisation

Example: longest consecutive run.

```text
Before the loop: i = 1, curlen = 1, maxlen = 1.
Precondition: a.length ≥ 1.

Invariant I(i):
  Bounds:     1 ≤ i ≤ a.length
  Processed:  maxlen = max run length in a[0..i)
  Current:    curlen = run length ending at a(i-1)

Bounds:
  i = 1 and a.length ≥ 1, so 1 ≤ i ≤ a.length.

Processed:
  a[0..1) contains exactly a(0).
  A single element forms a run of length 1, so maxlen = 1 is correct.

Current:
  curlen = 1, which is the length of the run ending at a(0) = a(i-1).

Therefore I(1) holds. □
```

---

## 2.4 Binary Search Initialisation

Lower-bound style:

```text
Before the loop: l = 0 and r = n.

Bounds:
  0 ≤ 0 ≤ n ≤ n, so 0 ≤ l ≤ r ≤ n.

Processed:
  The left eliminated region a[0..0) is empty, so ∀k < l. a(k) < v holds vacuously.
  The right eliminated region a[n..n) is empty, so ∀k ≥ r. a(k) ≥ v holds vacuously.

Current:
  No possible answer has been eliminated, so the lower-bound answer is still in [0, n].

Therefore I(0, n) holds. □
```

Rotation point:

```text
Before the loop: i = 0 and j = n - 1.

Bounds:
  Since 0 ≤ rot ≤ n-1, we have 0 ≤ i ≤ rot ≤ j < n.

Processed:
  There are no indices k < 0 and no indices k > n-1, so both processed clauses are vacuously true.

Current:
  The active interval [0, n-1] contains every index, so it contains rot.

Therefore I(0, n-1) holds. □
```

---

# Part 3 — Maintenance Proof

## 3.1 Universal Maintenance Template

```text
Maintenance: Assume I(...) holds at the start of an arbitrary iteration,
and assume the loop condition is true.

Write the invariant hypothesis explicitly.

For each branch:
  Case 1: [branch condition]
    Variable updates:
      x' = ...
      i' = ...

    Check I(new variables):
      Bounds:     prove new indices are in range
      Processed:  prove the processed region has been extended correctly
      Current:    prove accumulator/frontier variables have the correct meaning

  Case 2: [other branch]
    Repeat the same checks.

Conclusion:
  In every branch, I(new variables) holds. Therefore the invariant is maintained. □
```

---

## 3.2 Maintenance: Longest Consecutive Run

Assume `I(i)` holds at the start of an iteration with `1 ≤ i < a.length`.

### Case 1 — `a(i) == a(i-1)`

```text
Updates:
  curlen' = curlen + 1
  maxlen' = max(maxlen, curlen')
  i' = i + 1

Bounds:
  Since i < a.length, i' = i + 1 ≤ a.length.
  Since i ≥ 1, i' ≥ 2, so bounds hold.

Processed:
  By hypothesis, maxlen is the maximum run length in a[0..i).
  Since a(i) = a(i-1), the run ending at a(i-1) extends to a(i).
  Therefore curlen' is the run length ending at a(i).
  maxlen' = max(maxlen, curlen') is exactly the maximum run length in a[0..i').

Current:
  curlen' is the length of the run ending at a(i) = a(i'-1).

Thus I(i') holds.
```

### Case 2 — `a(i) != a(i-1)`

```text
Updates:
  curlen' = 1
  maxlen' = maxlen
  i' = i + 1

Bounds:
  Same as Case 1.

Processed:
  The previous run does not extend. A new run of length 1 starts at a(i).
  This cannot create a run longer than the previous maxlen, so maxlen remains correct for a[0..i').

Current:
  curlen' = 1 is the length of the run ending at a(i) = a(i'-1).

Thus I(i') holds.
```

Therefore the invariant is maintained. □

---

## 3.3 Maintenance: Right-to-Left Leaders

Use the sentinel invariant:

```text
curmax = max({ a(k) | i < k < n } ∪ { -∞ })
∀j. i < j < n ⇒ (b(j) = true ⇔ ∀k. j < k < n ⇒ a(j) > a(k))
```

Assume `I(i)` holds at the start of an iteration with `i ≥ 0`.

### Case 1 — `a(i) > curmax`

```text
Updates:
  b(i) = true
  curmax' = a(i)
  i' = i - 1

Bounds:
  Since i ≥ 0, i' ≥ -1. Since i < n, i' < n.

Processed curmax:
  By hypothesis, curmax is the maximum of the old processed suffix i < k < n.
  Since a(i) > curmax, the new maximum over i ≤ k < n is a(i).
  After i' = i - 1, the processed suffix is i' < k < n, i.e. i ≤ k < n.
  Therefore curmax' is correct.

Current/output array:
  b(i) = true is correct because a(i) is greater than all elements to its right.
  For old processed indices j with i < j < n, b(j) remains correct by hypothesis.
  The new processed output region is ∀j. i' < j < n, i.e. j ∈ {i} ∪ {j | i < j < n}.

Thus I(i') holds.
```

### Case 2 — `a(i) ≤ curmax`

```text
Updates:
  b(i) = false
  curmax' = curmax
  i' = i - 1

Bounds:
  Same as Case 1.

Processed curmax:
  Since a(i) ≤ curmax, adding a(i) to the processed suffix does not change the maximum.
  Therefore curmax' = curmax is the maximum over i ≤ k < n = i' < k < n.

Current/output array:
  b(i) = false is correct because a(i) is not greater than all elements to its right.
  The old processed suffix remains correct by hypothesis.

Thus I(i') holds.
```

Therefore the invariant is maintained. □

---

## 3.4 Maintenance: Lower-Bound Binary Search

Assume:

```text
0 ≤ l ≤ r ≤ n
∀k < l. a(k) < v
∀k ≥ r. a(k) ≥ v
```

Loop condition: `l < r`. Let `m = l + (r - l) / 2`, so `l ≤ m < r`.

### Case 1 — `a(m) < v`

```text
Update: l' = m + 1, r' = r.

Bounds:
  Since m < r, m + 1 ≤ r, so l' ≤ r'. Bounds hold.

Processed:
  Old left side k < l already satisfies a(k) < v.
  Since the array is sorted and a(m) < v, every k with l ≤ k ≤ m also has a(k) < v.
  Therefore every k < l' has a(k) < v.
  Right side is unchanged.

Thus I(l', r') holds.
```

### Case 2 — `a(m) ≥ v`

```text
Update: l' = l, r' = m.

Bounds:
  Since l ≤ m, l' ≤ r'. Bounds hold.

Processed:
  Left side is unchanged.
  Since the array is sorted and a(m) ≥ v, every k with m ≤ k < r has a(k) ≥ v.
  Old right side k ≥ r already satisfies a(k) ≥ v.
  Therefore every k ≥ r' has a(k) ≥ v.

Thus I(l', r') holds.
```

Therefore the invariant is maintained. □

---

## 3.5 Maintenance: Rotation Point Binary Search

Invariant:

```text
0 ≤ i ≤ rot ≤ j < n
∀k < i. a(k) > last
∀k > j. a(k) ≤ last
rot ∈ [i, j]
```

Loop condition: `i < j`. Let `m = i + (j - i) / 2`, so `i ≤ m < j`.

### Case 1 — `a(m) ≤ last`

```text
Update: i' = i, j' = m.

Bounds:
  Since i ≤ m < j, we have 0 ≤ i' ≤ j' < n.
  Because a(m) ≤ last and rot is the first index with a(rot) ≤ last, rot ≤ m.
  Therefore i' ≤ rot ≤ j'.

Processed:
  Left processed region is unchanged.
  The new right processed region is k > m. These indices lie at or after the rotation point,
  so their values are ≤ last.

Current:
  rot ∈ [i, m] = [i', j'].

Thus I(i', j') holds.
```

### Case 2 — `a(m) > last`

```text
Update: i' = m + 1, j' = j.

Bounds:
  Since m < j, m + 1 ≤ j. Also i' ≥ 0 and j' < n.
  Because a(m) > last, m is before the rotation point, so rot > m.
  Therefore i' = m + 1 ≤ rot ≤ j'.

Processed:
  The new left processed region is k < m + 1, i.e. k ≤ m.
  These indices are before the rotation point, so their values are > last.
  Right processed region is unchanged.

Current:
  rot ∈ [m + 1, j] = [i', j'].

Thus I(i', j') holds.
```

Therefore the invariant is maintained. □

---

# Part 4 — Termination Proof

## 4.1 Universal Termination Template

```text
Termination: Define a loop variant V = [non-negative integer expression].

Strict decrease:
  At the start of an iteration, the loop condition holds.
  After the body, compute V'.
  Show V' < V, often V' = V - 1.

Bounded below:
  Show V ≥ 0 at the start of each iteration.

Conclusion:
  A non-negative integer cannot strictly decrease forever.
  Therefore the loop terminates. At termination, combine the invariant with the negated loop condition.
```

---

## 4.2 Left-to-Right Loop

```text
Variant: V = n - i.

Strict decrease:
  Loop condition gives i < n.
  Body updates i' = i + 1.
  V' = n - i' = n - (i + 1) = V - 1.

Bounded below:
  During the loop, i ≤ n, so V ≥ 0.

Therefore the loop terminates. □
```

---

## 4.3 Right-to-Left Loop

Use `V = i + 1`, not `V = i`, because `i` may become `-1` at termination.

```text
Variant: V = i + 1.

Strict decrease:
  Loop condition gives i ≥ 0.
  Body updates i' = i - 1.
  V' = i' + 1 = (i - 1) + 1 = i = V - 1.

Bounded below:
  During the loop, i ≥ 0, so V ≥ 1.
  At termination, i = -1 and V = 0.

Therefore the loop terminates. □
```

---

## 4.4 Binary Search / Shrinking Interval

For half-open intervals `[l, r)`:

```text
Variant: V = r - l.

Strict decrease:
  Loop condition gives l < r, so V > 0.
  Let m = l + (r-l)/2 with l ≤ m < r.
  If l' = m + 1, then r' - l' = r - (m+1) < r - l.
  If r' = m, then r' - l' = m - l < r - l.

Bounded below:
  V is a non-negative integer because l ≤ r.

Therefore the loop terminates. □
```

For the closed rotation-point interval `[i, j]`:

```text
Variant: V = j - i.

Strict decrease:
  Loop condition gives i < j, so V > 0.
  m satisfies i ≤ m < j.
  If j' = m, then V' = m - i < j - i = V.
  If i' = m + 1, then V' = j - (m+1) < j - i = V.

Bounded below:
  V ≥ 0 because i ≤ j.

Therefore the loop terminates. At exit, i = j, and the invariant gives i = j = rot. □
```

---

## 4.5 Nested Loops

For nested loops, prove termination of the inner loop and outer loop separately.

```text
Inner loop:
  Define V_inner and prove it strictly decreases and is bounded below.

Outer loop:
  Define V_outer and prove it strictly decreases and is bounded below.
  Each outer iteration is finite because the inner loop terminates.

Therefore the entire nested loop terminates. □
```

---

# Final Self-Check

Before submitting an invariant proof, check:

- [ ] Invariant has concrete bounds, not “i is valid”.
- [ ] Invariant states exactly which prefix/suffix/interval is processed.
- [ ] Every mutated variable has a precise mathematical meaning.
- [ ] Initialisation verifies bounds, processed part, and current/frontier state.
- [ ] Maintenance covers every branch and proves the invariant for the next state.
- [ ] Maintenance distinguishes newly processed elements from old processed elements.
- [ ] Termination defines a variant, proves strict decrease, and proves boundedness.
- [ ] For right-to-left scans, the left side is explicitly unprocessed/garbage.
- [ ] For binary search, the interval convention is consistent: half-open `[l,r)` or closed `[i,j]`, not mixed.
- [ ] For rotation point, use closed `[i,j]` if the update is `j = m`.

---

*Use this full version for learning and detailed homework-style proofs. For exams, use the quick version once the reasoning pattern is familiar.*
