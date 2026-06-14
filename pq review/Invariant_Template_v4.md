# Loop Invariant & Formal Specification Template

> **Purpose**: Standardised mathematical formulations for pre-conditions, post-conditions, loop invariants, and termination proofs in CS2 exams.
> **Focus**: Precision over creativity — use these templates to avoid vague descriptions that lose marks.

---

## 1. Pre-Condition Patterns

### 1.1 Array-Processing Functions

| Situation | Formal Pre-Condition | Scala `require` |
|-----------|---------------------|-----------------|
| Any array | `a` is a valid `Array[Int]` | `require(a != null)` (implied) |
| Non-empty | `a.length > 0` | `require(a.length > 0)` |
| Non-negative elements | `∀i ∈ [0, a.length). a(i) ≥ 0` | `require(a.forall(_ >= 0))` |
| Sorted input | `∀i ∈ [0, a.length-1). a(i) ≤ a(i+1)` | (assert in tests) |
| Index parameter | `0 ≤ i < a.length` | `require(i >= 0 && i < a.length)` |
| Range parameter | `0 ≤ k < a.length` | `require(k >= 0 && k < a.length)` |
| Segment fits | `0 ≤ i ≤ a.length - m` | `require(i >= 0 && i + m <= a.length)` |

### 1.2 Exam Write-Up Template

```text
// Pre: a.length ≥ 0  (a is any integer array; no further constraints required)
// Pre: a.length > 0 ∧ ∀i. a(i) ≥ 0
// Pre: 0 ≤ i < a.length ∧ 0 ≤ m ≤ a.length - i
```

**Pitfall to avoid**: Never write "a is a valid list of Array[Int]" — this is meaningless. Specify concrete constraints.

---

## 2. Post-Condition Patterns

### 2.1 Set-Comprehension Style (Preferred)

Describe the return value as a set, maximum, or predicate over indices.

| Function | Vague (loses marks) | Precise (full marks) |
|----------|--------------------|---------------------|
| `lp(a)` | "returns the max of curlen" | `returns max { j-i+1 \| 0 ≤ i ≤ j < n ∧ a(i)=a(i+1)=...=a(j) }`, or `0` if `n=0` |
| `leaders(a)` | "returns true if element is a leader" | `returns b` where `b(i) = true ⟺ ∀j > i. a(i) > a(j)` |
| `lastIndex(a)` | "returns the rightmost index" | `returns b` where `b(v) = max { i \| a(i) = v }` or `-1` if `{i \| a(i) = v} = ∅` |
| `binarySearch(a,v)` | "returns the index of v" | `returns min { i \| a(i) = v }` if `v ∈ a`, else `-1` |
| `isKRotatedSorted(a,k)` | "checks if array is rotated" | `returns true ⟺ ∃ sorted c. a = c[k..n) ++ c[0..k)` |

### 2.2 Post-Condition with Frame Condition

For mutating methods (e.g., `add`, `multiply`), specify what does **not** change:

```text
// Post: p = p₀ + p₂  ∧  p₂ is unchanged  ∧  all other objects unchanged
// Post: p = p₀ · p₂  ∧  p₂ is unchanged
```

---

## 3. Loop Invariant — Standard Structure

### 3.1 The Four-Part Template

Every invariant should contain:

```
Invariant :=  (Bounds)  ∧  (Processed Correct)  ∧  (Current State)  ∧  (Remaining Work)
```

| Component | Meaning | Example (lp) |
|-----------|---------|-------------|
| **Bounds** | Range of loop variable | `1 ≤ i ≤ a.length` |
| **Processed Correct** | Result is correct for prefix already scanned | `maxlen = max run length in a[0..i)` |
| **Current State** | Meaning of variables at position `i-1` | `curlen = run length ending at a(i-1)` |
| **Remaining Work** | What the loop still needs to do | `maxlen` may need update from `a[i..n)` |

### 3.2 Array-Traversal Invariants (Left to Right)

