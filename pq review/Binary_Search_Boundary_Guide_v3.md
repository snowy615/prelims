# Binary Search Boundary Guide — CS2 Exam Reference

> **Purpose**: Eliminate confusion about `l`, `r`, `m` updates in binary search variants. One correct pattern, two natural extensions.

---

## 1. The One Pattern to Memorise: Left-Closed, Right-Open `[l, r)`

```scala
def binarySearch(a: Array[Int], v: Int): Int = {
  // Pre: a is sorted in strictly ascending order
  var (l, r) = (0, a.length)      // interval [l, r) — r is EXCLUDED

  while (l < r) {                   // terminates when [l, l) = empty
    val m = l + (r - l) / 2         // m is always inside [l, r)

    if (a(m) < v) l = m + 1        // a[m] is too small; m itself is excluded
    else           r = m           // a[m] is candidate; keep m in interval
  }

  // l == r. Check if we actually found v.
  if (l < a.length && a(l) == v) l else -1
}
```

### Why this works

| Component | Meaning |
|-----------|---------|
| `[l, r)` | `l` is the first index that **could** contain `v`; `r` is the first index that **cannot** |
| `l < r` | Loop while there is at least one candidate |
| `m = l + (r-l)/2` | Middle index, always valid because `l < r` guarantees `l <= m < r` |
| `a(m) < v => l = m+1` | Everything at `m` and left is too small; next candidate starts at `m+1` |
| `a(m) >= v => r = m` | `m` might be the answer; do not exclude it |

### The Invariant (Exam Write-Up)

```text
// Invariant: 0 ≤ l ≤ r ≤ a.length
//            ∧ (if v ∈ a then v ∈ a[l..r))
//            ∧ a[0..l) < v ≤ a[r..a.length)
```

**Termination**: When `l == r`, the interval is empty. By the invariant, if `v` were in `a`, it would be in `a[l..l)` — impossible. So either `a(l) == v` (found) or `v` is absent.

---

## 2. Two Natural Extensions (Same Pattern, Different Goal)

### Extension A: Find Leftmost Occurrence

Already shown above. Returns **first** index where `a(i) == v`.

```scala
// Post: returns min { i | a(i) = v } or -1
```

### Extension B: Find Rightmost Occurrence

```scala
def binarySearchRight(a: Array[Int], v: Int): Int = {
  var (l, r) = (0, a.length)      // same interval, same loop

  while (l < r) {
    val m = l + (r - l) / 2

    if (a(m) <= v) l = m + 1       // CHANGE: <= instead of <
    else           r = m           // same
  }

  // l == r. l is the first index STRICTLY greater than v.
  // So a(l-1) is the last occurrence of v, if it exists.
  if (l > 0 && a(l - 1) == v) l - 1 else -1
}
```

| | Leftmost | Rightmost |
|--|----------|-----------|
| **Condition** | `a(m) < v` | `a(m) <= v` |
| **Update** | `l = m + 1` | `l = m + 1` |
| **Otherwise** | `r = m` | `r = m` |
| **Post-check** | `a(l) == v` | `a(l-1) == v` |
| **Intuition** | Shrink from left, keep first candidate | Shrink from left, skip past all equals |

---

## 3. The Double-Closed `[l, r]` Pattern: More Error-Prone

Many textbooks use this. **Prefer the half-open form** in exams — it has fewer edge cases.

```scala
// ⚠️ More error-prone — double-closed interval
var (l, r) = (0, a.length - 1)
while (l <= r) {           // <= is easy to mess up
  val m = (l + r) / 2      // overflow risk
  if (a(m) < v) l = m + 1
  else if (a(m) > v) r = m - 1
  else return m            // found — but what if you need leftmost?
}
-1  // not found
```

| Problem | Why it hurts |
|---------|-------------|
| `l <= r` vs `l < r` | Off-by-one in termination |
| `r = m - 1` | Excludes `m`, but what if `m` is the leftmost? |
| `(l + r) / 2` | Integer overflow on large arrays |
| Found early return | Hard to adapt for "first/last occurrence" |

---

## 4. Rotation Point: Reuse the Same Pattern

Finding the rotation point (smallest element) in a rotated sorted array:

```scala
def findRotationPoint(a: Array[Int]): Int = {
  if (a.length == 0) return 0     // guard: empty array has no rotation point
  val last = a(a.length - 1)
  var (l, r) = (0, a.length)      // [l, r)

  while (l < r) {
    val m = l + (r - l) / 2

    if (a(m) > last) l = m + 1     // m is in "big" left segment; exclude
    else           r = m           // m is in "small" right segment; keep
  }

  l   // index of smallest element
}

> **Precondition for the rotation-point algorithm**: The input array must contain **distinct elements**. If duplicates exist (e.g., `Array(2, 2, 2, 0, 2)` which is a rotation of `Array(0, 2, 2, 2, 2)`), the comparison `a(m) > last` fails to distinguish the "big" left segment from the "small" right segment, and the algorithm may return an incorrect index.
```

| | Binary Search (value) | Rotation Point (structure) |
|--|----------------------|---------------------------|
| **What we compare** | `a(m)` vs `v` | `a(m)` vs `last` |
| **Left segment** | `a[0..l) < v` | `a[0..l) > last` (big elements) |
| **Right segment** | `a[r..n) >= v` | `a[r..n) <= last` (small elements) |
| **Goal** | Find `v` | Find boundary between big and small |

---

## 5. Exam Write-Up Template

### For any binary search variant, always state:

```text
// Pre: a is sorted ascending (value search) / a is a rotated-sorted array with distinct elements (rotation point)
// Interval: [l, r) where l is inclusive, r is exclusive
// Invariant: [state what is known about a[0..l) and a[r..n)]
// Variant: r - l (decreases strictly, bounded below by 0)
// Termination: l == r, interval empty
```

### Example: Rotation Point

```text
// Invariant: 0 ≤ l ≤ r ≤ a.length
//            ∧ a[0..l) are all > a(a.length-1)     (left of rotation)
//            ∧ a[r..a.length) are all ≤ a(a.length-1)  (right of rotation, including min)
//            ∧ the rotation point is in [l, r)
```

---

## 6. Quick Decision Tree

```
What do you need?
│
├─ Find exact value, any occurrence ──→ Double-closed [l,r] with early return (textbook style)
│                                        ⚠️ Easy to get wrong
│
├─ Find leftmost occurrence ──────────→ [l, r) with a(m) < v → l = m+1, else r = m
│                                        ✅ Exam-safe
│
├─ Find rightmost occurrence ─────────→ [l, r) with a(m) <= v → l = m+1, else r = m
│                                        ✅ Exam-safe
│
└─ Find rotation point / boundary ────→ [l, r) with a(m) > last → l = m+1, else r = m
                                         ✅ Exam-safe
```

---

## 7. Error Checklist

Before writing binary search in an exam:

- [ ] Did I use `[l, r)` with `while (l < r)`?
- [ ] Did I use `m = l + (r-l)/2` (not `(l+r)/2`)? — all code in this guide already uses the overflow-safe form.
- [ ] Is the update **exclusive** on one side (`l = m+1`) and **inclusive** on the other (`r = m`)?
- [ ] After termination, did I verify the result (not assume `a(l) == v`)?
- [ ] For rotation point: did I compare with `a(a.length-1)` (not `a(0)`)?

---

*Pattern: [l, r), m = l + (r-l)/2, one side excludes m, one side keeps m. Everything else is a special case of this.*
