# FP #8 — Representing Data by Functions (sets, sequences as functions)

**Frequency: 3/16 real papers — but when it appears it is a FULL 20-mark question, and it recurs in a clear cycle (2010, 2011, 2023). Worth one focused practice session.**
Appearances: 2010 Q1 (sets as predicates), 2011 Q2 (sequences as position functions), 2023 Q2 (same representation again, 12 years later), plus 2023 Q4(e) (`find` turns a tree into a predicate).

---

## Representation 1: sets as characteristic functions (2010 Q1)

```haskell
type Set a = a -> Bool
empty      = const False
addItem x s = \y -> y == x || s y            -- needs Eq a
union s t   = \y -> s y || t y
intersect s t = \y -> s y && t y
-- a set from a list:  member xs = \y -> y `elem` xs
-- anagrams s = member (perms s)
-- powers n = \m -> m `elem` takeWhile (<= m) (iterate (*n) n)   -- infinite set: bound the search!
```

**The discussion sub-question (2 marks, always there):** operations you CANNOT define for function-represented sets — anything that must *enumerate* the members: size/cardinality, listing elements, testing emptiness/equality of sets, folding over the set, minimum… because the function can only be *queried* pointwise on a possibly infinite domain.

**Infinite-domain trick:** to test membership of m in an infinite generated set, generate in increasing order and `takeWhile (<= m)` — guarantees termination. This trick is the difference between a pass and full marks here.

## Representation 2: sequences as position functions (2011 Q2, 2023 Q2)

```haskell
type Seq a = a -> [Int]        -- value -> increasing list of positions where it occurs
rep :: Eq a => [a] -> Seq a
rep xs v = [i | (i, x) <- zip [0..] xs, x == v]
empty v = []
cons x s = \v -> if v == x then 0 : map (+1) (s v) else map (+1) (s v)
-- take n / drop n (2023):
takeS n s = \v -> filter (< n) (s v)
dropS n s = \v -> map (subtract n) (filter (>= n) (s v))
-- evens / odds (alternate elements):
evensS s = \v -> [ i `div` 2 | i <- s v, even i ]
oddsS  s = \v -> [ i `div` 2 | i <- s v, odd  i ]
-- interleave (ilv): positions from the first double, from the second double+1, merged:
ilv s t = \v -> merge (map (2*) (s v)) (map (\i -> 2*i+1) (t v))
-- length (2011, given finite list elems of all possible values):
lengthS s = sum [ length (s e) | e <- elems ]
-- nats: rep [0..] = \v -> [v]  (for v >= 0)
```

Keep the **invariant**: returned position lists are finite-or-infinite but always *increasing* — preserve it in every operation (use a sorted merge, never ++ out of order).

**Equality discussion (2023 Q2c/f):** two Seq values are equal as functions iff they agree on every input; `rep [0..]` vs `\v -> [v]`: equal on naturals but `rep` diverges on inputs not in the list (searches forever) — a definedness difference; making them equal requires bounding the search, which needs an enumeration order on positions, possible when the list is sorted/known. `ilv (evens xs) (odds xs) = xs`: yes pointwise — prove by chasing both maps on an arbitrary v.

## The connecting idea (worth stating in any of these questions)

A function representation is an **abstract data type given by its observations**: you can answer queries but not invert them. Constructors become function-builders (compose a new query out of old ones); anything requiring enumeration of the domain is lost. The BST `find` fold (2023 Q4e) is the same move in reverse: collapse a data structure into its query function `a -> Bool`.

---

## Traps

- Every operation must return *another function* — beginners try to pattern-match on the function; you can only **apply** it.
- Type contexts: equality tests on elements force `Eq a` in the signatures — include them.
- Position indexing: 2011 starts at 1, 2023 starts at 0 — read the spec.
- For infinite sequences/sets, justify termination of every search (increasing order + takeWhile/filter bound).
- 2-mark discussion parts have model answers above (cannot enumerate / definedness) — don't improvise vaguely.
