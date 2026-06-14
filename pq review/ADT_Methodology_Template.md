# ADT Design Methodology — CS2 Exam Quick Reference

> **Purpose**: Universal framework for any ADT specification question (Q3-style). Apply Layer 1 to understand the question, then use Layer 2 (Polynomial/Set/Stack/Rational examples) to see how it instantiates.

---

## Layer 1: Universal Methodology (~60% of your answer)

### 1. Choosing Abstract State — Three Questions

Answer these in order. The result is your `// State:` comment.

| Question | What to write | Polynomial Example | Rational Example |
|----------|--------------|-------------------|------------------|
| **Q1: Mathematical identity?** | What is this object, mathematically? | `p ∈ ℝ[X]` (polynomial over reals) | `q ∈ ℚ` (rational number) |
| **Q2: What information constitutes a value?** | List the components | Coefficient map `c: ℕ → ℝ` | Pair `(num, den) ∈ ℤ × ℤ⁺` |
| **Q3: What constraints always hold?** | These become your DTI seeds | `c(e) ≠ 0` when stored; `e ≥ 0` | `den ≠ 0`; `gcd(num, den) = 1` (optional) |

**Exam template**:
```text
// State: [mathematical type] where [component 1], [component 2], ...
// DTI: [constraint 1] ∧ [constraint 2] ∧ ...
```

---

### 2. Two-Layer Invariant System

CS2 distinguishes between **abstract-level** and **representation-level** invariants:

| Layer | What it constrains | Written in | Example |
|-------|-------------------|------------|---------|
| **Abstract State + DTI** | The mathematical object itself, independent of any implementation | Part (a): trait specification | `p ∈ ℝ[X]` with finite support (only finitely many non-zero coefficients) |
| **Representation + Rep-Invariant** | The concrete data structure chosen to implement it | Part (b): implementation | `coeffs: Map[Int, Double]` with `coeffs(e) ≠ 0` (sparse storage invariant) |

**Critical exam rule**: Part (a) asks for abstract DTI — keep it mathematical. Part (b) asks for representation — here you mention `Map`, `Array`, `List`, and their structural constraints.

---

### 2a. Abstract DTI Patterns — Four Categories

These constrain the mathematical object, not the code.

| Category | Pattern | Example ADTs | Formalisation |
|----------|---------|---------------|---------------|
| **Finite support** | Only finitely many non-zero / active elements | Polynomial (finitely many non-zero coefficients), SparseMatrix | `|{e : c(e) ≠ 0}| < ∞` |
| **Non-negative / Positive** | Indices, sizes, counts must be ≥ 0 or > 0 | Polynomial (`e ≥ 0`), Rational (`den > 0`), Array indices | `∀x ∈ domain. x ≥ 0` (or `> 0` for strictly positive, e.g. `den > 0`) |
| **Well-formedness** | Mathematical constraints on the value | Rational (`den ≠ 0`), Complex (real/imag parts finite), BST (heap property) | `den ≠ 0` |
| **Ordering** | Sequence must be sorted for efficient ops | SortedList, BST invariants | `∀i,j. 0 ≤ i < j < size ⇒ store[i] ≤ store[j]` |
| **Mutual exclusion** | Two properties cannot both hold | Red-Black Tree (red node → children black), Mutex | `a ⇒ ¬b` |
| **Capacity** | Size bounded by pre-allocated space | BoundedQueue, BoundedStack, Array-based structures | `size ≤ capacity` |

**Exam write-up**: List the categories that apply, then instantiate with your ADT's specific constraints.

---

### 3. Post-Condition Templates by Operation Type

Classify every method into one of four types. Use the matching template.

| Type | Modifies `this`? | Returns | Template |
|------|------------------|---------|----------|
| **Observer** | ❌ No | Value | `// Post: state = state₀ ∧ returns f(state₀)` |
| **Producer** | ❌ No | New object | `// Post: returns new object s.t. state' = f(args)` |

> **Immutable vs Mutable**: Numeric ADTs (Rational, Complex, BigInt) are usually **immutable** — operations return new objects. Collection ADTs (Set, Stack, Polynomial) are usually **mutable** — operations modify `this`. Check the return type: `Unit` = mutable, `ADT` = immutable.
| **Mutator** | ✅ Yes | `Unit` | `// Post: state = f(state₀, args) ∧ args unchanged` |
| **Converter** | ❌ No | Other representation | `// Post: state = state₀ ∧ returns repr(state) where repr preserves info` |

