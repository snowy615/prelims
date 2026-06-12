# IP-03 — Array Algorithms, Binary Search Variants & Complexity

**Frequency:** the substance of Q2 every year (and parts of Q1). 2025 Q2 = array scanning with invariant (5), spec-driven array construction (4), property checker (3), binary-search failure example (2), modified binary search (6).

---

## 1. Standard scan patterns (know cold, with invariants)

**Running max / visibility scan** — O(n), track `m = max(a[0..i))`.

**Longest run / segment** — track `curlen` (run ending at `i−1`) and `maxlen`.

**Two-pointer / partition** — `while (i < j)`, variant `j − i`.

**Counting / bucket construction** (2025 Q2(a)(ii) `profile`): when the post-condition indexes by *values* `[0..a.max]`, allocate `new Array[Int](a.max + 1)` and fill by a single pass over `a`; running time **O(a.length + a.max)** — say both terms.

**Property checkers** (e.g. `isCAlmostSorted`): nested loop where the inner loop is bounded by the parameter → O(c·n); state the bound and why your loop achieves it.

## 2. Binary search — the canonical code (memorise exactly)

```scala
def binarySearch(a: Array[Int], v: Int): Int = {
  var (i, j) = (0, a.length)
  // I: 0 <= i <= j <= a.length && a[0..i) < v && a[j..a.length) >= v
  while (i < j) {
    val m = i + (j - i) / 2          // avoids overflow; i <= m < j
    if (a(m) < v) i = m + 1 else j = m
  }
  if (i < a.length && a(i) == v) i else -1   // leftmost occurrence
}
```

Talking points that earn marks: the invariant above; variant `j − i`; `m = i + (j−i)/2` avoids overflow; returns **leftmost** occurrence; O(log n) because the interval halves.

## 3. "Binary search on a perturbed array" template (the 2025 twist)

Pattern: array satisfies a weakened sortedness property (c-almost sorted, rotated, two interleaved sorted sequences). Template answer:

1. **Failure example (2 marks):** give a concrete small array + value where vanilla binary search commits to the wrong half; trace the output.
2. **Repair (6 marks):** decompose the array into structures that *are* sorted (e.g. a 2-almost sorted array = two interleaved sorted subsequences at even/odd indices; a rotated array = two sorted blocks split at the pivot, found by binary search on `a(m) ≥ a(0)`), binary-search each, combine (min of found indices).
3. **Justify:** correctness via the decomposition property + running time O(log n) = constant number of binary searches.

## 4. Sorting facts to quote

- Insertion sort O(n²) worst, O(n) on nearly-sorted input; quicksort O(n log n) average / O(n²) worst (sorted input, bad pivot); mergesort O(n log n) always, O(n) extra space.
- Partition step: invariant `a[l..i) ≤ p ∧ a[j..r) > p`.
- Recurrences: `T(n) = 2T(n/2) + O(n) ⇒ O(n log n)`; `T(n) = T(n−1) + O(n) ⇒ O(n²)`; `T(n) = T(n/2) + O(1) ⇒ O(log n)`. State the recurrence, then its solution.

## 5. Complexity answer template

> The while loop executes at most n iterations (variant n − i), each doing O(1) work, so the running time is O(n). [If recursive:] The running time satisfies T(n) = ⟨recurrence⟩, which solves to O(...). This meets the bound required for full marks.

Always tie the bound to a *variant or recurrence*, never just assert it.

## 6. Mark-scheme checklist

- [ ] Code compiles in your head: `a(i)` not `a[i]`, `Array[Int]`, `var`/`val` correct.
- [ ] While-loops unless the question explicitly permits for-loops (2025 said "you may use for-loops" only in specific parts).
- [ ] Invariant for any loop you write, if asked — including all your variables.
- [ ] Running time stated **and justified**; check the question's target bound ("full marks for O(log n)").
- [ ] Failure examples are concrete and traced.
