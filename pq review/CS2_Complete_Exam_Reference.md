# Scala & Spectra Exam Reference — CS2 Imperative Programming + Proof Systems

> **Purpose**: Single comprehensive reference for timed exams. Covers Scala syntax, common pitfalls, spectrum constructions, and complexity analysis templates.

---

## Part I: Scala — Arrays, Maps, and Loops

### 1. Array Creation (High-Frequency Error Zone)

```scala
// CORRECT ways to create arrays
val arr = new Array[Int](5)              // [0, 0, 0, 0, 0]
val bools = new Array[Boolean](n)        // [false, false, ...]
val filled = Array.fill(n)(0)            // fill with 0
val filled = Array.fill(n)(-1)           // for lastIndex default
val tabulated = Array.tabulate(n)(i => i * i)
val fromValues = Array(1, 2, 3, 4)       // literal (NOT [1,2,3])

// WRONG — these will NOT compile or do the wrong thing
val wrong1 = Array(5)                    // Array with ONE element [5]
val wrong2 = Array[n]                    // ILLEGAL SYNTAX
val wrong3 = new Array[Boolean][n]       // ILLEGAL SYNTAX
```

### 2. Array Access and Operations

```scala
arr(i)              // access (parentheses, NOT brackets)
arr(i) = x          // update
arr.length          // size (NOT arr.size — that's for collections)
arr.indices         // equivalent to 0 until arr.length

// Slicing (returns new array)
arr.slice(from, until)   // elements arr(from) .. arr(until-1)
arr.take(k)              // first k elements
arr.drop(k)              // all except first k
arr.tail                 // all except first
arr.init                 // all except last
arr ++ b                 // concatenation
arr.max / arr.min        // extrema
arr.sum                  // aggregation
```

### 3. mutable.Map — Polynomial Implementation Core

```scala
import scala.collection.mutable

// Creation
val m = mutable.Map.empty[Int, Double]
val m = mutable.Map.empty[Int, Double].withDefaultValue(0.0)
val m = mutable.Map(0 -> 4.0, 1 -> -1.0, 3 -> 2.5)

// Access and update
m.getOrElse(key, 0.0)    // safe read (exam-friendly)
m(key) = value           // insert / update
m += (key -> value)      // alternative syntax
m -= key                 // remove
m.contains(key)
m.size

// Iteration
for ((k, v) <- m) { ... }        // key-value pairs
for (k <- m.keys) { ... }

// Conversion
val arr: Array[(Int, Double)] = m.toArray   // for coefficientsAsArray
```

**Polynomial `add` — correct implementation:**
```scala
def add(p2: Poly): Unit =
  for ((e, c) <- p2.coefficientsAsArray)
    coeffs(e) = coefficient(e) + c
```

**Polynomial `multiply` — correct implementation:**
```scala
def multiply(p2: Poly): Unit = {
  val res = mutable.Map.empty[Int, Double].withDefaultValue(0.0)
  for ((d, cd) <- coeffs; (d2, cd2) <- p2.coefficientsAsArray)
    res(d + d2) = res(d + d2) + cd * cd2
  coeffs = res
}
```

### 4. Tuples and Destructuring

```scala
val t = (3, 2.5)         // (Int, Double)
t._1                     // first = 3
t._2                     // second = 2.5
val (i, j) = (0, n)      // destructuring
var (i, j) = (0, n)      // mutable destructuring (for binary search)
```

### 5. Loops — while and for

```scala
// Standard while
var i = 0
while (i < n) {
  // body
  i += 1    // MUST manually increment
}

// Downward loop
var j = n - 1
while (j >= 0) {
  // body
  j -= 1
}

// for-comprehension
for (i <- 0 until n) { ... }      // 0, 1, ..., n-1
for (i <- 0 to n) { ... }         // 0, 1, ..., n (inclusive)
for (i <- n-1 to 0 by -1) { ... } // reverse
for (x <- arr) { ... }            // iterate elements
for ((x, i) <- arr.zipWithIndex) { ... }  // with index
```

### 6. NO break / continue in Scala

**Option A — Boolean flag (safest for exams):**
```scala
var found = false
var i = 0
while (i < n && !found) {
  if (condition) found = true
  else i += 1
}
```

**Option B — Early return (idiomatic, preferred):**
```scala
def findRightmost(a: Array[Int], v: Int): Int = {
  var i = a.length - 1
  while (i >= 0) {
    if (a(i) == v) return i
    i -= 1
  }
  -1
}
```

**Option C — scala.util.control.Breaks (avoid in exams):**
```scala
import scala.util.control.Breaks._
breakable { for (...) { if (cond) break() } }
```

### 7. Pre/Post Conditions and Assertions

```scala
require(a.length > 0)       // precondition on caller input
require(c != 0.0)           // DTI check for constructors
assert(e >= 0)              // internal invariant check
```

**Mnemonic:** `require` guards against bad inputs; `assert` guards against logic bugs.

### 8. Boundary Value Initialisations

```scala
Int.MinValue    // initial curmax when finding maximum
Int.MaxValue    // initial curmin when finding minimum
```