**Critical**: For **Mutators**, always specify that arguments are unchanged (frame condition). For **Observers**, always specify `state = state₀`.

---

### 4. Initial State — Two Cases

| Case | When it applies | What to write | Example |
|------|----------------|---------------|---------|
| **A: Meaningful initial state** | There is a natural "empty" or "zero" value | Specify it explicitly | Polynomial: zero polynomial (empty map). Set: empty set. Stack: empty stack. |
| **B: No meaningful initial state** | The ADT requires parameters to be valid | State "cannot be specified until constructor is called" | Rational: `num` and `den` must be provided; no default rational exists. |

**Exam trap**: Never say "initial state is undefined" — always choose A or B and justify.

---

### 2b. Representation Invariant (Rep-Invariant) — Part (b)

These constrain the **concrete data structure**, not the abstract object. They appear in part (b) with the abstraction function.

| Category | When it applies | Example |
|----------|---------------|---------|
| **Sparse storage** | Don't store zero / default values | Polynomial `Map` only stores non-zero coefficients |
| **Canonical form** | Unique representation per abstract value | Rational `gcd(|num|, den) = 1`; polynomial terms sorted by exponent |
| **No duplicates** | Data structure enforces uniqueness | Set `Map[A, Unit]` has unique keys; list-based Set must check before insert |
| **Sorted for efficiency** | Structure maintains order | BST left < root < right; sorted array for binary search |
| **Structural integrity** | Pointers / indices valid | Linked list: `tail.next == null`; array-based: `0 ≤ head < capacity` |

---

### 5. Implementation Patterns — Three Representations

| Pattern | When to use | Operations fast | Operations slow | Example ADTs |
|---------|-------------|-----------------|-----------------|--------------|
| **Map-based** | Sparse data, key-value lookup | `get`, `contains` | `min`, `max`, iteration | Polynomial, SparseMatrix, Set |
| **Array-based** | Dense data, index access | `apply(i)`, `length` | `insert`, `delete`, `search` | SortedList, ArrayStack, Queue |
| **Linked structure** | Frequent insert/delete at ends | `prepend`, `append` | `random access`, `length` | ListStack, LinkedList, Deque |

**Abstraction function template**:
```text
// Abs: store represents [mathematical object] where
//      [explain how each stored element contributes]
```

---

## Layer 2: Case Studies (~40% of your answer)

### Case Study A: Polynomial (Your Q3)

Apply Layer 1:
- **Abstract State**: `p: ℕ → ℝ` (function from exponents to coefficients)
- **Abstract DTI**: Finite support — `|{e : p(e) ≠ 0}| < ∞` (only finitely many non-zero coefficients)
- **Methods**: `coefficient` (Observer), `add`/`multiply` (Mutator), `coefficientsAsArray` (Converter)
- **Initial**: Case A/B hybrid — the zero polynomial exists mathematically, but the trait has no default constructor; objects are created via companion factories

**Representation (Part b)**:
- **Rep**: `coeffs: Map[Int, Double]`
- **Abs**: `p(e) = coeffs.getOrElse(e, 0.0)`
- **Rep-Invariant**: `coeffs(e) ≠ 0` (sparse storage) ∧ `e ≥ 0` (valid exponent)

See `Polynomial_ADT_Template_v2.md` for full implementation.

---

### Case Study B: Rational Number

**Trait (Part a — Abstract)**:
```scala
trait Rational {
  // State: q ∈ ℚ
  // DTI: (trivial — every rational number is a valid abstract value)

  def numerator: Int     // Observer
  def denominator: Int   // Observer
  def add(r: Rational): Unit   // Mutator: this = this₀ + r
  def multiply(r: Rational): Unit  // Mutator: this = this₀ · r
}
```

