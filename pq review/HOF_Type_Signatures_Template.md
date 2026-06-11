# Higher-Order Function Type Signatures — Exam Template

> **Purpose**: Derive and write correct type signatures for higher-order functions in FP exam questions. This addresses the common error of overly restrictive types (e.g. forcing `a -> a` when `a -> b` is required).

---

## 1. The Golden Method: Type Derivation by Template Matching

When asked to "find f such that `unfoldr f x = ...`", do not guess. Follow this mechanical procedure.

### Step 1 — Write down the known type of the outer function

```haskell
unfoldr :: (a -> Maybe (b, a)) -> a -> [b]
```

### Step 2 — Identify the concrete instantiation

Look at the **argument** `x` in the call `unfoldr f x`. Its type is the concrete type that replaces the type variable `a`.

> **Example (Q1b)**: `unfoldr f (g, xs) = map g xs`
> - The seed `x` is `(g, xs)`, so `a` is instantiated to `(g_type, xs_type)`.
> - The result is `[b]` where `b` is the element type of `map g xs`.

### Step 3 — Solve for f's type by substitution

Since `unfoldr :: (a -> Maybe (b, a)) -> a -> [b]`, and `a` is now `(g_type, xs_type)`:

```
f :: (g_type, xs_type) -> Maybe (b, (g_type, xs_type))
```

Now determine `g_type` and `xs_type` from the result:
- `map g xs` requires `g :: c -> d` and `xs :: [c]`, producing `[d]`.
- The result of `unfoldr` is `[b]`, so `b = d`.
- Therefore `g :: c -> b` and `xs :: [c]`.

Substituting back:
```
f :: (c -> b, [c]) -> Maybe (b, (c -> b, [c]))
```

### Step 4 — Rename type variables to standard letters

```haskell
f :: (a -> b, [a]) -> Maybe (b, (a -> b, [a]))
```

**Critical check**: Does `g` appear in the output state? Yes — `g` must be carried forward unchanged because `unfoldr` only transforms the seed; it does not preserve external variables unless you thread them through the state.

---

## 2. Common Higher-Order Function Types (Memorise)

| Function | Type Signature | Key Observation |
|----------|---------------|-----------------|
| `map` | `(a -> b) -> [a] -> [b]` | Transforms elements; list structure preserved |
| `filter` | `(a -> Bool) -> [a] -> [a]` | Predicate; same type in and out |
| `foldr` | `(a -> b -> b) -> b -> [a] -> b` | Right-associative; accumulator type `b` independent of `a` |
| `foldl` | `(b -> a -> b) -> b -> [a] -> b` | Left-associative; same independence |
| `unfoldr` | `(a -> Maybe (b, a)) -> a -> [b]` | Seed type `a` can differ from output element type `b` |
| `scanr` | `(a -> b -> b) -> b -> [a] -> [b]` | Like `foldr` but returns all intermediate `b` values |
| `scanl` | `(b -> a -> b) -> b -> [a] -> [b]` | Like `foldl` but returns all intermediate `b` values |
| `iterate` | `(a -> a) -> a -> [a]` | Seed and element type must be identical |
| `(.)` | `(b -> c) -> (a -> b) -> (a -> c)` | Function composition; three distinct types |

---

## 3. The Most Common Exam Mistake: Over-Unification

### The Error Pattern

Students see a function like `g` inside a tuple and assume `g` must have type `a -> a` because "it stays the same type throughout".

**Wrong (Q1b)**:
```haskell
f :: (a -> a, [a]) -> Maybe (a, (a -> a, [a]))
```

**Why wrong**: This forces `g :: a -> a`, meaning `map g xs` can only produce `[a]`. But `map` in general produces `[b]` where `b` can differ from `a`. The question says "for all functions g", including `g :: Int -> Bool`.

**Right**:
```haskell
f :: (a -> b, [a]) -> Maybe (b, (a -> b, [a]))
```

### The Rule

> **If a function is passed as an argument and applied to data, its return type is determined by the result of that application, not by the input type of the data.**

---

## 4. Type Signature Writing Checklist

Before finalising any type signature in an exam, verify:

- [ ] **Are all type variables explicitly introduced?** (No free variables.)
- [ ] **Does each type variable appear on both sides of `->` when it should?** (Or is it properly scoped?)
- [ ] **Are distinct semantic roles given distinct type variables?** (If a function transforms `a` to `b`, do not reuse `a` for the output.)
- [ ] **Does the signature allow the full generality claimed by the question?** (e.g. "for all functions g" means `g` must be polymorphic in your answer.)
- [ ] **Is the state properly threaded?** (In `unfoldr`, `f` must return the new seed; any data needed in future steps must be in the seed.)

---

## 5. Worked Examples from Past Papers

### Example A: Q1b — unfoldr for map

**Question**: Find `f` such that `unfoldr f (g, xs) = map g xs` for all `g` and `xs`.

**Derivation**:
1. `unfoldr :: (a -> Maybe (b, a)) -> a -> [b]`
2. Seed is `(g, xs) :: (a -> b, [a])`, so `a` instantiates to `(a -> b, [a])`.
3. Result is `[b]` (from `map g xs`), so output element type is `b`.
4. Therefore:
   ```haskell
   f :: (a -> b, [a]) -> Maybe (b, (a -> b, [a]))
   ```
5. Implementation:
   ```haskell
   f (g, [])     = Nothing
   f (g, x:xs)   = Just (g x, (g, xs))
   ```

