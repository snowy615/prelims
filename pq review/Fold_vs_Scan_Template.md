# fold vs scanl / scanr — Decision Template (Exam-Ready)

> **Purpose**: Distinguish when to use `fold`/`foldr` versus `scanl`/`scanr` in FP exam questions. This addresses the common error of using `scanl` where `fold` is required (e.g. polynomial composition, Horner's rule).

---

## 1. Type Signatures (Memorise These)

| Function | Type Signature | Output Type |
|----------|---------------|-------------|
| `foldr` | `(a -> b -> b) -> b -> [a] -> b` | **Single value** of type `b` |
| `foldl` | `(b -> a -> b) -> b -> [a] -> b` | **Single value** of type `b` |
| `scanr` | `(a -> b -> b) -> b -> [a] -> [b]` | **List** of intermediate results `[b]` |
| `scanl` | `(b -> a -> b) -> b -> [a] -> [b]` | **List** of intermediate results `[b]` |

**Key distinction**: `fold` collapses a list into **one** value. `scan` collapses a list but **retains every intermediate accumulator value**.

---

## 2. Operational Definitions

### foldr (right fold)
```haskell
foldr f e []     = e
foldr f e (x:xs) = f x (foldr f e xs)
```
**Associativity**: `f x1 (f x2 (f x3 e))` — parentheses group to the **right**.

### foldl (left fold)
```haskell
foldl f e []     = e
foldl f e (x:xs) = foldl f (f e x) xs
```
**Associativity**: `f (f (f e x1) x2) x3` — parentheses group to the **left**.

### scanr (right scan)
```haskell
scanr f e []     = [e]
scanr f e (x:xs) = f x (head ys) : ys  where ys = scanr f e xs
```
**Result**: `[foldr f e xs, foldr f e (tail xs), ..., foldr f e [last x], e]`

### scanl (left scan)
```haskell
scanl f e []     = [e]
scanl f e (x:xs) = e : scanl f (f e x) xs
```
**Result**: `[e, f e x1, f (f e x1) x2, ...]`

---

## 3. The Golden Rule: When to Use Which?

### Use fold when...
- The problem asks for a **single final value** (sum, product, maximum, boolean, polynomial evaluation).
- You are **composing a sequence of operations** into one result.
- The accumulator type `b` is **different** from the list element type `a`.
- **Classic patterns**: `sum`, `product`, `and`, `or`, `maximum`, `length`, `reverse`, `map` (via `foldr`), `filter` (via `foldr`), **Horner's rule**.

### Use scan when...
- The problem asks for **all prefix/suffix results** (running totals, running maxima, partial sums).
- You need to **inspect intermediate states** of a fold.
- The output is a **list** where each element is an accumulator snapshot.
- **Classic patterns**: `prefix sums`, `running maximum`, `factorials` (via `scanl (*) 1 [1..n]`), **building Pascal's triangle iteratively**.

---

## 4. Common Exam Traps

### Trap 1: "I need a list, so I use scan"
**Wrong reasoning**: The output type alone does not determine the function.

**Counter-example**: `reverse` produces a list but is defined with `foldl`:
```haskell
reverse = foldl (\acc x -> x : acc) []
```
Here the accumulator `acc` is a list, but we only need the **final** accumulator, not all intermediate reversed prefixes. Hence `foldl`, not `scanl`.

### Trap 2: Using scan for nested/iterated application
**Wrong**: Using `scanl` to build a sequence of composed polynomials.
```haskell
-- WRONG: scanl returns a list of all intermediate polynomials
compose p q = scanl (...) [last p] (init p)  -- Type error in logic
```
**Why wrong**: `scanl` gives you `[acc0, acc1, acc2, ...]`. If you only need the **last** element (the fully composed polynomial), you are throwing away the rest. More importantly, the recurrence for polynomial composition is **not** a simple left-to-right accumulation of the coefficients of `p`.

**Right**: Use `foldr` with the accumulator being a polynomial:
```haskell
compose p q = foldr (\a acc -> padd [a] (pmul q acc)) [0] p
-- Or equivalently, from the highest-degree term:
compose p q = foldl (\acc a -> padd [a] (pmul q acc)) [last p] (init p)
```

### Trap 3: Confusing the associativity direction
**Rule of thumb**:
- If the operation is naturally **right-associative** (e.g. `(:)`, function composition, Horner's rule), use `foldr` or `scanr`.
- If the operation is naturally **left-associative** (e.g. `(++)` on difference lists, reversing), use `foldl`.

---

## 5. Decision Flowchart (Use in Exams)

```
Start
│
├─ Does the problem ask for a SINGLE final value?
│  ├─ YES → Use fold
│  │        ├─ Natural right-associativity? → foldr
│  │        └─ Natural left-associativity?    → foldl
│  │
│  └─ NO → Does it ask for ALL intermediate accumulator values?
│          ├─ YES → Use scan
│          │        ├─ Right-to-left accumulation? → scanr
│          │        └─ Left-to-right accumulation? → scanl
│          │
│          └─ NO (ambiguous) → Re-read the question carefully.
│                              If building a list where each element
│                              depends on the previous, consider
│                              iterate / unfoldr instead.
```

---

## 6. Worked Examples from Past Papers

### Example A: Horner's Rule (Q3c) — Use fold
**Requirement**: Evaluate polynomial `a0 + a1*x + a2*x^2 + ... + an*x^n` using exactly `n` multiplications.

**Why fold**: We need **one integer** (the value of the polynomial at `x`).

```haskell
eval :: Poly -> Integer -> Integer
eval p x = foldr (\a acc -> a + x * acc) 0 p
```
**Trace** for `p = [a0, a1, a2]`:
```
foldr (\a acc -> a + x*acc) 0 [a0, a1, a2]
= a0 + x*(foldr ... 0 [a1, a2])
= a0 + x*(a1 + x*(a2 + x*0))
= a0 + a1*x + a2*x^2          ✓ Horner's rule
```
**Why not scanr**: `scanr` would return `[a0 + x*(...), a1 + x*a2, a2, 0]`. We only need the first element.

---

### Example B: Polynomial Composition (Q3d) — Use fold
**Requirement**: Compute `p(q(x))` where `p = Σ ai*x^i`. Follow Horner's rule structure.

**Why fold**: The result is **one polynomial** (a single list of coefficients).

```haskell
compose :: Poly -> Poly -> Poly
compose p q = foldr (\a acc -> padd [a] (pmul q acc)) [0] p
```
**Trace** for `p = [a0, a1, a2]`:
```
foldr (\a acc -> padd [a] (pmul q acc)) [0] [a0, a1, a2]
= padd [a0] (pmul q (foldr ... [0] [a1, a2]))
= padd [a0] (pmul q (padd [a1] (pmul q (padd [a2] (pmul q [0])))))
= a0 + q*(a1 + q*(a2 + q*0))
= a0 + a1*q + a2*q^2          ✓ matches p(q(x))
```
**Why not scanl/scanr**: The accumulator is a **single polynomial** that gets repeatedly multiplied by `q` and added to. We do not need the sequence of all intermediate polynomials `0, a2*q, a1*q + a2*q^2, ...`.

---

### Example C: Pascal's Triangle (Q3e) — Use iterate or scanl
**Requirement**: Infinite list `pascal :: [Poly]` where `pascal !! i = (1 + x)^i`.

**Why scanl/iterate**: We need **all powers**, not just one.

```haskell
pascal :: [Poly]
pascal = iterate (\p -> pmul p [1, 1]) [1]
-- Or equivalently:
pascal = scanl (\acc _ -> pmul acc [1, 1]) [1] [1..]
```
Here `scanl` is appropriate because the output is a **list of all intermediate results** (`[1], [1,1], [1,2,1], ...`).

---

## 7. Quick Reference Card (Memorise for Exam)

| Situation | Function | Why |
|-----------|----------|-----|
| Sum of list | `foldr (+) 0` | Single number |
| Product of list | `foldr (*) 1` | Single number |
| Reverse list | `foldl (flip (:)) []` | Single list (final accumulator) |
| Map function | `foldr (\x xs -> f x : xs) []` | Single list (final result) |
| Horner's rule | `foldr (\a acc -> a + x*acc) 0` | Single number |
| Polynomial composition | `foldr (\a acc -> padd [a] (pmul q acc)) [0]` | Single polynomial |
| Prefix sums | `scanl (+) 0` | List of running totals |
| Pascal's triangle | `iterate` or `scanl` | List of all powers |
| Tails + fold identity | `scanr` | List of all suffix folds (Q1e) |

---

## 8. Self-Test Checklist

Before writing your answer, ask:

- [ ] Is the desired output a **single value** or a **list of values**?
- [ ] If it's a list, does each element depend on **accumulating** previous ones?
- [ ] Is the natural associativity **left-to-right** or **right-to-left**?
- [ ] Would `scanl/scanr` produce **unnecessary intermediate results** that the question ignores?
- [ ] Is the accumulator type the **same** as the output type? (If yes, fold is likely.)

---

## 9. One-Sentence Rule

> **If the question asks for "the result of applying f to all elements", use `fold`. If it asks for "the sequence of partial results", use `scan`. Never use `scan` just because the accumulator happens to be a list.**

---

*Template compiled for Oxford CS1 FP examination style. Covers Q1e (scanr), Q3c (fold for Horner), Q3d (fold for composition), and Q3e (iterate/scanl for Pascal).*