**Implementation (Part b — Representation)**:
```scala
class RationalImpl private (var num: Int, var den: Int) extends Rational {
  // Rep: (num, den) ∈ ℤ × ℤ⁺
  // Abs: q = num / den
  // Rep-Invariant: den > 0 ∧ gcd(|num|, den) = 1  (canonical form)

  // DTI maintenance: after every operation, reduce by gcd
  private def canonicalize(): Unit = {
    val g = gcd(math.abs(num), den)
    num /= g
    den /= g
  }
}
```

---

### Case Study C: Generic Set (Mutable)

**Trait**:
```scala
trait Set[A] {
  // State: s ⊆ A (a subset of the element type)
  // DTI: (trivial — sets have no duplicates by mathematical definition)

  def contains(x: A): Boolean   // Observer
  def add(x: A): Unit           // Mutator: s = s₀ ∪ {x}
  def remove(x: A): Unit        // Mutator: s = s₀ \ {x}
  def toArray: Array[A]         // Converter
}
```

**Implementation choice**: `mutable.Set[A]` or `mutable.Map[A, Unit]` (Map-based pattern, exploiting uniqueness).

**Rep-Invariant**: If using `List[A]`, must ensure no duplicates are stored (check before `add`). If using `mutable.Set[A]`, the data structure guarantees uniqueness.

---

### Case Study D: Stack (LIFO)

**Trait**:
```scala
trait Stack[A] {
  // State: seq = [a₁, a₂, ..., aₙ] (ordered list, top at end)
  // DTI: none beyond type constraints

  def push(x: A): Unit    // Mutator: seq = seq₀ ++ [x]
  def pop(): A             // Mutator: returns last(seq₀), seq = init(seq₀)
  def peek: A              // Observer: returns last(seq)
  def isEmpty: Boolean     // Observer: returns seq.length == 0
}
```

**Implementation choice**:
- **Linked list (List-based)**: `top at front` — `push` = prepend, `pop` = remove head, both O(1). Never put top at end (tail pop is O(n) in singly-linked list).
- **Array (Array-based)**: `top at end` — `push` = append, `pop` = remove last, both amortised O(1).

---

## Layer 3: Common Exam Pitfalls (Universal)

| Pitfall | Why it loses marks | Correct approach |
|---------|-------------------|------------------|
| **Implementation in State** | `// State: p is a mutable.Map` — this is representation, not abstraction | `// State: p: ℕ → ℝ` (mathematical). Keep `Map`/`Array` for part (b) only. |
| **Missing DTI** | Forgetting `den > 0` or `e ≥ 0` | Use the 4-category abstract checklist; put canonical-form / non-zero in Rep-Invariant (part b) |
| **Operational post-conditions** | "First add the coefficients, then..." | `state = state₀ + p₂` (mathematical) |
| **Missing frame condition** | Mutator says `state = f(state₀)` but doesn't say `args` unchanged | Always add `∧ args unchanged` |
| **No initial state discussion** | Skipping the "Is it meaningful?" question | Explicitly choose Case A or B |
| **Observer modifies state** | `coefficient` accidentally caches or updates | Start with `// Post: state = state₀` |
| **DTI violation in code** | `add` stores zero coefficients | Filter or remove after operation |

---

## Exam Strategy: Q3-Style ADT Questions

### Time allocation (20 marks ≈ 25-30 minutes)

| Section | Marks | Time | What to write |
|---------|-------|------|---------------|
| (a) Spec + DTI + Initial | 6-7 | 8 min | State (math), DTI (6 categories), post-conditions (4 types), initial state (A/B) |
| (b) Implementation | 6-7 | 10 min | Class skeleton, Abs function, each method with DTI maintenance |
| (c) Companion object | 2-4 | 5 min | Factory methods with assertions |
| (d) Pitfalls / Analysis | 2-4 | 5 min | Floating-point, overflow, or complexity discussion |

### If the ADT is unfamiliar

1. Identify the **mathematical identity** (Q1)
2. Pick **2-3 DTI categories** from the 6 that obviously apply (non-zero, non-negative, uniqueness are most common)
3. Classify each method as **Observer / Producer / Mutator / Converter**
4. Write post-conditions using the templates
5. Choose implementation pattern based on density (Map vs Array vs Linked)

---

*Template for CS2 Mock 2026. Layer 1 gives you the method; Layer 2 shows you the method in action. Master Layer 1, reference Layer 2.*
