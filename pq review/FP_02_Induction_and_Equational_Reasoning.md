# FP #2 — Structural Induction & Equational Reasoning (incl. the Take Lemma)

**Frequency: 16/16 real papers. The compulsory Q1 now ends with a 10-mark induction proof (2025 Q1e: map (foldl f x)·inits = scanl f x).**
Appearances: 2010 Q2, 2011 Q1(c), 2012 Q1(c)/Q2(c), 2013 Q2, 2014 Q4, 2015 Q1(b), 2016 Q1, 2017 Q4(d), 2018 Q1(h), 2019 Q1, 2020 Q1/Q2, 2021 Q1, 2022 Q1, 2023 Q1, 2024 Q1(a), 2025 Q1(e); also 2026 mock Q1(e).

---

## The list-induction template (use verbatim)

> **Claim:** P(xs) holds for all finite lists xs.
> **Proof by structural induction on xs.**
> **Base case `[]`:** LHS = … = … = RHS, using the definitions of … (name each function whose equation you use, one rewrite per line).
> **Inductive case `x:xs`,** assuming **IH:** P(xs).
> LHS
> = …  { definition of foldr / map / ++ }
> = …  { IH }
> = …  { definition of … }
> = RHS. ∎

Layout rule that earns marks: **one equation per line, with the justification for each step in braces.** Cite "associativity of ++" etc. as named lemmas; if non-standard, prove them as auxiliary lemmas first (questions say "state and prove any auxiliary results").

**When the claim says "all lists" (partial/infinite too):** add the **⊥ case**: P(⊥), which typically needs strictness of the functions involved. (See 2020 Q1b: for *infinite* lists the equation `sum (map (×x) as) = sum as × x` holds trivially because both sides are ⊥ — argue both sides diverge.)

**What must be checked to prove `f xs = g xs` for all lists by induction (2020 Q2d):** the cases `⊥`, `[]`, and `x:xs` assuming it for `xs` — admissibility lets this cover infinite lists too.

**Function equality (2020 Q2c):** `f = g :: a -> b` iff `f x = g x` for every `x :: a` (extensionality).

---

## The Take Lemma (2020 Q2, 2024 Q1a)

> Two lists `xs`, `ys` (partial, finite or infinite) are equal iff `take n xs = take n ys` for **all** natural n.

Use it to prove equalities about **infinite/corecursive** lists (iterate, unfold) where ordinary induction fails. Template:
> Prove by induction on n: `take n (LHS) = take n (RHS)` for all n (and all seeds x — generalise!).
> n = 0: both sides `[]`.
> n+1: unfold one step of each side, e.g. `take (n+1) (map h (iterate g x)) = h x : take n (map h (iterate g (g x)))`, apply hypothesis `h·g = g·h` and the induction hypothesis **at seed g x / h x**.

Worked: `map h . iterate g = iterate g . h` given `h·g = g·h` (2020 Q2g); the unfold fusion law (2024 Q1a): `unfold p f g . h = unfold p' f' g'` given `p·h = p'`, `f·h = f'`, `g·h = h·g'`.

---

## Standard proved results (rehearse the full proofs)

1. **`labels . reflect = reverse . labels`** for binary trees (2012 Q2): tree induction (cases Empty / Node l x r); auxiliary lemma `reverse (xs ++ ys) = reverse ys ++ reverse xs` — prove it too.
2. **`foldl f e (xs ++ ys) = foldl f (foldl f e xs) ys`** (2016 Q1a): induction on xs, **generalising over e** (the IH must be ∀e — say "we prove for all e").
3. **`foldr f e xs = foldl (flip f) e (reverse xs)`** (2016 Q1b): induction using 1.
4. **fold–map fusion / `foldr f e . map g = foldr (f·g) e`** (2015 Q1b): plain induction or by fold fusion.
5. **`x ⊕ foldl (⊗) y xs = foldl (⊗) (x ⊕ y) xs`** given `x ⊕ (y ⊗ z) = (x ⊕ y) ⊗ z` (2013 Q2a): induction on xs generalising y.
6. **`foldl s a xs = ⊥` for infinite xs** (2019 Q1b): every unfolding step is again a foldl on a longer prefix — formally, by induction `foldl` only produces a result at `[]`, never reached; or: foldl is tail-strict.
7. **map (foldl f x) . inits = scanl f x** (2025 Q1e): induction on xs **generalising over x**; auxiliary lemmas `foldl f x . (y:) = foldl f (f x y)` and `map (g·h) = map g · map h` — state both.
8. **map (fold f e) . tails = scanr f e** (2026 mock, mirror of 7): auxiliary `head (scanr f e xs) = fold f e xs`.

## Derivation questions ("derive a more efficient definition", 2016 Q1c, 2017 Q4c, 2016 Q2c)

These are *calculations*, not guesses: start from the specification, e.g. `scanl f e = map (foldl f e) . inits`, case-split on the argument, push `map` through, recognise the recursive call, and present:
> `scanl f e [] = [e]`
> `scanl f e (x:xs) = e : scanl f (f e x) xs`
"You can only receive full credit for a derivation" — show every rewriting step.

---

## Traps

- **Generalise the induction hypothesis** (over the accumulator e/y) whenever the accumulator changes in the recursive call — the single most common reason a foldl proof gets stuck.
- Prove auxiliary lemmas (++ associativity is usually granted; anything about reverse/concat is not).
- For "no type" / impossibility proofs (2018 Q1h): assume expressibility, evaluate both sides on two small inputs that force a contradiction (see FP_01).
- Strictness: `f` strict means `f ⊥ = ⊥`. It is **not** the same as "f evaluates its argument" (2023 Q1a: `f x = x + 0` vs lazy patterns... give: strictness is a semantic property — `const 0` is not strict even though `seq`-style evaluation may occur; canonical answer: `f xs = case xs of _ -> 0`... safest: `f ⊥ = ⊥` is the definition; a function may be strict without ever pattern-matching, e.g. `f x = x`, and `f x = 0` evaluates nothing yet `f undefined = 0 ≠ ⊥`).
- Write `⊥` cases when the statement covers infinite lists; skip them (and say why: "for finite lists only") otherwise.
