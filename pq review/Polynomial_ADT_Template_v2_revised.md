# Polynomial ADT — CS2 Exam Template

> **Purpose**: Complete reference for Q3 (Imperative Programming Part 2), worth 20 marks. Covers trait specification, `mutable.Map` implementation, companion object factories, and floating-point pitfalls.

---

## 1. Trait Specification (Q3a) — 6 marks

### 1.1 Abstract State Space

```text
// State: p ∈ ℝ[X], equivalently a finite-support function p: ℕ → ℝ
//        where p(e) is the coefficient of x^e
//        and p(e) = 0.0 for all but finitely many e
```

**Important distinction**: The abstract state is the mathematical polynomial, not the data structure used to store it. A `Map[Int, Double]` is one possible **implementation representation**, so it belongs in the abstraction function / representation invariant, not in the abstract state.

### 1.2 Abstract Data Type Invariant (DTI)

```text
// Abstract DTI:
//   1. Only non-negative integer exponents are allowed: e ∈ ℕ
//   2. The polynomial has finite support: only finitely many e have p(e) ≠ 0.0
```

For a `Map` implementation, the representation invariant will additionally require that no near-zero coefficients are stored.

### 1.3 Method Postconditions

```scala
trait Poly {
  // Pre: e ≥ 0
  // Post: p = p₀  ∧  returns p₀(e), the coefficient of x^e in p₀
  def coefficient(e: Int): Double

  // Pre: p2 is a valid Poly
  // Post: p = p₀ + p₂₀
  //       any argument object not aliased with this is unchanged
  def add(p2: Poly): Unit

  // Pre: p2 is a valid Poly
  // Post: p = p₀ · p₂₀
  //       any argument object not aliased with this is unchanged
  def multiply(p2: Poly): Unit

  // Post: p = p₀  ∧  returns Array[(Int, Double)] of all non-zero coefficients
  //       sorted by exponent in ascending order
  def coefficientsAsArray: Array[(Int, Double)]
}
```

**Aliasing note**: If `p2` is the same object as `this`, then `p2` cannot remain unchanged because it is exactly the object being mutated. The postconditions therefore use `p₂₀`, the value of `p2` before the call, and state the frame condition only for argument objects not aliased with `this`.

### 1.4 Initial State

> **Yes**, the initial state can be specified: the zero polynomial. In the map representation, this is represented by an empty coefficient map with no entries. All methods must preserve the abstract DTI and the representation invariant.

---

## 2. MapPoly Implementation (Q3b) — 6 marks

### 2.1 Class Skeleton

```scala
import scala.collection.mutable

class MapPoly private (var coeffs: mutable.Map[Int, Double]) extends Poly {
  import MapPoly.Eps   // use the same epsilon as companion object

  // Abs: coeffs represents the polynomial p where
  //      p(e) = coeffs(e) if e ∈ coeffs.keys
  //      p(e) = 0.0       if e ∉ coeffs.keys
  //
  // Representation invariant:
  //   1. ∀e ∈ coeffs.keys. e ≥ 0
  //   2. ∀e ∈ coeffs.keys. math.abs(coeffs(e)) >= Eps
  //      (no near-zero coefficients are stored)
```

**Note**: "No duplicate keys" is guaranteed by the `Map` data structure and need not be stated as a separate invariant.

### 2.2 coefficient

```scala
  def coefficient(e: Int): Double = {
    require(e >= 0, "Exponent must be non-negative")
    coeffs.getOrElse(e, 0.0)
  }
```

**Key insight**: The implementation enforces the method precondition with `require`. If the exponent is valid but not stored, the coefficient is zero.

### 2.3 add — In-Place Addition

```scala
  def add(p2: Poly): Unit = {
    val terms2 = p2.coefficientsAsArray   // snapshot, also safe when p2 aliases this

    for ((e, c) <- terms2) {
      val newCoeff = coefficient(e) + c
      if (math.abs(newCoeff) < Eps) coeffs -= e   // maintain RI: remove near-zero coefficients
      else coeffs(e) = newCoeff
    }
  }
```

