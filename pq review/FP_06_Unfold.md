# FP #6 — unfold / unfoldr (lists and trees)

**Frequency: 7/16 real papers — but unfoldr OPENS the compulsory Q1 in both 2024 and 2025. Treat as top priority despite moderate historical frequency.**
Appearances: 2011 Q1(d)(e), 2014 Q3, 2019 Q2(g), 2021 Q2(f), 2023 Q4(a)(d), 2024 Q1, 2025 Q1(a)(b); also 2026 mock Q1(a)(b).

---

## Memorise verbatim (both versions appear!)

```haskell
-- three-function version (2011, 2014, 2024)
unfold :: (a -> Bool) -> (a -> b) -> (a -> a) -> a -> [b]
unfold p f g x = if p x then [] else f x : unfold p f g (g x)

-- Maybe version, the Prelude's unfoldr (2026)
unfoldr :: (a -> Maybe (b, a)) -> a -> [b]
unfoldr f x = case f x of
  Nothing     -> []
  Just (y, z) -> y : unfoldr f z
```

**Most general type** of `unfold` (asked 2011, 2014): derive it by reading the definition — p consumes the seed to Bool, f produces elements, g transforms the seed.

**Relation to standard functions:** `unfold p f g = map f . takeWhile (not . p) . iterate g` (2014 Q3b).

---

## Express-as-unfold drill (the standard sub-question)

| Function | As unfold |
|---|---|
| identity on lists (2025 Q1a) | `unfoldr f` with `f [] = Nothing; f (x:xs) = Just (x, xs)` |
| `xs ++ ys` from seed `(xs,ys)` (2025 Q1b) | `f ([], []) = Nothing; f ([], y:ys) = Just (y, ([], ys)); f (x:xs, ys) = Just (x, (xs, ys))` — **rebuild the whole pair in the seed** |
| `map f` | `unfold null (f . head) tail` ; unfoldr: `\(g,xs) -> case xs of [] -> Nothing; (y:ys) -> Just (g y, (g, ys))` (2026 mock Q1b) |
| `iterate f` | `unfold (const False) id f` |
| countdown `[n..1]` (2026) | `unfoldr (\n -> if n == 0 then Nothing else Just (n, n-1))` |
| `int2bits` (lsb first, 2011) | `unfold (== 0) (`mod` 2) (`div` 2)` |
| `filter r` (2024) | `unfold p f g` with `g = dropWhile (not.r) . tail`-style: seed = `dropWhile (not.r)`-ed list; `p = null . dropWhile (not.r)`… cleaner: precompose `h = dropWhile (not.r)` and use the **unfold fusion law** — this is exactly 2024 Q1(b)'s intended route |
| `primes` (2024 Q1d) | `unfold (const False) head sieve-step [2..]` — seed = list of remaining candidates; f = head; g = filter out multiples of head |
| `dft` of a rose tree (2014) | seed = list of trees (a stack): `p = null`, `f = root of head`, `g = children of head ++ tail` |
| `toTree`/`build` balanced (2019 Q2g, 2021, 2023) | tree unfold: `unfold :: (b -> Either a (b,b)) -> b -> Tree a`; build: singleton list → Left; else split in half → Right (halves) |

**Tree unfold (2019/2021/2023):**
```haskell
unfoldT :: (b -> Either a (b, b)) -> b -> Tree a
unfoldT p x = case p x of
  Left a       -> One a
  Right (l, r) -> Two (unfoldT p l) (unfoldT p r)
-- build = unfoldT (\xs -> if length xs == 1 then Left (head xs)
--                         else Right (halve xs))
-- BST build from sorted list: split at middle, root = middle element
```

## Unfold fusion law (2024 Q1a — proof; then applied)

`unfold p f g . h = unfold p' f' g'` provided `p·h = p'`, `f·h = f'`, `g·h = h·g'`.
**Proof by the take lemma:** show `take n (unfold p f g (h x)) = take n (unfold p' f' g' x)` for all n, x; induction on n; n+1 case splits on `p (h x) = p' x`; in the false branch heads agree by `f·h = f'` and tails by IH at seed `g' x` using `g (h x) = h (g' x)`.

## "Can it be expressed as an unfold?" (2024 Q1c)

Same information-flow reasoning as folds, mirrored: `unfold` produces the head from the current seed and continues from a new seed; it **cannot decide to stop based on what comes later**, and every produced element comes from one seed step. `singleton`: need one element then stop — seed must flip from "produce" to "stop": with `p x` initially False then True… but the seed type is fixed: use a `Maybe`-wrapped seed. If the seed type may be chosen freely, singleton IS an unfold (`unfoldr (fmap (\y -> (y, Nothing)))` on `Just x`). If forced to keep seed type = element type, argue impossibility. **Match the question's constraints before answering.**

---

## Traps

- Don't confuse the stopping convention: 3-function version stops when `p x` is **True**; some papers define it with the test reversed — restate the definition you're given.
- Defining new seeds: the seed can be a *pair/list* carrying extra state — the dft-as-unfold trick. Mention the seed type explicitly.
- unfold proofs need the **take lemma** (corecursion), not list induction — there is no list to induct on.
- `unfold`-expressible ⟺ the output's each-step head and the rest are functions of a evolving seed; productivity is the corecursive analogue of termination.