**Template for `lp(a)` — Longest Consecutive Run:**

```text
// Invariant: 1 ≤ i ≤ a.length
//            ∧ maxlen = max { run-length of consecutive equal elements in a[0..i) }
//            ∧ curlen = length of consecutive run ending at a(i-1)
//            ∧ a(i-1) is the last element of that run
```

**Template for `lastIndex` — Single Pass:**

```text
// Invariant: 0 ≤ i ≤ a.length
//            ∧ ∀v. b(v) = max { j < i | a(j) = v }  or  -1 if no such j exists
```

### 3.3 Right-to-Left Scan Invariants (Corrected)

**Template for `leaders(a)` — Right-to-Left Scan:**

```text
// Invariant: -1 ≤ i < a.length
//            ∧ curmax = max { a(k) | i < k < a.length }
//            ∧ ∀j ∈ (i..a.length). b(j) = true ⟺ a(j) > a(k) for all k > j
//            ∧ b[i+1..a.length) is correctly assigned
```

**Critical correction**: Since `i` decrements from `n-1` down to `-1`, the **processed suffix** is `b[i+1..n)`, not `b[0..i+1)`. The left portion `b[0..i+1)` remains unprocessed garbage until `i` reaches `-1`.

### 3.4 Binary Search Invariants

**Template for standard binary search:**

```text
// Invariant: 0 ≤ i ≤ j ≤ a.length
//            ∧ (if v ∈ a then v ∈ a[i..j))
//            ∧ a[0..i) < v ≤ a[j..a.length)
```

**Template for finding rotation point (left-closed, right-open):**

```text
// Invariant: 0 ≤ i ≤ j ≤ a.length
//            ∧ the rotation point (index of the minimum) is in a[i..j)
//            ∧ a[0..i)         are all  > a(a.length-1)   (so a(i-1) > last when i > 0)
//            ∧ a[j..a.length)  are all  ≤ a(a.length-1)   (so a(j)   ≤ last when j < n)
```

**Note on interval choice**: The left-closed, right-open interval `[i, j)` is standard for binary search. When `a(m) <= last`, we set `j = m` (keeping `m` in the interval), not `j = m+1` (which would exclude `m`). The loop terminates with `i == j`, returning `i` as the rotation point. This is correct because the interval shrinks but never excludes a valid candidate until convergence.

---

## 4. Invariant Proof — Three-Step Template

> The remaining 1 mark is for stating the invariant formally (see Section 3).

### Step 1: Initialization (1/5 marks)

**Goal**: Show invariant holds before first iteration.

**Template**:
> "Before the loop executes, `[variable] = [initial value]`. The processed portion `[prefix]` contains `[description]`. Therefore `[component 1]`, `[component 2]`, and `[component 3]` hold trivially. The invariant is satisfied."

**Example for `lp` (with empty-array guard):**
> "If `a.length = 0`, the function returns `0` immediately by the guard clause, satisfying the post-condition. Otherwise, `a.length > 0` and we enter the loop with `i = 1`, `maxlen = 1`, `curlen = 1`. The processed prefix is `a[0..1) = {a(0)}`. The longest (and only) consecutive run has length 1. The run ending at `a(0)` has length 1. Thus `maxlen = 1` and `curlen = 1` satisfy the invariant."

### Step 2: Maintenance (2/5 marks)

**Goal**: Assume invariant holds at start of iteration; prove it holds at end.

**Template**:
> "Assume the invariant holds at the start of an iteration with current `i`. We analyse the iteration body:
> 
> **Case 1**: `[condition for branch 1]`. Then `[variable updates]`. By the invariant hypothesis, `[property of prefix]`. After the update, `[property of extended prefix]`. Thus the invariant holds for `i+1`.
> 
> **Case 2**: `[condition for branch 2]`. Then `[variable updates]`. ..."

