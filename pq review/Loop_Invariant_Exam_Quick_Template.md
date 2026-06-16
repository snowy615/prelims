# Loop Invariant Proof — Exam Quick Version

> **Purpose**: A short version to memorise for CS2 loop-invariant questions. Use this in the exam when time is limited.

---

## 0. 5-Mark Structure

```text
Statement       ≈ 1 mark
Initialisation  ≈ 1 mark
Maintenance     ≈ 2 marks
Termination     ≈ 1 mark
```

Write in this order:

```text
1. I(...): Bounds + Processed + Current.
2. Init: show I holds before first iteration.
3. Maintenance: assume I, check each branch, prove I for next state.
4. Termination: variant V decreases and is bounded below.
```

---

# 1. Invariant Statement Cheat Sheet

## Universal Template

```text
Invariant I(...):
  Bounds:     [exact range of index variables]
  Processed:  [what is correct for processed prefix/suffix/interval]
  Current:    [meaning of accumulator/frontier variables]
  Remaining:  [unprocessed part, optional]
```

**Rule**: Every mutated variable must appear in the invariant or variant.

---

## Left-to-Right Loop

For loop over `a[0..i)`:

```text
I(i):
  Bounds:     0 ≤ i ≤ n
  Processed:  acc = F(a[0..i))
  Remaining:  a[i..n) unexamined
```

For `lp` / longest consecutive run, where first element is preprocessed:

```text
I(i):
  Bounds:     1 ≤ i ≤ n
  Processed:  maxlen = max run length in a[0..i)
  Current:    curlen = run length ending at a(i-1)
  Remaining:  a[i..n) unexamined
```

**Boundary warning**: If `i = 0`, do not write `a(i-1)`.

---

## Right-to-Left Loop

Sentinel version, loop starts at `i = n-1`:

```text
I(i):
  Bounds:     -1 ≤ i < n
  Processed:  curmax = max({a(k) | i < k < n} ∪ {-∞})
  Current:    ∀j. i < j < n ⇒
                (b(j) = true ⇔ ∀k. j < k < n ⇒ a(j) > a(k))
  Remaining:  a[0..i] and b[0..i] unprocessed/garbage
```

Do **not** write `j ∈ (i..n)` if it might include `j = n`.

---

## Binary Search

### Lower-bound style: first index with `a(index) ≥ v`

```text
I(l,r):
  Bounds:     0 ≤ l ≤ r ≤ n
  Processed:  ∀k < l. a(k) < v
              ∀k ≥ r. a(k) ≥ v
  Remaining:  [l,r) contains the possible answer boundary
```

### Exact-search style

```text
I(l,r):
  Bounds:     0 ≤ l ≤ r ≤ n
  Processed:  ∀k < l. a(k) < v
              ∀k ≥ r. a(k) > v
  Current:    if v occurs, some occurrence is in a[l..r)
```

---

## Rotation Point Binary Search

Use closed interval `[i,j]`.

```text
last = a(n-1)
rot = first index k such that a(k) ≤ last

I(i,j):
  Bounds:     0 ≤ i ≤ rot ≤ j < n
  Processed:  ∀k < i. a(k) > last
              ∀k > j. a(k) ≤ last
  Current:    rot ∈ [i,j]
```

Typical update:

```scala
m = i + (j - i) / 2
if (a(m) <= last) j = m
else i = m + 1
```

Do **not** use `[i,j)` with `j = m`; it can exclude the answer when `rot = m`.

---

# 2. Initialisation Cheat Sheet

## Template

```text
Before the loop: [variables = initial values].

Bounds:
  Show initial indices satisfy the invariant bounds.

Processed:
  Identify initial processed region: usually empty a[0..0) or singleton a[0..1).
  Explain why the accumulator/result has the correct base value.

Current:
  Explain frontier variable, if any.
  If i = 0, say there is no frontier yet; do not use a(i-1).

Therefore I(initial) holds. □
```

## Common Base Cases

```text
Empty prefix:      a[0..0) is empty; sum/count/max-so-far must match empty definition.
Singleton prefix:  a[0..1) contains a(0); run length / maxlen is usually 1.
Binary search:     l = 0, r = n; eliminated regions are empty, so clauses hold vacuously.
Rotation point:    i = 0, j = n-1; [0,n-1] contains every possible index.
```

