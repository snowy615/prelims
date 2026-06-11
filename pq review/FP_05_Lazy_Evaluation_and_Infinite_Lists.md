# FP #5 — Lazy Evaluation, Infinite Lists & Cyclic Structures

**Frequency: 11/16 real papers. New-format papers end Q2 and Q3 with a 3-mark "build this infinite structure" (2025: infinite tree in BFS order, infinite matrix m!!i!!j = f i j).**
Appearances: 2012 Q3 (interleave/all pairs), 2013 Q1(c) (Pascal), 2015 Q4 (sieve, triples), 2017 Q1(c)/Q2 (pascal, hamming), 2018 Q2 (power series), 2019 Q4 (interleave, cp), 2020 Q2 (take lemma), 2022 Q4 (numerics), 2023 Q2(c), 2024 Q1(d) (primes via unfold), 2025 Q2(c)/Q3(e); also 2026 mock Q2(c)/Q3(e).

---

## Memorise verbatim

**Lazy evaluation:** expressions are evaluated only when needed (call-by-need: at most once, results shared), and only as far as needed (to weak head normal form). Key properties: enables **infinite data structures**; never takes more steps than eager evaluation; terminates whenever any strategy does.

```haskell
iterate f x = x : iterate f (f x)
takeWhile, dropWhile, zipWith, repeat, cycle
nats = [1..] ;  fibs = 0 : 1 : zipWith (+) fibs (tail fibs)
primes = sieve' [2..] where sieve' (p:xs) = p : sieve' [x | x <- xs, x `mod` p /= 0]
sieve (x:xs) = [y | y <- xs, y `mod` x /= 0]      -- 2015 wording
pascal = iterate (\row -> zipWith (+) (0:row) (row ++ [0])) [1]   -- 2013/2017
hamming = 1 : merge (map (2*) hamming) (merge (map (3*) hamming) (map (5*) hamming))
merge (x:xs) (y:ys) | x < y = x : merge xs (y:ys)
                    | x > y = y : merge (x:xs) ys
                    | otherwise = x : merge xs ys      -- distinct-output merge!
```

---

## The fair-interleaving cluster (2012 Q3, 2019 Q4 — near-identical questions)

- `interleave (x:xs) ys = x : interleave ys xs; interleave [] ys = ys` — alternates, works for finite/infinite; every element appears at a finite position (element i of xs appears by position 2i). **Say the position bound** — that is the "briefly justify" mark.
- **Why `[(x,y) | x <- xs, y <- ys]` fails for infinite ys:** the comprehension exhausts all ys-pairs for the first x first, so (x₂, y₀) is never reached.
- `interleaveList :: [[a]] -> [a]`: `foldr (\xs r -> interleave xs r)`-style or diagonal enumeration; each element of list j must land at a finite index — diagonalisation (enumerate by anti-diagonals) is the safe answer:
  `allpairs2 xs ys = interleaveList [[ (x,y) | y <- ys ] | x <- xs]`.
- `cp :: [[a]] -> [[a]]` with infinite element lists (2019 Q4g): recursive diagonal product — combine `[ y:zs | … ]` via the fair pair function from the earlier part. Build on earlier parts.
- Enumerate all finite trees / all triples (2012 Q3g, 2015 Q4b): enumerate by *size*; or trees = diagonal product of smaller trees.
- **Partial lists break fairness** (2019 Q4c): if xs is partial, interleave eventually hits ⊥ and the rest of ys is lost — answer NO with this reason.

## Cyclic structures (2017 Q2)

`hamming` defined *in terms of itself* — sharing means each element is computed once: computing the first n elements takes O(n) merges ⇒ **O(n)** steps overall (each step advances one of three pointers into the same shared list). Draw the diagram if asked: one list cell chain with three "multiplier taps" pointing back into it.
Generalisation `hamming' as = 1 : foldr1 merge [map (a*) (hamming' as) | a <- as]`.

## Power series & numerics as infinite lists (2018 Q2, 2022 Q4)

- Series = infinite coefficient list; `plus = zipWith (+)`; `deriv as = zipWith (*) [1..] (tail as)`; `integral as = 0 : zipWith (/) as [1..]`.
- **Self-referential definition works iff it is *productive***: `expx = one + integral expx` works because `integral` outputs its first element (the constant 0…) before consuming — each element depends only on earlier ones. `expx = deriv expx` does NOT (deriv needs element i+1 to emit element i). This productivity argument is the asked-for justification.
- `sinx = integral cosx; cosx = one - integral sinx` — mutually recursive, productive.
- Numeric limits (2022): `within eps (x:y:rest) = if abs (x-y) < eps then y else within eps (y:rest)`; sequence acceleration by indexing `2^i`; Newton–Raphson as `iterate step x0` then `within eps`.
- `zero == zero` for infinite lists **diverges** (⊥): (==) compares forever — 2018 Q2d.

## Infinite trees and matrices (2025 Q2c/Q3e, 2026 mock Q2c/Q3e)

Corecursion without base case: `t n = Fork (colour n) (t (2n+1)) n (t (2n+2))` (heap numbering ⇒ BFS order 0,1,2,…); infinite matrix `[[f i j | j <- [0..]] | i <- [0..]]`; `pascalPolys = iterate (pmul [1,1]) [1]`. These are 3-mark closers — write them in one line and move on.

---

## Traps

- The sieve must *not* test primality by division by all earlier numbers — filter by the head only, recursively.
- `merge` for hamming must drop duplicates (the `otherwise` case consumes both) — the plain sorted merge is wrong here.
- Justify "every element appears at a finite position" for any fair enumeration — it's always worth a mark or two.
- Productivity, not termination, is the correctness notion for corecursive definitions; name it.
- `take`-based experiments are how you check your own answer in the exam (mentally run `take 6 …` — 2012 Q3 literally asks for the first 6/8 elements).
