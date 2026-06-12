# IP-01 — Loop Invariants, Variants & Program Correctness

**Frequency:** the single most examined IP topic (59 past questions; 11/20 marks of 2025 Q1). Appears in Q1 nearly guaranteed.

---

## 1. Definitions you must write verbatim

**Loop invariant.** A predicate over the program variables that holds immediately before the loop, is preserved by every iteration (if it holds and the guard holds before an iteration, it holds after), and therefore holds on exit; combined with the negation of the guard it establishes the post-condition.

**Loop variant.** An integer expression in the program variables that (i) is **bounded below** (typically ≥ 0 while the guard holds), and (ii) **strictly decreases** on every iteration. Its existence proves the loop **terminates**.

**Purpose template (3-mark answer):**
> Loop variants are used to prove termination. A variant is an integer expression bounded below that strictly decreases with each iteration; since a bounded-below integer cannot decrease forever, the loop must exit after finitely many iterations. For this loop, `a.length - i` is a variant: the guard guarantees `i < a.length` so it is positive, and `i += 1` at the end of the body strictly decreases it.

**Pre/post-condition.** Pre: what the caller must guarantee (e.g. `a` is non-null, sorted, non-empty). Post: relation between the result and the (initial) inputs; write `a = a0` if the array is unmodified.

## 2. The standard invariant-proof template (5 marks)

For a loop computing something over `a[0..n)` with counter `i`:

1. **State I.** E.g. for "longest increasing contiguous segment ending logic":
   `I: 1 ≤ i ≤ a.length ∧ curlen = (length of the longest increasing run ending at position i−1) ∧ maxlen = (length of the longest increasing run within a[0..i))`
   **Always include the range bounds on `i` and a clause for every variable the question names.**
2. **Initialisation.** Before the loop `i = 1, curlen = 1, maxlen = 1`: the only run in `a[0..1)` has length 1. ✓
3. **Preservation.** Assume I and guard `i < a.length`. Case split on the body's branch condition (e.g. `a(i) > a(i−1)` vs not); show each branch re-establishes I with `i+1` in place of `i`. ✓
4. **Conclusion.** On exit `¬guard ∧ I` gives `i = a.length`, hence `maxlen` = longest run in the whole array = the claimed post-condition.

**Pitfalls:** an invariant like `i > 0 ∧ i ≤ 10` may fail to hold *after the final increment* (e.g. exit with `i = 11`) — check the exit value, not just inside the loop (this exact trap was 2023 Q1(b)). An invariant must also be *strong enough*: `true` is an invariant but proves nothing.

## 3. "Write the function AND its invariant" template

When asked to write code + invariant (5-mark format, 2025 Q2(a)(i)):

```scala
def visibility(a: Array[Int]): Array[Boolean] = {
  val b = new Array[Boolean](a.length)
  if (a.length > 0) b(0) = false        // or per the definition
  var m = if (a.length > 0) a(0) else 0 // running maximum
  var i = 1
  // I: 1 <= i <= a.length && m = max(a[0..i)) &&
  //    for all j in [0,i): b(j) = (a(j) > max of a[0..j))
  while (i < a.length) {
    b(i) = a(i) > m
    if (a(i) > m) m = a(i)
    i += 1
  }
  b
}
```

Then add one sentence: *"On exit i = a.length, so by the invariant every b(j) is correctly set; hence the post-condition holds."* Include **every variable you introduced** in the invariant — the question says so and marks are docked otherwise.

## 4. Variant phrasing bank

- `while (i < n)` with `i += 1` → variant `n − i`.
- `while (i < j)` two-pointer → variant `j − i`.
- Binary search → variant `j − i` (halves each iteration).
- Loop on linked list → variant = number of nodes from current to end.
- Repeated halving `n /= 2` → variant `n`.

## 5. Mark-scheme checklist

- [ ] Invariant includes bounds on the counter (`0 ≤ i ≤ n`).
- [ ] Invariant mentions *all* named variables.
- [ ] Initialisation, preservation (with case split), exit argument all present.
- [ ] Variant: bounded below + strictly decreasing + one-line justification.
- [ ] Pre/post written as Scala-style comments: `// pre: ...  // post: a = a0 && returns ...`.
