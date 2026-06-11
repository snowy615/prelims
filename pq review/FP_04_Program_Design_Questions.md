# FP #4 — The Program-Design Question (tables, games, simulations, IO)

**Frequency: 14/16 real papers — the ~20-mark "build a small program" Q3. Recent style is mathematical (matrices 2024, determinant/Laplace 2025, polynomials in the mock) rather than table-formatting. Less theory, more fluency: it rewards knowing the Prelude.**
Appearances: 2010 Q3/Q4 (expression interpreter; World-Cup table), 2011 Q4 (encryption), 2012 Q4 (medals), 2013 Q3 (word frequency), 2015 Q2/Q3 (Polish notation; rule system), 2016 Q4 (matrices), 2017 Q3 (Sudoku), 2018 Q4 (league table), 2019 Q3 (tic-tac-toe), 2021 Q3 (bell ringing), 2022 Q3 (word ladder), 2023 Q3 (Wordle), 2024 Q2/Q3 (election; matrices), 2025 Q3 (determinant); also 2026 mock Q3 (polynomials).

**2025 Q3 worked core (rehearse):** `get i (x:xs) = if i==1 then x else get (i-1) xs` (undefined on overflow — say why); `omit [] = []; omit (x:xs) = xs : map (x:) (omit xs)`; `minors i m = map (omit-col) (rows-without-i)` = `omit`-on-columns of the matrix with row i deleted: `minors i m = map (map snd) …` simplest: `minors i m = [ map (delete j-th) (deleteRow i m) | j <- [1..n] ]` built from omit: `minors i m = map transposeBack … ` — cleanest: `minors i m = map (\m' -> m') (omit-applied)`: delete row i (`get`-style), then `transpose`, `omit` the columns, `map transpose` back. `det [[x]] = x; det m = sum (zipWith3 (\j a mj -> (-1)^(1+j) * a * det mj) [1..] (head m) (minors 1 m))`. Infinite matrix: `m = [[ f i j | j <- [0..]] | i <- [0..]]`.

---

## The fixed skeleton of these questions

1. **(2–3 marks) Design types.** `type` synonyms for domain names; `data` for enumerations/records.
2. **(2–8 marks each) Small functions** building on each other — the later parts always reuse the earlier ones ("Using part (b), or otherwise…"). Reuse them.
3. **(final, 6–10 marks) Assemble + format/IO** — sorting with a custom order, building display strings, `putStr`.

## Toolbox you must be able to write without thinking

```haskell
-- counting / grouping
count x = length . filter (== x)
nub-like dedup:  removedup = foldr (\x ys -> x : filter (/= x) ys) []
-- sorting by key (no Data.List in some years; insertion sort is acceptable)
sortBy bigger = foldr insert [] where
  insert x [] = [x]
  insert x (y:ys) | bigger x y = x:y:ys
                  | otherwise  = y : insert x ys
-- ranking ties (2012 Q4d): rank of c = 1 + number with strictly higher score
-- lexicographic comparison trick (2018 Q4d): compare on tuples
weight r = (points r, goalDiff r, goalsFor r)   -- tuples order lexicographically!
-- formatting (often the function is given; if not:)
rjustify n s = replicate (n - length s) ' ' ++ s
ljustify n s = s ++ replicate (n - length s) ' '
-- output
makeTable = putStr . unlines . map formatRow
-- transpose (encryption 2011, matrices 2016/2024)
transpose ([]:_) = [];  transpose xss = map head xss : transpose (map tail xss)
-- chunking
rows n [] = [];  rows n s = take n s' : rows n (drop n s')  where s' = pad…
-- matrices: addMat = zipWith (zipWith (+)); multMat a b = [[sum (zipWith (*) r c) | c <- transpose b] | r <- a]
-- Horner (2026): eval p x = foldr (\a acc -> a + x*acc) 0 p
```

## League-table template (2010, 2012, 2018, 2024 — four times!)

1. `type Team = String; data Result/Match = …`
2. Per-team statistics: filter the matches involving the team, fold up (P, W, D, L, F, A): one helper per number, or a single fold accumulating a record.
3. Points: `3*wins + draws` (Olympic score 3/2/1 in 2012; Borda count positions in 2024 — same shape).
4. Order: sort by `(points, goalDiff, goalsFor)` descending; ties alphabetical where stated (2024: *lower* lexicographic first on ties — read the tie rule carefully).
5. Output: header line, then map a formatting function over rows; columns padded to stated widths.

## Game/search template (2017 Sudoku, 2019 tic-tac-toe, 2022 ladder, 2023 Wordle)

- Represent state simply (lists, not arrays).
- Generate: all candidate moves / expansions (list comprehension).
- Test: completion / win predicates.
- Search: `solve = filter complete . expand . choices` (generate-and-test), then *improve* it (prune; the question walks you through it). For shortest-something use **BFS over a tree of states** (2022: `expand` to an infinite tree, bfs, first hit = shortest ladder).
- For 2-player games: recursive exploration of all legal continuations, collecting outcomes (2019 Q3e); inefficiency discussion: repeated states → share/memoise (Q3f).

## Interpreter template (2010 Q3, 2015 Q2)

```haskell
data Op = Add | Sub | Mul | Div
data Expr = Num Int | Var Name | App Op Expr Expr
value env (Num n)       = n
value env (Var v)       = eval env v
value env (App op l r)  = apply op (value env l) (value env r)
-- commands: exec :: Env -> Cmd -> Env, with While via recursion:
exec env (While e c) = if value env e /= 0 then exec (exec env c) (While e c) else env
-- RPN evaluation: foldl with a stack [Int]; push numbers, pop two on operator
```

---

## Traps

- **Read the spec's example output and match it exactly** (column widths, tie-breaking, index from 1, full stops aligned — 2024 Q2d).
- "Using part (x), or otherwise" — using part (x) is the intended 3-line answer.
- Don't use `!!` if forbidden (2024 Q3 says so explicitly); recurse structurally.
- Type signatures on every top-level function — cheap marks, always give them.
- IO appears only at the outermost layer: pure `prepare :: … -> String`, then `putStr . prepare`. Keep everything else pure.
- If a helper like `sortby`/`rjustify` is provided in the question, *use it*; do not re-derive.