**Example for `lp`:**
> "Assume the invariant holds at iteration `i` (where `1 ≤ i < a.length`).
> 
> **Case 1**: `a(i) = a(i-1)`. The run ending at `a(i-1)` extends to `a(i)`. The new `curlen` becomes `curlen + 1`. If this exceeds `maxlen`, `maxlen` is updated. All runs in `a[0..i)` are unchanged except the last, which is now longer. The invariant holds for `i+1`.
> 
> **Case 2**: `a(i) ≠ a(i-1)`. The previous run ends. A new run of length 1 starts at `a(i)`. `curlen` is reset to 1. `maxlen` remains the maximum over `a[0..i)`. The invariant holds for `i+1`."

### Step 3: Termination + Correctness (1/5 marks)

**Goal**: Use invariant + termination condition to derive post-condition.

**Template**:
> "The loop terminates when `[termination condition]`. At this point, by the invariant, `[processed = entire input]`. Therefore `[variable]` equals `[desired result]`. The post-condition `[formal post]` is satisfied."

**Example for `lp`:**
> "The loop terminates when `i = a.length`. By the invariant, `maxlen` is the maximum length of any consecutive equal run in `a[0..a.length) = a`. The post-condition — that `maxlen` equals the length of the longest subarray of equal elements — is satisfied. The function returns `maxlen`."

---

## 5. Loop Variant Template

### 5.1 Required Properties

A valid loop variant `V` must satisfy:
1. `V ≥ 0` at loop start (non-negative)
2. `V` strictly decreases each iteration (progress)
3. `V = 0` implies loop condition is false (termination)

### 5.2 Common Variants

| Loop Structure | Variant | Justification |
|---------------|---------|---------------|
| `while (i < n) { i += 1 }` | `n - i` | `i` increases by 1, so `n-i` decreases by 1 |
| `while (i >= 0) { i -= 1 }` | `i + 1` | `i` decreases by 1, bounded below by -1 |
| `while (i < j) { ... }` | `j - i` | interval shrinks each iteration |
| Nested loops | `outer + inner` or product | analyse combined progress |

### 5.4 Variant + Invariant Together

| Question | Use Variant | Use Invariant |
|----------|-------------|---------------|
| "Prove the loop terminates" | ✅ Yes | ❌ No |
| "Prove the result is correct" | ❌ No | ✅ Yes |
| "Prove array indices are valid" | ❌ No | ✅ Yes (bounds in invariant) |

**Critical exam trap**: Using the invariant to argue termination = **0 marks**. The variant must be stated and justified independently. Conversely, using the variant to argue correctness = **0 marks** — only the invariant connects loop state to post-condition.

**Q1 structure (typical 11 marks):**
- (a) Variant: state + justify termination → 3 marks
- (c) Invariant: state + prove init + maintenance + termination/correctness → 5 marks

### 5.3 Exam Write-Up

```text
// Variant: a.length - i
// - Initially i = 1, so variant = a.length - 1 ≥ 0 (since a.length ≥ 1 in loop)
// - Each iteration i increases by 1, so variant decreases by 1
// - When variant = 0, i = a.length, loop condition i < a.length fails
// - Therefore loop terminates
```

---

## 6. Data Type Invariant (DTI) for Classes

### 6.1 Template Structure

For a class representing abstract data (e.g., `MapPoly`):

```text
// Abstract State: p(x) = Σ_{e ∈ keys} coeffs(e) · x^e
//
// DTI:
//   1. ∀e ∈ coeffs.keys. coeffs(e) ≠ 0.0   (no zero coefficients stored)
//   2. ∀e ∈ coeffs.keys. e ≥ 0              (non-negative exponents)
```

**Note**: The third property "no duplicate keys" is guaranteed by the `Map` data structure itself and need not be stated explicitly in the DTI.

**Common pitfall — implementation detail in DTI:**
```text
// ❌ Wrong: DTI describes the representation, not the abstract constraint
// DTI: coeffs is a mutable.Map[Int, Double]

// ✅ Correct: DTI describes the mathematical property
// DTI: ∀e ∈ coeffs.keys. coeffs(e) ≠ 0.0 ∧ e ≥ 0
```

