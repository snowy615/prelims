# FP #1 — fold / foldr / foldl, Fold Fusion, and "Can it be written as a fold?"

**Frequency: 16/16 real papers — appears EVERY year, now in the COMPULSORY Q1 (confirmed by 2025). This is the most valuable topic in the entire exam.**
Appearances: 2010 Q2, 2011 Q1, 2012 Q1, 2013 Q1/Q2, 2014 Q4, 2015 Q1/Q2(d), 2016 Q1, 2017 Q4, 2018 Q1, 2019 Q1, 2020 Q1, 2021 Q1, 2022 Q1, 2023 Q1, 2024 Q1(c), 2025 Q1(c)(d)(f)/Q3(b); also 2026 mock Q1(c)(d)(e).

---

## Memorise verbatim (definitions and types — asked for directly, 1–2 marks each)

```haskell
foldr :: (a -> b -> b) -> b -> [a] -> b
foldr f e []     = e
foldr f e (x:xs) = f x (foldr f e xs)

foldl :: (b -> a -> b) -> b -> [a] -> b
foldl f e []     = e
foldl f e (x:xs) = foldl f (f e x) xs

map f      = foldr (\x ys -> f x : ys) []
filter p   = foldr (\x ys -> if p x then x:ys else ys) []
concat     = foldr (++) []
(++ ys)    = \xs -> foldr (:) ys xs        -- xs ++ ys = foldr (:) ys xs
reverse    = foldl (flip (:)) []           -- O(n); foldr version is O(n²)
length     = foldr (\_ n -> 1 + n) 0
compose    = foldr (.) id                  -- 2021
```

`foldr` is **the natural fold** for lists because it replaces the constructors: `foldr f e` substitutes `f` for `(:)` and `e` for `[]`; equivalently `foldr (:) [] = id`.

**Fold fusion law (state it exactly):**
`f . foldr g a = foldr h b` provided
(i) `f` is strict, (ii) `f a = b`, (iii) `f (g x y) = h x (f y)` for all x, y.

**Fold-map fusion:** `foldr g e . map f = foldr (g . f) e`.

---

## Proof of fusion (asked as "briefly outline", 2–4 marks)

Induction over the structure of lists, with cases `⊥`, `[]`, `x:xs`:
- **⊥ case:** LHS = `f (foldr g a ⊥) = f ⊥ = ⊥` needs **strictness of f** (condition i); RHS = `⊥`.
- **[] case:** `f (foldr g a []) = f a = b = foldr h b []` — needs (ii).
- **x:xs case:** `f (foldr g a (x:xs)) = f (g x (foldr g a xs)) = h x (f (foldr g a xs))` by (iii), `= h x (foldr h b xs)` by IH, `= foldr h b (x:xs)`.
Including the ⊥ case makes the result valid for **partial and infinite lists** — that is why strictness is needed. Say this.

## Using fusion to *calculate* a fold (the 3–4 mark "explain how" question, 2021/2022)

To write a given `f :: [a] -> b` as a fold: aim for `f = f . id = f . foldr (:) []`, then fuse: `f` strict, `b = f []`, and find `h` with `f (x : xs) = h x (f xs)` — i.e. show `f (x:xs)` depends on `xs` only through `f xs`.

---

## "Can f be written as a fold?" — the new favourite (2018 Q1h, 2023 Q1, 2024 Q3b, 2025 Q1c/d + Q3b, 2026 mock Q1c/d)

**Key principle:** `foldr f e` computes its result on `x:xs` from `x` and `foldr f e xs` ONLY. So `f` can be a fold iff `f (x:xs)` is determined by `x` and `f xs`.

**Standard impossibility proof (write this shape):**
> Suppose `tail = foldr f e`. Then `tail [1] = []` and `tail [2] = []`, so `f 1 e = f 2 e = []`... now `tail [1,1] = [1]` and `tail [2,1] = [1]`: `f 1 (f 1 e) = f 1 [] = [1]` but also `tail [1,2] = [2] = f 1 (f 2 e) = f 1 []` — the same expression `f 1 []` would need two different values. Contradiction.
Find two inputs with **equal fold-images of the tail but different required results**.