---

# 3. Maintenance Cheat Sheet

## Template

```text
Assume I(...) holds at the start of an iteration and the loop condition is true.

Case 1: [branch condition]
  Updates: x' = ..., i' = ...
  Bounds: prove new indices are valid.
  Processed: prove processed region is extended correctly.
  Current: prove accumulator/frontier variables have correct meaning.

Case 2: [other branch]
  Repeat.

Therefore I(new state) holds in every case. □
```

---

## `lp` Maintenance

```text
Assume 1 ≤ i < n.

Case a(i) = a(i-1):
  curlen' = curlen + 1; maxlen' = max(maxlen, curlen'); i' = i + 1.
  The old run extends to a(i), so curlen' is correct.
  maxlen' is the maximum run length in a[0..i').

Case a(i) ≠ a(i-1):
  curlen' = 1; maxlen' = maxlen; i' = i + 1.
  A new run starts at a(i), so curlen' = 1.
  No longer run is created, so maxlen remains correct.
```

---

## Right-to-Left Leaders Maintenance

```text
Assume i ≥ 0.

Case a(i) > curmax:
  b(i) = true; curmax' = a(i); i' = i - 1.
  a(i) is greater than all elements to its right, so b(i) is correct.
  New curmax is max over i ≤ k < n.

Case a(i) ≤ curmax:
  b(i) = false; curmax' = curmax; i' = i - 1.
  a(i) is not greater than all elements to its right.
  curmax remains the max over i ≤ k < n.

Old suffix stays correct; new processed suffix is i' < j < n.
```

---

## Lower-Bound Binary Search Maintenance

```text
m = l + (r-l)/2, so l ≤ m < r.

Case a(m) < v:
  l' = m + 1.
  Sortedness gives ∀k ≤ m. a(k) < v, so new left processed region is correct.

Case a(m) ≥ v:
  r' = m.
  Sortedness gives ∀k ≥ m. a(k) ≥ v, so new right processed region is correct.
```

---

## Rotation Point Maintenance

```text
Invariant: rot ∈ [i,j], closed interval.

Case a(m) ≤ last:
  j' = m.
  Since rot is the first index with value ≤ last, rot ≤ m.
  Therefore rot ∈ [i,m].

Case a(m) > last:
  i' = m + 1.
  m is before the rotation point, so rot > m.
  Therefore rot ∈ [m+1,j].
```

---

# 4. Termination Cheat Sheet

## Template

```text
Define variant V = [non-negative integer expression].

Strict decrease:
  Compute V' after one iteration and show V' < V.

Bounded below:
  Show V ≥ 0.

Therefore the loop terminates because a non-negative integer cannot strictly decrease forever. □
```

---

## Common Variants

| Loop type | Variant |
|---|---|
| Left-to-right `while (i < n)` | `V = n - i` |
| Right-to-left `while (i >= 0)` | `V = i + 1` |
| Half-open binary search `[l,r)` | `V = r - l` |
| Closed rotation search `[i,j]` | `V = j - i` |
| Nested loops | prove inner and outer variants separately |

---

## Left-to-Right Termination

```text
V = n - i.
After body, i' = i + 1.
V' = n - (i+1) = V - 1.
V ≥ 0 because i ≤ n.
Therefore the loop terminates. □
```

## Right-to-Left Termination

```text
V = i + 1.
After body, i' = i - 1.
V' = (i-1) + 1 = i = V - 1.
V ≥ 0 and reaches 0 when i = -1.
Therefore the loop terminates. □
```

## Binary Search Termination

```text
V = r - l or j - i.
Each update shrinks the active interval strictly.
V is a non-negative integer.
Therefore the loop terminates. □
```

---

# Final Exam Checklist

- [ ] Did I give concrete bounds?
- [ ] Did I specify the exact processed prefix/suffix/interval?
- [ ] Did every changed variable appear in the invariant?
- [ ] Did I verify initialisation component by component?
- [ ] Did I split maintenance by branch?
- [ ] Did I prove bounds, processed part, and current state after each branch?
- [ ] Did I define a variant and prove strict decrease?
- [ ] Did I avoid `a(i-1)` when `i = 0`?
- [ ] Did I use closed `[i,j]` for rotation point with `j = m`?

---

*Memorise the templates, then adapt the processed/current clauses to the exact code in the question.*