**Key insight**: Iterate over `p2`'s non-zero coefficients only. For each exponent, add to this polynomial. If the result is near zero, remove the key to maintain the representation invariant.

### 2.4 multiply — The Critical Method

```scala
  def multiply(p2: Poly): Unit = {
    val terms1 = coeffs.toArray            // snapshot of this polynomial
    val terms2 = p2.coefficientsAsArray    // snapshot of p2, safe even if p2 == this
    val res = mutable.Map.empty[Int, Double].withDefaultValue(0.0)

    for ((d1, c1) <- terms1; (d2, c2) <- terms2) {
      res(d1 + d2) = res(d1 + d2) + c1 * c2   // exponent adds, coefficient multiplies
    }

    // Remove near-zeros to maintain the representation invariant
    coeffs = res.filter { case (_, c) => math.abs(c) >= Eps }
  }
```

**Why this works**:
- `(c₁·x^{d₁}) · (c₂·x^{d₂}) = (c₁·c₂)·x^{d₁+d₂}`
- The double loop considers all pairs of terms from `p₁` and `p₂`
- Multiple pairs may produce the same exponent, so their coefficients must be accumulated into `res`
- Taking snapshots avoids modifying `coeffs` while iterating and handles the aliasing case `p.multiply(p)` safely

**Common exam trap**: Modifying `coeffs` while iterating over it. Always build a new map, then assign.

### 2.5 coefficientsAsArray

```scala
  def coefficientsAsArray: Array[(Int, Double)] = {
    coeffs.toArray.sortBy(_._1)   // sort by exponent ascending
  }
}
```

---

## 3. Companion Object (Q3c) — 4 marks

### 3.1 Factory Methods with Assertions

```scala
object MapPoly {
  private val Eps = 1e-12

  // Zero polynomial
  def ZeroPoly: MapPoly = new MapPoly(mutable.Map.empty)

  // Monomial: c · x^e
  def MonomialPoly(c: Double, e: Int): MapPoly = {
    require(e >= 0, "Exponent must be non-negative")
    if (math.abs(c) < Eps) ZeroPoly
    else {
      val m = mutable.Map(e -> c)
      new MapPoly(m)
    }
  }

  // Constant: c · x^0
  def ConstantPoly(c: Double): MapPoly = {
    if (math.abs(c) < Eps) ZeroPoly
    else {
      val m = mutable.Map(0 -> c)
      new MapPoly(m)
    }
  }
}
```

**Common exam trap**: `ConstantPoly` — the exponent is `0`, not `c`. `Map(0 -> c)` means key = exponent `0`, value = coefficient `c`.

### 3.2 Zero Polynomial

The zero polynomial is represented by an empty map because all coefficients are zero and the representation invariant says near-zero coefficients must not be stored. It is already provided by `ZeroPoly` in the companion object.

---

## 4. Double Precision Pitfalls (Q3d) — 4 marks

### 4.1 Four Required Points

| Problem | Explanation | Mitigation |
|---------|-------------|------------|
| **Catastrophic cancellation** | Subtracting two nearly equal large numbers loses significant digits. E.g., `1.0e16 + 1.0 - 1.0e16 = 0` (not 1). | Use `BigDecimal` for financial calculations; restructure algorithms to avoid subtraction of similar magnitudes. |
| **Round-off in accumulation** | Adding many small numbers to a large accumulator causes the small numbers to be lost. E.g., summing `1e-10` one billion times with a running total of `1.0`. | Use Kahan summation algorithm; sort by magnitude and add smallest first. |
| **Equality testing** | Direct `==` on `Double` is unreliable due to representation errors. E.g., `0.1 + 0.2 == 0.3` is `false`. | Use `math.abs(a - b) < epsilon` for approximate equality; never use `==` for coefficient comparison in production. |
| **Overflow / underflow** | `Double` has finite range, approximately up to `1.8e308`. Polynomial evaluation with large exponents can overflow; very small coefficients can underflow to 0. | Use logarithmic scaling; check intermediate results; use `BigDecimal` for arbitrary precision. |

