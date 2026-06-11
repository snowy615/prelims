# FP #3 — Algebraic Datatypes & Folds over Trees (BTree, RTree/Rose, BST)

**Frequency: 12/16 real papers. Every recent paper has one full tree question; Q2 is now a fixed slot: keyword question + THREE linear-time invariant checkers + an infinite tree (2025: red-black trees; mock: leftist heaps).**
Appearances: 2012 Q2, 2013 Q4, 2014 Q1/Q3, 2016 Q2/Q3, 2018 Q1, 2019 Q2, 2020 Q1(e,f), 2021 Q2/Q4, 2022 Q3, 2023 Q4, 2024 Q4, 2025 Q2; also 2026 mock Q2.

---

## The datatypes that appear (know all five)

```haskell
data BTree a = Empty | Node (BTree a) a (BTree a)       -- node-labelled
data Tree  a = One a | Two (Tree a) (Tree a)            -- leaf-labelled (2018/19)
data RTree a = Node a [RTree a]                         -- rose tree (2014/16/20/24)
data Natural = Zero | Succ Natural                      -- (2014/17)
data Maybe a = Nothing | Just a
```

**The natural fold replaces each constructor by a function of matching type** — one argument per constructor:

```haskell
foldBTree :: (b -> a -> b -> b) -> b -> BTree a -> b
foldBTree f e Empty        = e
foldBTree f e (Node l x r) = f (foldBTree f e l) x (foldBTree f e r)

foldTree  :: (a -> b) -> (b -> b -> b) -> Tree a -> b   -- leaf trees
foldTree one two (One a)   = one a
foldTree one two (Two l r) = two (foldTree one two l) (foldTree one two r)

foldRTree :: (a -> [b] -> b) -> RTree a -> b
foldRTree f (Node x ts) = f x (map (foldRTree f) ts)

foldNat :: (a -> a) -> a -> Natural -> a
foldNat f e Zero     = e
foldNat f e (Succ n) = f (foldNat f e n)
```

"What makes it natural": `foldBTree Node Empty = id` (folding with the constructors is the identity). This equation is asked verbatim (2018 Q1c, 2019 Q2c).

---

## Stock functions as folds (write instantly)

```haskell
size    = foldTree (const 1) (+)                  -- leaf count
sizeB   = foldBTree (\l _ r -> l + 1 + r) 0
depth   = foldBTree (\l _ r -> 1 + max l r) 0
labels  = foldBTree (\l x r -> l ++ [x] ++ r) []  -- in-order
flatten = foldTree (:[]) (++)                      -- leaves, l-to-r
reflect = foldBTree (\l x r -> Node r x l) Empty
dft (RTree)   = foldRTree (\x ys -> x : concat ys) -- depth-first traversal
sumT  = foldT id sum;  mapT f = foldT (One . f) (Many . map mapT-style)  -- 2020
subs  = foldTree (\a -> [One a]) (\ls rs -> Two-rebuild...)  -- 2018 Q1f: include t itself
```

**BST question set (2013 Q4, 2023 Q4):** insert (compare, recurse one side, O(depth): average O(log n), worst O(n)); delete (leaf / one child / two children → replace by min of right subtree); `flatten` (in-order, sorted) as fold; `build :: [a] -> Tree a` balanced from a sorted list (split in the middle, recurse — naturally an **unfold**); `find` as a fold returning a predicate `a -> Bool`.

**Mutually recursive types (2016 Q3 Zig/Zag):** define functions in mutually recursive pairs; fold takes *two* (f,e) pairs and swaps them at each level. Follow the type — the swap `foldZag (fZag,eZag) (fZig,eZig)` is forced.

**Efficiency upgrade (2019 Q2e, 2016 Q2c, 2018 Q3):** naive `flatten` with ++ is O(n²) worst case (left spine); accumulate instead: `flatten2 ts xs = ...` with `flatten t xs` meaning `flatten t ++ xs` — derive the recursion from that specification. CatList (2018 Q3): represent lists by their concatenation tree; `abs` via accumulator in O(size); `rev` swaps Cat children in O(size); reverse = abs·rev·rep in O(n).

---

## "Can it be written as a fold (on trees)?" — same logic as lists

- A tree fold computes the result at a node from the recursive results only. If two subtrees have equal fold-images but require different behaviour, it is NOT a fold.
- `proper` subtrees (2018 Q1h): NO — `proper (One a)= []` for all a forces the leaf-image to be constant []; but then `proper (Two (One 1) (One 2))` and trees with different leaves… derive a contradiction (the fold cannot reconstruct the subtrees it must return).
- `bfs`/`levels` on rose trees (2024 Q4d): bfs is NOT a foldRose directly (level order interleaves subtree results in a way that loses the grouping)… BUT `levels` can be computed and bfs = concat·levels; if the question allows folding to `[[a]]` with `levels`, bfs becomes expressible — answer what *their* fold type permits, and justify either way with the information argument.
- `levels :: Rose a -> [[a]]` (2024): `levels (Node x ts) = [x] : foldr (zipWith' (++)) [] (map levels ts)` where `zipWith'` keeps the longer tail (longZip). Complexity: O(size · depth) worst case or O(size) with careful merging — discuss as asked.

---

## Invariant-checker questions — now a fixed 15-mark slot (2016 Q2b, 2021 Q4f, 2025 Q2b, 2026 mock Q2b)

Pattern: check a global invariant in **one bottom-up pass returning richer information** (Maybe depth, (Bool, height), (min, max)):
```haskell
depthIfPerfect :: RTree a -> Maybe Int
-- Node: depths of all children equal (and Just) -> Just (1+d); else Nothing
```
**2025 red-black checks (each in linear time):**
- *No red node has a red child:* one pass returning `(ok, colourOfRoot)`; at a red Fork, fail if either child's root colour is Red.
- *Equal black-height on all root-to-Empty paths:* return `Maybe Int` (black height): Empty → `Just 0` (or 1, be consistent); Fork: both children `Just h` with equal h → `Just (h + if black then 1 else 0)`; mismatch → Nothing.
- *BST property:* DO NOT check "i > all of left" by recomputing maxima per node (quadratic trap). Either return `(isBST, min, max)` from each subtree, or check that the in-order `flatten` is strictly increasing — `flatten` is linear (with accumulator) and `increasing (x:y:r) = x < y && increasing (y:r)`.
Mock leftist-heap checks (heap/rank/leftist properties) follow the same `(holds?, minValue / right-spine-length)` pattern. **"For full marks linear time" = return the auxiliary quantity, never recompute it.**

Infinite trees (2025 Q2c, 2026 mock Q2c): corecursive definition, no base case. 2025 (BFS order 0,1,2,…; even black, odd red):
`t n = Fork (colour n) (t (2*n+1)) n (t (2*n+2))` with `colour n = if even n then Black else Red`, tree = `t 0` — heap-style numbering gives BFS order; justify with one sentence.

---

## Traps

- Fold argument order must match constructor field order; give the fold's **most general type** when asked, with reasoning (one type variable per distinct position).
- For folds over `Natural`: addition `add m = foldNat Succ m`, multiplication `mul m = foldNat (add m) Zero`, exponentiation analogous; fib via pairs `(fib n, fib (n+1))` then project (2014 Q1).
- Tree induction = one case per constructor; IH for every recursive field (two IHs for binary nodes, "for all subtrees in ts" for rose trees — use `map`-IH style).
- Quadratic `++` in tree flattening is the most common efficiency follow-up — know the accumulator derivation cold.