**Common error to avoid**: Writing `f :: (a -> a, [a]) -> ...` — this only works when `g` is an endofunction.

---

### Example B: Q1a — unfoldr for countdown

**Question**: Find `f` such that `unfoldr f n = [n, n-1, ..., 1]` for `n >= 0`.

**Derivation**:
1. `unfoldr :: (a -> Maybe (b, a)) -> a -> [b]`
2. Seed is `n :: Integer`, so `a = Integer`.
3. Result is `[Integer]`, so `b = Integer`.
4. Therefore:
   ```haskell
   f :: Integer -> Maybe (Integer, Integer)
   ```
5. Implementation:
   ```haskell
   f 0 = Nothing
   f n = Just (n, n-1)
   ```

**Note**: Here `a = b = Integer`, but this is a coincidence of the specific problem, not a requirement of `unfoldr`.

---

### Example C: Q1c — filter as fold

**Question**: Is `filter p = fold f x` possible? If so, find `f` and `x`.

**Derivation**:
1. `filter :: (a -> Bool) -> [a] -> [a]`
2. `fold :: (a -> b -> b) -> b -> [a] -> b`
3. The result type of `fold` is `b`. We need `b = [a]`.
4. Therefore:
   ```haskell
   f :: a -> [a] -> [a]
   x :: [a]
   ```
5. Solution:
   ```haskell
   f = \y acc -> if p y then y : acc else acc
   x = []
   ```

**Type check**: `fold f [] :: [a] -> [a]`, which matches `filter p :: [a] -> [a]`. ✓

---

### Example D: Q1d — tail as fold

**Question**: Is `tail = fold f x` possible?

**Derivation**:
1. `tail :: [a] -> [a]`
2. `fold :: (a -> b -> b) -> b -> [a] -> b`
3. If `tail = fold f x`, then `b = [a]`.
4. So `f :: a -> [a] -> [a]` and `x :: [a]`.
5. But `fold f x [] = x`, and `tail []` is undefined. Contradiction at the base case.
6. Even ignoring `[]`, `fold` processes elements one by one with no access to the list length, while `tail` must drop exactly one element regardless of length. The accumulator cannot distinguish "drop the first element" from "drop all elements" without an external counter.

**Conclusion**: Impossible. `fold` has no mechanism to skip exactly one element and return the rest.

---

## 6. Type Variable Naming Convention

| Convention | Meaning | Example |
|------------|---------|---------|
| `a, b, c` | General types | `map :: (a -> b) -> [a] -> [b]` |
| `a` (same letter) | Same type throughout | `filter :: (a -> Bool) -> [a] -> [a]` |
| `a` and `b` (different) | Types may differ | `map :: (a -> b) -> [a] -> [b]` |
| `a -> a` | Endofunction | `iterate :: (a -> a) -> a -> [a]` |
| `a -> b` | General function | `map :: (a -> b) -> [a] -> [b]` |

**Rule**: If the question says "for all functions g", and `g` is applied to elements of type `a` producing elements that go into the result list, the type of `g` is almost certainly `a -> b`, not `a -> a`.

---

## 7. Quick Reference: Type Derivation Cheat Sheet

When you see `unfoldr f seed = result`:

```
Step 1: unfoldr :: (s -> Maybe (e, s)) -> s -> [e]
Step 2: seed :: S  determines  s = S
Step 3: result :: [E]  determines  e = E
Step 4: f :: S -> Maybe (E, S)
Step 5: Decompose S into its components; write f concretely.
```

When you see `fold f e xs = result`:

```
Step 1: fold :: (a -> b -> b) -> b -> [a] -> b
Step 2: xs :: [A]  determines  a = A
Step 3: result :: R  determines  b = R
Step 4: f :: A -> R -> R,  e :: R
Step 5: Check base case: fold f e [] = e. Does this match the desired behaviour?
```

---

## 8. Exam Writing Format

When asked to "find f", present your answer in this exact format:

```haskell
-- Type signature (mandatory)
f :: <derived type>

-- Definition
f <pattern> = <expression>
```

**Example (Q1b)**:
```haskell
f :: (a -> b, [a]) -> Maybe (b, (a -> b, [a]))
f (_, [])     = Nothing
f (g, x:xs)   = Just (g x, (g, xs))
```

**Why the type signature is mandatory**: In 2-mark questions, a correct type signature with a slightly wrong implementation often receives partial credit. A correct implementation with a wrong or missing type signature may lose marks for "lack of clarity".

---

## 9. Self-Test Questions

Test your understanding before the exam:

1. If `unfoldr f (n, m) = [n, n+m, n+2m, ...]`, what is the type of `f`?
   <details><summary>Answer</summary>`f :: (Integer, Integer) -> Maybe (Integer, (Integer, Integer))`</details>

2. If `fold f e xs = concat (map singleton xs)` where `singleton x = [x]`, what are `f` and `e`?
   <details><summary>Answer</summary>`f = \x acc -> [x] ++ acc`, `e = []`</details>

3. Why is `f :: (a -> a, [a]) -> Maybe (a, (a -> a, [a]))` too restrictive for Q1b?
   <details><summary>Answer</summary>It forces `g :: a -> a`, but `map` works for `g :: a -> b`.</details>

---

*Template compiled for Oxford CS1 FP examination style. Covers Q1a-d (unfoldr and fold) and general type derivation methodology.*