### 4.2 Exam Write-Up Template

> **Problem 1 — Catastrophic cancellation**: When two `Double` values of similar magnitude but opposite sign are added, the result may lose precision. For example, in polynomial subtraction `(2.5x³ + 1.0e16x²) - (2.5x³ + 1.0e16x² - x)`, the `x` term is lost because the small difference is below the precision available at magnitude `1.0e16`. **Avoidance**: Use `BigDecimal` or symbolic computation for exact arithmetic.
>
> **Problem 2 — Equality testing**: Due to binary representation, `0.1 + 0.2 ≠ 0.3` in `Double`. Testing `coeffs(e) == 0.0` to remove zero coefficients may fail for values that should be zero but are `1e-16` due to round-off. **Avoidance**: Use `math.abs(c) < 1e-12` instead of `c == 0.0`.

### 4.3 Important Caveat About `Eps`

Using `Eps` makes the implementation practical for floating-point arithmetic, but it is an approximation. A mathematically non-zero coefficient with very small magnitude may be treated as zero. For exact algebra, use exact numeric types such as rational numbers or `BigDecimal` where appropriate.

---

## 5. Quick Reference: Common Mistakes

| Mistake | Why Wrong | Correct |
|---------|-----------|---------|
| Writing abstract state as `Map[Int, Double]` | Confuses abstraction with implementation | Abstract state is `p ∈ ℝ[X]`; `Map` belongs in the representation |
| `coeffs(e) = c` for `math.abs(c) < Eps` | Violates the representation invariant by storing a near-zero coefficient | Remove key: `coeffs -= e` |
| `Map((c, 0))` in `ConstantPoly` | Key = `c`, value = `0`; this stores a zero coefficient at the wrong exponent | `Map(0 -> c)` (exponent 0, coefficient c) |
| `multiply` modifies `coeffs` while iterating | Concurrent modification / lost updates / aliasing bugs | Snapshot terms, build `res`, then assign `coeffs = res` |
| `add` uses `coeffs += (e -> c)` without checking existing | Overwrites instead of adding | `coeffs(e) = coefficient(e) + c`, then remove near-zero result if needed |
| `coefficientsAsArray` returns unsorted | No ordering guarantee from `Map` | `.toArray.sortBy(_._1)` |
| `== 0.0` for coefficient comparison | Floating-point round-off | `math.abs(c) < Eps` |
| Forgetting `require(e >= 0)` | Implementation fails to enforce the method precondition | Add `require(e >= 0, "Exponent must be non-negative")` |

---

## 6. Exam Strategy Checklist

Before writing Q3 answer:

- [ ] **State**: Did I write the abstract mathematical state, e.g. `p ∈ ℝ[X]` or `p: ℕ → ℝ` with finite support?
- [ ] **Abstraction vs representation**: Did I keep `Map[Int, Double]` in the implementation section rather than the abstract state?
- [ ] **DTI / RI**: Did I include finite support for the abstract DTI and non-negative exponents + no near-zero stored coefficients for the representation invariant?
- [ ] **Postconditions**: Did I specify `p = p₀` for observers and `p = p₀ + p₂₀` / `p = p₀ · p₂₀` for mutators?
- [ ] **Preconditions**: Did I state and enforce `e ≥ 0` for coefficient lookup and monomial construction?
- [ ] **multiply**: Did I use snapshots and a double loop with `d1 + d2` and `c1 * c2`?
- [ ] **DTI / RI maintenance**: After `add` and `multiply`, did I remove near-zero coefficients?
- [ ] **Factories**: Did I use `require` for assertions? Did I get `ConstantPoly` exponent right?
- [ ] **Double issues**: Did I mention cancellation, round-off, equality testing, and overflow / underflow?
- [ ] **Floating-point consistency**: Does my implementation use `math.abs(c) < Eps` instead of `== 0.0`, matching the floating-point discussion in Q3d?

---

*Template for CS2 Mock 2026. The multiply method is the single biggest differentiator — memorise the snapshot + double-loop pattern.*