### 6.2 Abstraction Function

```text
// Abs: coeffs represents polynomial p where
//      p(x) = Σ_{(e,c) ∈ coeffs} c · x^e
//
// Rep Invariant: ∀(e,c) ∈ coeffs. c ≠ 0 ∧ e ≥ 0
```

---

## 7. Common Pitfalls in Formulation

| Vague (loses marks) | Precise (full marks) |
|--------------------|---------------------|
| "a is a valid list" | `a.length ≥ 0 ∧ a ≠ null` |
| "returns the max" | `returns max { ... \| ... }` |
| "curlen is the current length" | `curlen = length of run ending at a(i-1)` |
| "maxlen stores the maximum" | `maxlen = max { run-length in a[0..i) }` |
| "the loop works correctly" | "Invariant: [formal statement]. Proof: [three steps]" |
| "i increases so it terminates" | "Variant: `n-i`, decreases by 1, bounded below by 0" |

---

## 8. Quick Reference: Invariant by Algorithm Type

### 8.1 Accumulation (sum, max, count)

```text
// Invariant: 0 ≤ i ≤ n
//            ∧ acc = f(a[0..i))   where f is the accumulation function
```

### 8.2 Search (find, indexOf)

```text
// Invariant: 0 ≤ i ≤ n
//            ∧ target ∉ a[0..i)   (if searching left-to-right)
//            ∧ target ∉ a[i+1..n)   (if searching right-to-left, adjust)
```

### 8.3 Two-Pointer / Sliding Window

```text
// Invariant: 0 ≤ left ≤ right ≤ n
//            ∧ [property holds for window a[left..right)]
//            ∧ [property is optimal/maximal/minimal among all valid windows]
```

### 8.4 Partition (quickselect, Dutch national flag)

```text
// Invariant: 0 ≤ lt ≤ i ≤ gt ≤ n
//            ∧ a[0..lt)  < pivot
//            ∧ a[lt..i)  = pivot
//            ∧ a[i..gt)  unprocessed
//            ∧ a[gt..n)  > pivot
```

---

## 9.0 Invariant Self-Check

Before submitting an invariant answer, verify:

- [ ] **Bounds**: Does the invariant specify the range of every loop variable? (worth ~1 mark)
- [ ] **Processed portion**: Is the result correct for the prefix already scanned?
- [ ] **Current state**: Does every mutable variable have a precise meaning?
- [ ] **Empty/boundary cases**: Does the invariant hold when the array is empty or has 1 element?
- [ ] **Formal notation**: Are you using set-builder notation `{ ... | ... }` instead of vague English?

**Rule of thumb**: If a variable appears in the loop body but not in the invariant, its correctness has not been proven.

## 9. Exam Strategy: Invariant Questions

### Typical Mark Distribution (5-mark invariant question)

| Component | Marks | Time Budget |
|-----------|-------|-------------|
| State invariant formally | 1 | 2 min |
| Prove initialization | 1 | 2 min |
| Prove maintenance (all cases) | 2 | 5 min |
| Prove termination + correctness | 1 | 2 min |
| **Total** | **5** | **11 min** |

### If Stuck

1. **Write the bounds first** — always 1 mark for `0 ≤ i ≤ n`
2. **Describe in English** before formalising — partial credit for correct intuition
3. **Use the post-condition** — work backwards: what must be true when loop ends?
4. **Check all variables** — invariant must mention every variable that changes

---

## 10. Changelog from Previous Version

| Issue | Original | Corrected |
|-------|----------|-----------|
| leaders invariant direction | `b[0..i+1) is correctly assigned` | `b[i+1..a.length) is correctly assigned` |
| lp initialization | Missing empty-array guard | Added explicit empty-array case handling |
| DTI redundancy | Listed "no duplicate keys" as required | Removed — guaranteed by Map structure |

---

*Template for CS2 Mock 2026. Invariant precision is the single biggest differentiator between 2/5 and 5/5.*