Worked verdicts:
- `filter p` — YES (above). `tails` — YES: `foldr (\x (ys:yss) -> (x:ys):(ys:yss)) [[]]` (head of result is the input). `inits` — YES: `foldr (\x yss -> [] : map (x:) yss) [[]]`.
- `takeWhile p` (2025 Q1c) — YES: `fold (\y acc -> if p y then y:acc else []) []`.
- `dropWhile p` (2025 Q1d) — NO: `dropWhile p [a] = []` for any a with p a; so the fold-image of `[a]` is the same for different a, but `dropWhile p (b:[a])` (with p b True) must return `dropWhile p [a]`… the collision: take p = even; `dropWhile even [2] = [] = dropWhile even [4]`, so `f 2 x' = f 4 x'` forced equal, yet `dropWhile even [3,2] = [3,2]` requires the fold to know the dropped elements. Cleanest witness: `[2]` and `[4]` have equal images `[]`, but `2:[2]→[]` vs… use prepend-witness: results on `[6,2]` = `[]` and `[6,4]` = `[]` fine; the real conflict needs the *kept* case: `[3,2]` → `[3,2]` and `[3,4]` → `[3,4]`: `f 3 (image [2]) = f 3 [] ` must be both `[3,2]` and `[3,4]` — contradiction. (Same two-witness pattern as `tail`.)
- `omit` (2025 Q3b: list of the list-with-ith-element-removed) — YES: `fold (\x acc -> map-based rebuild) [[]]`-style: `omit (x:xs) = xs : map (x:) (omit xs)`, which is literally a fold with `f x acc = xs : map (x:) acc`… careful: it uses `xs` itself, not just `acc` — so as stated it is NOT a plain fold; answer NO with this observation, or strengthen to a paired fold returning `(xs, omit xs)` and project (tupling trick). State which the question permits.
- `tail`, `ptails`, `pinits` (proper versions) — NO: they discard exactly the information the fold no longer has (e.g. `ptails xs` and `ptails ys` can be equal while `xs ≠ ys` need different one-step results). 2023 Q1 tests exactly this trio.
- `proper` subtrees (2018), `bfs` on rose trees (2024) — NO; same two-witness argument.
- `insert` into a BST (2023 Q4g) — NO as a tree fold (insertion needs the path, not recursive results on both subtrees... actually argue: fold recurses into *both* subtrees; insert must leave one untouched — the fold image of a subtree does not determine the subtree). YES if the fold returns the tree itself (id-like fold) — match your argument to your fold's type.
- `zipWith f` — YES with a functional accumulator: `foldr (\x r ys -> case ys of [] -> []; (y:ys') -> f x y : r ys') (const []) xs` applied to the second list. `flip (zipWith f)` — same trick on the other argument (2022 Q1f/g).
- `singleton x = [x]` as **unfold** — NO (an unfold producing `[x]` must produce the head from the seed and then stop; possible? 2024 Q1c: argue with the fusion/definition — actually singleton IS expressible: `unfold p f g x` with a flag... if the question says justify, test small cases first!). Always *try to construct first*, only then prove impossibility.

---

## foldr vs foldl (2013 Q2, 2016 Q1, 2019 Q1)

- `foldl f e xs = ⊥` for every infinite `xs` (it must reach the end). Hence no `c, b` can make `foldr c b = foldl s a` on infinite lists unless `foldr` also diverges.
- Duality: `foldr f e xs = foldl (flip f) e (reverse xs)` (finite xs); if `f`/operators associate appropriately (`x ⊕ (y ⊗ z) = (x ⊕ y) ⊗ z` and `x ⊕ a = a ⊗ x`) then `foldr (⊕) a = foldl (⊗) a` (2013 Q2).
- `foldl s a xs = foldr (k s) id xs a` where `k s x f y = f (s y x)` (2019 Q1g) — the "fold with functional accumulator" trick; prove by induction generalising over `a`.
- Efficiency: `concat` via foldr is O(total); via foldl it is O(n·m²)-ish (left-nested (++) recopies) — 2013 Q2e.

## scanl / scanr (2017 Q4, 2025 Q1e, 2026 mock Q1e)

`scanl f e = map (foldl f e) . inits`; efficient form: `scanl f e xs = e : case xs of [] -> []; x:xs' -> scanl f (f e x) xs'` — derive it by unfolding `inits` (the derivation is the question; little credit for just stating the result).
**2025 Q1e (10 marks): prove `map (foldl f x) . inits = scanl f x`.** Induction on xs, **generalising over the accumulator x** (the IH must hold for all x, because the recursive call is `scanl f (f x y) ys`). Inductive step: `map (foldl f x) (inits (y:ys)) = map (foldl f x) ([] : map (y:) (inits ys)) = x : map (foldl f x . (y:)) (inits ys)` — auxiliary lemma: `foldl f x . (y:) = foldl f (f x y)` (immediate from foldl's definition, but STATE it) and map-functor law `map (g.h) = map g . map h` — then apply IH at accumulator `f x y`.
`scanr f e = map (foldr f e) . tails` — the mock's mirror version: inductive case uses auxiliary lemma `head (scanr f e xs) = foldr f e xs`.
Complexity (2-mark closer, 2025 Q1f): LHS Θ(n²) (foldl from scratch on every prefix), RHS Θ(n) (each element processed once).

---

## Traps

- Quote fusion's three conditions **in full** including strictness; check strictness explicitly when applying it.
- In induction proofs, include the ⊥/partial-list case whenever the claim says "all lists" (not just finite).
- `flip f x y = f y x` — define it before using.
- When asked for "most general type", give context too: e.g. `foldr :: (a -> b -> b) -> b -> [a] -> b`, no `Eq` unless used.
- Show the *calculation steps* (= equational reasoning) — answers that jump to the result lose most of the marks.