**Critical pitfall:** Never use `var curmax = 0` for finding max — it fails on all-negative arrays.

---

## Part II: Canonical Algorithm Templates

### Template 1: Right-to-Left Scan (Leaders / Suffix Problems)

```scala
def leaders(a: Array[Int]): Array[Boolean] = {
  val n = a.length
  val b = new Array[Boolean](n)
  if (n == 0) return b

  var curmax = Int.MinValue
  var i = n - 1
  while (i >= 0) {
    if (a(i) > curmax) {
      b(i) = true
      curmax = a(i)
    } else {
      b(i) = false
    }
    i -= 1
  }
  b
}
```

### Template 2: Standard Binary Search (Leftmost Occurrence)

```scala
def binarySearch(a: Array[Int], v: Int): Int = {
  var (i, j) = (0, a.length)
  // invariant: v not in a[0..i) and not in a[j..n)
  while (i < j) {
    val m = i + (j - i) / 2    // avoid overflow: NOT (i + j) / 2
    if (a(m) < v) i = m + 1
    else j = m
  }
  if (i < a.length && a(i) == v) i else -1
}
```

### Template 3: lastIndex — O(n) Single Pass

```scala
def lastIndex(a: Array[Int]): Array[Int] = {
  require(a.length > 0 && a.forall(_ >= 0))
  val mx = a.max
  val b = Array.fill(mx + 1)(-1)
  for (i <- a.indices) b(a(i)) = i   // overwrite: last write wins
  b
}
```

### Template 4: isKRotatedSorted

```scala
def isKRotatedSorted(a: Array[Int], k: Int): Boolean = {
  val n = a.length
  if (n <= 1) return true

  // Check a[0..n-k) is increasing
  val firstSorted = (0 until n - k - 1).forall(i => a(i) <= a(i + 1))
  // Check a[n-k..n) is increasing  
  val secondSorted = (n - k until n - 1).forall(i => a(i) <= a(i + 1))
  // Check rotation point (or k=0 meaning fully sorted)
  val rotationOK = k == 0 || a(n - k - 1) > a(n - k)

  firstSorted && secondSorted && rotationOK
}
```

### Template 5: Find Rotation Point in O(log n)

```scala
def findRotationPoint(a: Array[Int]): Int = {
  var (i, j) = (0, a.length)
  val last = a(a.length - 1)
  while (i < j) {
    val m = i + (j - i) / 2
    if (a(m) > last) i = m + 1
    else j = m
  }
  i   // index of smallest element = rotation point k
}
```

---

## Part III: Spectra — First-Order Logic

### 1. Core Definitions

For a first-order sentence F, the **spectrum** S(F) is:

$$S(F) = \{ |U_{\mathcal{A}}| : \mathcal{A} \models F, \ |U_{\mathcal{A}}| \text{ is finite} \} \subseteq \mathbb{N}$$

### 2. Building Blocks (Memorise These)

**"At least n elements":**
$$AtLeast_n := \exists x_1 \dots \exists x_n \bigwedge_{i<j} x_i \neq x_j$$

**"At most n elements":**
$$AtMost_n := \forall y_1 \dots \forall y_{n+1} \bigvee_{i<j} y_i = y_j$$

**"Exactly n elements":**
$$Exactly_n := AtLeast_n \land AtMost_n$$

**Functionality (R is a partial function):**
$$\forall x \forall y \forall z (R(x,y) \land R(x,z) \to y = z)$$

**Injectivity:**
$$\forall x \forall y (f(x) = f(y) \to x = y)$$

**Surjectivity:**
$$\forall y \exists x (f(x) = y)$$

### 3. Template: Finite Set is a Spectrum [Q5(d)(i)]

**Construction:**
$$F := Exactly_{n_1} \lor Exactly_{n_2} \lor \dots \lor Exactly_{n_k}$$

**Verification:**
- If $\mathcal{A} \models F$, then $\mathcal{A} \models Exactly_{n_i}$ for some $i$, so $|U_{\mathcal{A}}| = n_i \in M$.
- Conversely, for any $n_i \in M$, a structure with exactly $n_i$ elements satisfies $Exactly_{n_i}$, hence $F$.

**Exam write-up:**
> Let $M = \{n_1, \dots, n_k\}$. Define $F = \bigvee_{i=1}^{k} Exactly_{n_i}$. Then $\mathcal{A} \models F$ iff $|U_{\mathcal{A}}| = n_i$ for some $i$, so $S(F) = M$. $\square$

### 4. Template: Multiples of k [Q5(d)(ii)]

**Key idea:** A set has size divisible by $k$ iff it admits a fixed-point-free permutation where every cycle has length exactly $k$.

**Construction** (for $k = 3$):
$$F := \forall x (f(f(f(x))) = x) \land \forall x (f(x) \neq x)$$

In finite models, $f^3 = id$ implies $f$ is bijective. With no fixed points, all cycles have length 3, so $|U_{\mathcal{A}}| = 3n$.

**Verification:**
- Model $\to$ size: $f$ partitions $U$ into disjoint 3-cycles.
- Size $\to$ model: Partition $3n$ elements into $n$ triples, cycle within each.

