# IP-04 — ADT Specification & Implementation (abstract state, DTI, Abs, floating point)

**Frequency:** 28 past questions; the whole of 2025 Q3 (20 marks). Q3 is compulsory — this is the highest-value template in the folder.

---

## 1. The vocabulary (definitions worth ~1 mark each)

- **Abstract state space:** the mathematical object the ADT represents, written as a comment, e.g. `// state: book : Name ⇸ Number` (partial map), `// state: p_X : Int ⇸ [0,1]`, `// state: s : seq Int`.
- **Datatype invariant (DTI):** a property of the abstract state that holds before and after *every* operation, e.g. `// DTI: Σ_c p_X(c) = 1 ∧ ∀c. p_X(c) > 0` or `// DTI: s contains no duplicates`.
- **Pre/post-conditions on operations:** post relates new abstract state to old (`p_X = p_X0`); use subscript-0 convention for initial values; `returns ...` for the result.
- **Initial state:** the abstract value after construction. *Know the 2025 trap:* "Is it possible to meaningfully specify the initial state?" — if the DTI forces a nontrivial constraint (probabilities sum to 1), there is **no natural canonical initial value** (an "empty" distribution violates the DTI), so either no meaningful initial state exists or one must pick an arbitrary one (e.g. point mass at 0) — *justify whichever you claim*.
- **Concrete state space:** the actual fields of the implementing class.
- **Abstraction function (Abs):** maps concrete state to abstract state, written `// Abs: p_X(c) = probs.getOrElse(c, 0.0)` or `// Abs: book = { (e.name, e.number) | e is a node reachable from buckets(hash(name)) }`. Every implementation question expects this comment.

## 2. The "complete the trait" template (2025 Q3(a) — 6 marks)

```scala
// state: p_X : Int -> [0,1]     (a discrete probability distribution)
// DTI: sum over all c of p_X(c) = 1, and p_X(c) >= 0 for all c
trait IntPD {
  // post: p_X = p_X0 and returns p_X(outcome)
  def probability(outcome: Int): Double

  // post: p_X(c) = sum over a,b with a+b=c of p_X0(a) * (pd2.p_Y)(b)
  def add(pd2: IntPD): Unit

  // post: p_X(c) = sum over a,b with a*b=c of p_X0(a) * (pd2.p_Y)(b)
  def multiply(pd2: IntPD): Unit

  // post: p_X = p_X0 && returns array of exactly the pairs (c, p_X(c)) with p_X(c) > 0
  def entriesAsArray: Array[(Int, Double)]
}
```

Rules: a **mutating** operation's post mentions how the state changed; a **query**'s post asserts the state is unchanged (`p_X = p_X0`) and describes the return value; the DTI is stated once, not per-operation.

## 3. The "implement with mutable.Map" template (6 marks)

```scala
import scala.collection.mutable

class MapIntPD private (var probs: mutable.Map[Int, Double]) extends IntPD {
  // Abs: p_X(c) = probs(c) if c in probs.keys, else 0
  // (concrete DTI: values positive, sum to 1)

  def probability(outcome: Int): Double = probs.getOrElse(outcome, 0.0)

  def add(pd2: IntPD): Unit = {
    val result = mutable.Map.empty[Int, Double].withDefaultValue(0.0)
    for ((a, pa) <- probs; (b, pb) <- pd2.entriesAsArray)
      result(a + b) = result(a + b) + pa * pb
    probs = result
  }
  // multiply: identical with a * b
}
```

Key idioms: build a *fresh* map then swap (mutating in place corrupts the inputs you still read); iterate the second distribution via its public `entriesAsArray` (you only know the trait); `withDefaultValue` for accumulation.

## 4. Companion-object factory template (4 marks)

```scala
object MapIntPD {
  def UniformPD(left: Int, right: Int): MapIntPD = {
    assert(left < right)                       // guarantees a genuine distribution
    val m = mutable.Map.empty[Int, Double]
    for (k <- left until right) m(k) = 1.0 / (right - left)
    new MapIntPD(m)
  }
  def BernoulliPD(v1: Int, v2: Int, pv1: Double): MapIntPD = {
    assert(0.0 <= pv1 && pv1 <= 1.0 && v1 != v2)
    new MapIntPD(mutable.Map(v1 -> pv1, v2 -> (1.0 - pv1)))
  }
}
```

Note "Notice the return type" = factories return the *concrete* class; constructor is `private` so the factories are the only entry points, which lets them enforce the DTI with `assert`.

## 5. Floating-point problems template (2025 Q3(d) — 4 marks; recurring since 2010)

Name two of, with the fix:

1. **Representation/rounding error:** 1/6 is not representable in binary; repeated addition of probabilities makes the sum ≠ 1.0, breaking the DTI. *Fix:* store exact rationals (pair of `BigInt`s / numerator-denominator), or normalise after operations.
2. **Equality comparison fails:** `sum == 1.0` is unreliable. *Fix:* compare with a tolerance `math.abs(sum - 1.0) < eps`.
3. **Accumulated error / cancellation:** many tiny products summed in different orders give different results; subtracting near-equal values loses precision. *Fix:* Kahan summation, summing smallest-first, or exact arithmetic.
4. **Underflow:** products of many small probabilities underflow to 0. *Fix:* work with logarithms of probabilities.

## 6. Mark-scheme checklist

- [ ] `state:`, `DTI:`, `Abs:` comments all present and in the expected notation.
- [ ] Posts use the `x0` convention and `returns`.
- [ ] Queries assert state unchanged.
- [ ] Initial-state question answered **with justification**.
- [ ] Factories `assert` the DTI; private constructor remark.
- [ ] Two FP problems + concrete avoidance each.