### 5. Template: Perfect Squares [Q5(d)(iii)]

**Key idea:** A set of size $n$ has $n^2$ ordered pairs. Make $U \cong A \times A$.

**Construction:** Use unary predicate $P$ (the "axis" set) and ternary relation $R$ encoding $P \times P \to U$.

$$F := \underbrace{\forall x \forall y (P(x) \land P(y) \to \exists z (R(x,y,z) \land \forall z' (R(x,y,z') \to z = z')))}_{\text{R is a total function on } P \times P}$$
$$\land \underbrace{\forall z \exists x \exists y (P(x) \land P(y) \land R(x,y,z))}_{\text{surjective}}$$
$$\land \underbrace{\forall x_1 \forall x_2 \forall y_1 \forall y_2 \forall z (R(x_1,y_1,z) \land R(x_2,y_2,z) \to x_1 = x_2 \land y_1 = y_2)}_{\text{injective}}$$

**Verification:**
- If $\mathcal{A} \models F$, then $R$ gives bijection $P^{\mathcal{A}} \times P^{\mathcal{A}} \cong U_{\mathcal{A}}$, so $|U_{\mathcal{A}}| = |P^{\mathcal{A}}|^2 = n^2$.
- Conversely, any $n^2$-sized set admits such a pairing structure.

### 6. Advanced Constructions

| Set | Technique | Formula Pattern |
|-----|-----------|----------------|
| $\{2n\}$ | Involution without fixed points | $f(f(x)) = x \land f(x) \neq x$ |
| $\{kn\}$ | $k$-cycles | $f^k(x) = x \land \bigwedge_{d|k, d<k} f^d(x) \neq x$ |
| $\{n^k\}$ | $k$-ary product | Generalise pairing to $k$-ary relations |
| $\{n!\}$ | Permutation group | Universe = set of all permutations of $n$ elements |

---

## Part IV: Complexity Analysis Templates

### Standard Phrases for Exams

| Situation | Template Phrase |
|-----------|-----------------|
| Single loop over $n$ | "The loop iterates $n$ times, each doing $O(1)$ work, giving $O(n)$ total." |
| Nested loops | "The outer loop runs $n$ times; the inner loop runs $m$ times per iteration, giving $O(n \cdot m)$." |
| Halving search space | "Each iteration halves the search space, so there are $O(\log n)$ iterations, each $O(1)$, giving $O(\log n)$ total." |
| Amortised | "Operation $X$ may take $O(n)$, but it only occurs after $n$ cheap operations, so amortised cost is $O(1)$." |
| Space complexity | "The algorithm stores an array of size $n$, giving $O(n)$ auxiliary space." |

### Binary Search Complexity Justification

> "The invariant maintains that the target, if present, lies in $a[i..j)$. Each iteration computes $m = \lfloor(i+j)/2\rfloor$ and halves the interval. After $k$ steps, the interval has size at most $n/2^k$. The loop terminates when $n/2^k < 1$, i.e., $k > \log_2 n$. Thus $O(\log n)$ iterations, each $O(1)$, yield $O(\log n)$ total time."

---

## Part V: Error Checklist

### Before Submitting — 30-Second Scan

- [ ] **Array creation**: `new Array[T](n)`, not `Array(n)` or `Array[n]`
- [ ] **Array access**: `arr(i)`, not `arr[i]`
- [ ] **Length**: `arr.length`, not `arr.size`
- [ ] **Loop increment**: Every `while` has its counter updated
- [ ] **Break alternative**: No `break` used; replaced by flag or early return
- [ ] **Map default**: `m.getOrElse(k, default)`, not bare `m(k)` when key might be absent
- [ ] **Max initialisation**: `Int.MinValue`, not `0`
- [ ] **Return type match**: Boolean functions return `true`/`false`, not indices
- [ ] **Slice arithmetic**: When passing subarrays, account for index offsets
- [ ] **Overflow-safe midpoint**: `i + (j - i) / 2`, not `(i + j) / 2`

---

## Part VI: Exam Strategy

### Time Allocation (3-hour paper, 5 questions)

| Phase | Time | Action |
|-------|------|--------|
| Reading | 10 min | Skim all 6 questions; mark order of attack |
| Quick wins | 30 min | Answer parts (a), (d), (g) — usually definition-based |
| Core work | 120 min | Work through remaining parts in priority order |
| Review | 20 min | Check syntax, invariants, completeness |

### Priority Order (based on mark density)

1. **Spectra constructions** (Q5(d)): 9 marks, template-driven, high ROI
2. **Loop invariants** (Q1(c)): 5 marks, follow template strictly
3. **Algorithm implementation** (Q2, Q3): Focus on correctness over cleverness
4. **Proof systems** (Q5(a)-(c), Q6): State definitions precisely

### If Stuck

- Write down the **definition** being asked for (1 mark even if incomplete)
- State the **invariant/variant** in words before formalising
- For spectra: always write $Exactly_n$ formula first

---

*Template compiled from CS2 Mock 2026 analysis. Focus on precision over cleverness.*
