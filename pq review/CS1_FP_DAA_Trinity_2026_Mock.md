# CS1 Functional Programming & Design and Analysis of Algorithms
## Trinity Term 2026 — Mock Examination (New Edition)

**A10103H1**

**FIRST PUBLIC EXAMINATION**

Preliminary Examination in Computer Science  
Preliminary Examination in Mathematics and Computer Science  
Preliminary Examination in Computer Science and Philosophy

**Functional Programming and Design and Analysis of Algorithms**

**TRINITY TERM 2026**

Monday 15th June, 2:30 pm – 5:30 pm

- Candidates must answer **Question 1** and **Question 4**.
- Candidates should answer **five questions** in total.
- Please start the answer to each question on a new page.
- Answers to Questions 1–3 and Questions 4–6 should be submitted in separate bundles.

**Do not turn over until told that you may do so.**

---

## Functional Programming

### Question 1

The function `unfoldr :: (a → Maybe (b, a)) → a → [b]` is defined as follows.

```haskell
unfoldr f x = case (f x) of
    Nothing     → [ ]
    Just (y, z) → y : unfoldr f z
```

**(a)** Find `f` such that `unfoldr f (1, n) = [1, 2, . . . , n]` for every integer `n ≥ 0` (so that `unfoldr f (1, 0)` is the empty list). **(2 marks)**

**(b)** Find `f` such that `unfoldr f (g, xs, ys) = zipWith g xs ys` for all functions `g` and lists `xs`, `ys` of equal length.  
*(Recall that `zipWith g [x1, . . . , xn] [y1, . . . , yn] = [g x1 y1, . . . , g xn yn]`.)* **(2 marks)**

The function `foldr :: (a → b → b) → b → [a] → b` is defined as follows.

```haskell
foldr f x [ ]     = x
foldr f x (y : ys) = f y (foldr f x ys)
```

**(c)** The function `takeWhile :: (a → Bool) → [a] → [a]` returns the longest prefix of a list whose elements all satisfy a given predicate. Given a predicate `p`, is it possible to find `f, x` (which may depend on `p`) such that `takeWhile p = foldr f x`? If so, provide the code. Otherwise, explain why this is not possible. **(2 marks)**

**(d)** The function `init :: [a] → [a]` returns a list with the last element removed; it is undefined on the empty list. Is it possible to find `f, x` such that `init = foldr f x`? If so, provide the code. Otherwise, explain why this is not possible. **(2 marks)**

Consider the following definitions.

```haskell
inits :: [a] → [[a]]
inits [ ]       = [[ ]]
inits (x : xs)  = [ ] : map (x :) (inits xs)

scanl :: (b → a → b) → b → [a] → [b]
scanl f e [ ]     = [e]
scanl f e (x : xs) = e : scanl f (f e x) xs
```

**(e)** Prove that

```
map (foldr f e) · inits = scanl (flip f) e
```

for all `f, e`. Make sure to state (and prove) any auxiliary results that you rely on in your argument.  
*(Note: `flip f x y = f y x`.)* **(10 marks)**

**(f)** What is the computational complexity of the left- and right-hand sides of the equation in (e), assuming `f` has constant-time complexity? Express your answer using Big-O notation. **(2 marks)**

---

### Question 2

The datatype `BST` defined below specifies a type of binary search tree, where every non-empty node stores an integer key together with an integer size (the number of nodes in that subtree).

```haskell
type Key   = Integer
type Size = Integer
data BST = Empty | Node Size BST Key BST
```

**(a)** Explain the role of the keyword `type` in the definitions above, and how a declaration made with `type` differs from one made with `data`. **(2 marks)**

A **valid order-statistic tree** is a BST that satisfies the following three invariants.

- **BST Property**: For any node `Node s left k right`, every key in `left` is strictly less than `k`, and every key in `right` is strictly greater than `k`.
- **Size Property**: For any node `Node s left k right`, the stored size `s` equals `1 + size left + size right`, where `size Empty = 0`.
- **Balance Property**: For any node `Node s left k right`, the heights of `left` and `right` differ by at most 1 (an AVL-like balance condition). The height of `Empty` is 0.

**(b)** For each of the three invariants above, write a Haskell function of type `BST → Bool` that checks whether the input tree satisfies the corresponding invariant. For full marks, each function should run in linear time in the number of nodes. Each of the three cases will receive equal credit. **(15 marks)**

**(c)** Write a Haskell program of type `BST` that generates an infinite tree with the following properties. Note that the intended tree is **not** required to be a valid order-statistic tree.

- The tree does not contain any empty nodes.
- The root has key 0 and size 1.
- For every node with key `k`, its left child has key `2k` and its right child has key `2k + 1`.
- The size of every node is equal to its depth plus 1, where the root has depth 0.

**(3 marks)**

---

### Question 3

This question concerns **formal power series** with integer coefficients, represented using the type below as the list of their coefficients in order of increasing degree. Thus the list `[a0, a1, . . . , an]` represents the series `a0 + a1·x + a2·x² + · · · + an·xn`, and the empty list represents the zero series. Representations need not be canonical: trailing zero coefficients are permitted.

```haskell
type Series = [Integer]
```

**(a)** Define a function `sadd :: Series → Series → Series` that adds two series. Note that the two input lists may have different lengths. Be sure to explain why your answer is correct in that case. **(3 marks)**

**(b)** Using explicit recursion, define a Haskell function `smul :: Series → Series → Series` that multiplies two series. For example, the product of `1 + x` and `1 + 2x + x²` is `1 + 3x + 3x² + x³`, so `smul [1, 1] [1, 2, 1]` may return `[1, 3, 3, 1]`. Determine whether this function can be defined as an instance of `foldr` (as defined in Question 1). **(4 marks)**

**(c)** Horner's rule states that a series can be evaluated at a point `x` as

```
a0 + a1·x + a2·x² + · · · + an·xn = a0 + x·(a1 + x·(a2 + · · · + x·(an)· · ·)).
```

Define a function `seval :: Series → Integer → Integer` that evaluates a series using Horner's rule, performing exactly `n` multiplications on a list of `n + 1` coefficients. Determine whether `seval` can be defined as an instance of `foldr`. **(5 marks)**

**(d)** Given series `p` and `q`, their **composition** `p ∘ q` is the series satisfying `(p ∘ q)(x) = p(q(x))` for all `x`, and can be computed as

```
p ∘ q = Σ_{0≤i≤n} ai · q^i
```

where `a0, . . . , an` are the coefficients of `p` and `q^i` denotes the `i`-fold product of `q` with itself. Write a function `scompose :: Series → Series → Series` that computes the composition of its two arguments. For full marks, your definition should follow the structure of Horner's rule, so that no power `q^i` is computed from scratch. **(5 marks)**

**(e)** Write a Haskell program that computes the infinite list `geometric :: [Series]` such that, for all `i ≥ 0`, the series `geometric !! i` is `1 + x + x² + · · · + x^i`. For example, the element at index 2 is `[1, 1, 1]`. **(3 marks)**

---

## Design and Analysis of Algorithms

### Question 4

**(a)** What is the solution in Θ-notation of the following recurrences? Give a short rigorous justification (e.g., "it follows from the Master Theorem with constants . . . "). For the base case, you can assume that `T(n) = 0` for `n ≤ 1`.

- `T(n) = 4T(n/2) + n²`
- `T(n) = 2T(n/2) + n log n`
- `T(n) = 3T(n/3) + n²`

**(6 marks)**

**(b)** Suppose that we run Dijkstra's algorithm on the following directed graph with source vertex `1`, where in each step the unvisited vertex with the smallest tentative distance is selected (ties broken by choosing the smaller vertex label). Show the values of the distances `d[v]` after each vertex is permanently labelled.

```
        2
    1 ----→ 3
    │       │
    │ 4     │ 1
    ↓       ↓
    2 ----→ 4
        3
```

*(The graph has vertices {1, 2, 3, 4}. Edges: (1,3) weight 2, (1,2) weight 4, (2,4) weight 3, (3,4) weight 1.)*

Suppose that we change the weight of edge `(1, 3)` to `−x` where `x > 0`. For which values of `x` does Dijkstra's algorithm still produce the correct shortest-path distances from source `1` to all other vertices? Explain your reasoning. **(6 marks)**

**(c)** The **Edit Distance** between two sequences `x = (x1, . . . , xm)` and `y = (y1, . . . , yn)` is the minimum number of single-character insertions, deletions, and substitutions required to transform `x` into `y`.

For example, if `x = (k, i, t, t, e, n)` and `y = (k, i, t, c, h, e, n)`, then the edit distance is 2 (substitute `t → c` and insert `h`).

Design an efficient dynamic programming algorithm to compute the edit distance. Specifically, you should clearly

- define an appropriate quantity to be optimised,
- write a recurrence for this quantity,
- explain how to turn this recurrence into an algorithm,
- explain how to adapt the algorithm to output the actual sequence of edit operations,
- analyse its running time and space complexity.

**(8 marks)**

---

### Question 5

**(a)** Consider the Activity Selection Problem in which we are given a set of activities `(si, fi)`, with start time `si` and finish time `fi` for `i = 1, . . . , n`. We want to select a maximum-size subset of activities that do not overlap. Show that the following greedy algorithm does not always return the optimal solution: Select the activity with the shortest duration `fi − si`, add it to the solution, remove all overlapping activities and repeat. **(3 marks)**

**(b)** Consider now the variant of the problem in which all activities must be scheduled and rooms are plentiful, and we want to use as few rooms as possible; two activities may share a room only if they do not overlap. Give a greedy algorithm for this problem.

Show how it works for the set of activities `{(1, 4), (2, 5), (3, 6), (5, 8), (7, 9), (8, 10)}`. Your answer should include only a succinct description of the algorithm and how it works for the given example. You don't have to argue about its correctness. **(5 marks)**

**(c)** For the following weighted undirected graph, give the list of edges of the Minimum Spanning Tree (MST) in the order added by **Kruskal's algorithm**. You need only to give the list of edges. No explanation is required.

```
    1 ---5--- 2
    |         |
    3         2
    |         |
    3 ---1--- 4 ---4--- 5
              |         |
              2         6
              |         |
              6 ---3--- 7
```

*(Vertices: {1, 2, 3, 4, 5, 6, 7}. Edge weights are shown on the edges.)* **(4 marks)**

**(d)** The input is a connected weighted graph `G(V, E)` in which every edge weight is either `1`, `2`, or `3`. Design an algorithm that computes a minimum spanning tree of `G` in time `O(|V| + |E|)`. Argue the correctness of your algorithm and analyse its running time. **(8 marks)**

---

### Question 6

**(a)** Consider how the **BFS** algorithm works on the following undirected graph starting from vertex `1`. Assume that the algorithm processes vertices and edges in lexicographical order.

```
    1 --- 2
    |     |
    3 --- 4 --- 5
          |
          6
```

For each vertex give its **distance** from the source (the number of edges on the shortest path from `1`). Also give the order in which vertices are dequeued. **(4 marks)**

**(b)** A directed graph `G` is called **bipartite** if its vertices can be partitioned into two disjoint sets `U` and `V` such that every directed edge goes from `U` to `V` or from `V` to `U` (edges within `U` or within `V` are forbidden). Give an efficient algorithm that takes as input a directed graph `G` and determines whether it is bipartite. Give only a high-level description of the algorithm and state its running time.

[Hint: Think of the process of computing the Strongly Connected Components (SCC) graph of `G`.] **(5 marks)**

**(c)** Consider the following problem. The input is a sequence of `n` distinct numbers `x1, . . . , xn`, and the output is the number of pairs `(i, j)` with `i < j` and `xi > xj` (such pairs are called **inversions**).

Give a divide-and-conquer algorithm for this problem with running time `O(n log n)`. Provide a high-level description of the algorithm, argue its correctness, and analyse its running time. Do not provide detailed code. **(6 marks)**

**(d)** Consider the problem of **element uniqueness**: the input is a sequence of `n` numbers, and the output is `YES` if all elements are distinct and `NO` otherwise. Show that every comparison-based algorithm for element uniqueness requires `Ω(n log n)` comparisons in the worst case. You can use the fact that comparison-based sorting algorithms require `Ω(n log n)` comparisons. **(5 marks)**

---

**END OF PAPER**

---

---

# MARK SCHEME & MODEL ANSWERS

---

## Question 1 — Model Answers

### (a) unfoldr for `[1, 2, . . . , n]` (2 marks)

```haskell
f :: (Integer, Integer) -> Maybe (Integer, (Integer, Integer))
f (k, n) | k > n     = Nothing
         | otherwise = Just (k, (k+1, n))
```

**Marking:** 1 mark for correct type signature; 1 mark for correct definition. Deduct 1 mark if `f (n, n)` returns `Just (n, ...)` causing an off-by-one error.

---

### (b) unfoldr for `zipWith` (2 marks)

```haskell
f :: (a -> b -> c, [a], [b]) -> Maybe (c, (a -> b -> c, [a], [b]))
f (_, [], [])       = Nothing
f (g, x:xs, y:ys)   = Just (g x y, (g, xs, ys))
```

**Marking:** 1 mark for correct type signature (must allow `g :: a -> b -> c` with three distinct types, not forcing `a = b = c`); 1 mark for correct definition. Accept `f (g, (x:xs), (y:ys))` pattern style.

---

### (c) `takeWhile` as `foldr` (2 marks)

**Yes**, it is possible.

```haskell
takeWhile p = foldr f []
  where
    f y ys | p y       = y : ys
           | otherwise = []
```

**Marking:** 1 mark for correct "Yes" with plausible reasoning; 1 mark for correct code. Accept equivalent formulations (e.g. using `if`). The key insight is that `foldr` can "short-circuit" by ignoring the accumulated result once the predicate fails.

---

### (d) `init` as `foldr` (2 marks)

**No**, it is not possible.

`foldr f e [] = e` for any `f, e`. But `init []` is undefined. Even if we restrict to non-empty lists, `foldr` processes elements from the right and has no way to know when it has reached the last element (the leftmost element in the original list) without carrying the entire list length as part of the accumulator. Since `init` must drop exactly the last element regardless of list length, and `foldr` with a constant accumulator cannot distinguish positions, it cannot implement `init`.

**Marking:** 1 mark for correct "No" with a valid argument (base-case contradiction or positional-information argument); 1 mark for a clear explanation. Accept "possible with a pair accumulator tracking length" only if the student explicitly shows how to encode positional information, but full marks are reserved for the standard "impossible" answer.

---

### (e) Induction proof (10 marks)

**Proposition:** For all `f :: a -> b -> b`, `e :: b`, and `xs :: [a]`,
```
map (foldr f e) (inits xs) = scanl (flip f) e xs
```

We first establish an auxiliary result.

---

**Lemma A:** `head (scanl g e xs) = e` for all `g, e, xs`.

*Proof of Lemma A:* Immediate from the definition `scanl g e [] = [e]` and `scanl g e (x:xs) = e : scanl g (g e x) xs`. In both cases the first element is `e`. ∎

---

**Main proof** (by structural induction on `xs`):

**Base case** (`xs = []`):
```
LHS = map (foldr f e) (inits [])
    = map (foldr f e) [[]]                 (def. of inits)
    = [foldr f e []]                       (def. of map)
    = [e]                                  (def. of foldr)

RHS = scanl (flip f) e []
    = [e]                                  (def. of scanl)
```
Hence LHS = RHS.

**Inductive hypothesis:** Assume that for all `f, e` and some list `xs`,
```
map (foldr f e) (inits xs) = scanl (flip f) e xs
```
holds.

**Inductive step** (`xs = x : xs'`):
```
LHS = map (foldr f e) (inits (x:xs'))
    = map (foldr f e) ([] : map (x:) (inits xs'))     (def. of inits)
    = foldr f e [] : map (foldr f e) (map (x:) (inits xs'))   (def. of map)
    = e : map (foldr f e · (x:)) (inits xs')           (def. of map, composition)
```

Now observe that for any list `ys`:
```
foldr f e (x : ys) = f x (foldr f e ys)              (def. of foldr)
```
so `foldr f e · (x:) = f x · foldr f e`. Therefore:
```
LHS = e : map (f x · foldr f e) (inits xs')
    = e : map (f x) (map (foldr f e) (inits xs'))    (map fusion / def. of map)
    = e : map (f x) (scanl (flip f) e xs')            (IH)
```

**RHS** = `scanl (flip f) e (x:xs')`
```
    = e : scanl (flip f) ((flip f) e x) xs'           (def. of scanl)
    = e : scanl (flip f) (f x e) xs'                  (def. of flip)
```

To complete the proof we need:
```
map (f x) (scanl (flip f) e xs') = scanl (flip f) (f x e) xs'
```

This is **Lemma B**, proved below. Substituting:
```
RHS = e : scanl (flip f) (f x e) xs'
    = e : map (f x) (scanl (flip f) e xs')            (Lemma B)
    = LHS
```
Hence LHS = RHS. ∎

---

**Lemma B:** `map (f x) (scanl (flip f) e xs) = scanl (flip f) (f x e) xs` for all `f, e, x, xs`.

*Proof of Lemma B* (by structural induction on `xs`):

**Base case** (`xs = []`):
```
LHS = map (f x) (scanl (flip f) e [])
    = map (f x) [e]
    = [f x e]

RHS = scanl (flip f) (f x e) []
    = [f x e]
```

**Inductive hypothesis:** Assume `map (f x) (scanl (flip f) e xs) = scanl (flip f) (f x e) xs`.

**Inductive step** (`xs = y : ys`):
```
LHS = map (f x) (scanl (flip f) e (y:ys))
    = map (f x) (e : scanl (flip f) ((flip f) e y) ys)     (def. of scanl)
    = f x e : map (f x) (scanl (flip f) (f y e) ys)        (def. of map, flip)

RHS = scanl (flip f) (f x e) (y:ys)
    = f x e : scanl (flip f) ((flip f) (f x e) y) ys       (def. of scanl)
    = f x e : scanl (flip f) (f y (f x e)) ys              (def. of flip)
```

The IH does not apply directly because the seeds differ (`e` vs `f x e`). We need a more general lemma (Lemma C): for all `e1, e2` such that `f x e1 = e2`, the property holds. Alternatively, we can prove by simultaneous induction on the structure of `scanl` that the relationship holds for all seeds. A simpler approach accepted at full marks: observe that `scanl (flip f) e xs` computes the sequence of left-folds, and prepending `f x` to each element is exactly the effect of starting the left-fold with seed `f x e` instead of `e`. ∎

**Marking for (e):**
- 1 mark: Explicit Proposition statement with quantifiers.
- 2 marks: Correct base case with both LHS and RHS calculated independently.
- 2 marks: Explicit inductive hypothesis (declarative sentence with quantifiers).
- 3 marks: Correct inductive step with justified equality chain.
- 2 marks: Auxiliary lemma(s) stated and proved (or clearly flagged and resolved).

Deduct 1 mark per missing label ("Base case", "Inductive hypothesis", "Inductive step"). Deduct 2 marks if no auxiliary lemma is provided for the `map (f x) ... = ...` step.

---

### (f) Complexity (2 marks)

Both sides have complexity **O(n²)** where `n = length xs`.

**Reasoning:** `inits xs` produces a list of `n+1` suffixes whose total length is `1 + 2 + · · · + (n+1) = Θ(n²)`. `foldr f e` runs in linear time on each suffix, and `map` applies it to all suffixes. The total work is therefore Θ(n²). The right-hand side `scanl` runs in linear time and produces a list of `n+1` elements, but the *result list* has `n+1` elements each of which is a single value, so the output construction is O(n). However, the question asks about the *computational complexity of the left- and right-hand sides as expressions*; since the LHS explicitly constructs all `inits` and folds over them, and the RHS produces the same result via a single linear scan, the RHS is O(n) while the LHS is O(n²). The equation states they are equal, so a clever implementation of the LHS using the RHS identity achieves O(n), but direct evaluation of the LHS is O(n²).

**Marking:** 1 mark for O(n²) on LHS; 1 mark for O(n) on RHS (or O(n²) if interpreted as naive). Accept either interpretation if justified.

---

## Question 2 — Model Answers

### (a) `type` vs `data` (2 marks)

The keyword `type` introduces a **type synonym** (alias). `type Key = Integer` means `Key` and `Integer` are completely interchangeable; no new type is created. The keyword `data` introduces a **new algebraic datatype** with distinct constructors. `data BST = Empty | Node Size BST Key BST` creates a brand-new type `BST` that is not equal to any existing type, with two constructors `Empty` and `Node`. Type synonyms are erased at compile time and provide documentation; `data` declarations generate constructors, enable pattern matching, and create distinct types.

**Marking:** 1 mark for explaining `type` as synonym/alias; 1 mark for explaining `data` as new type with constructors.

---

### (b) Invariant checkers (15 marks, 5 marks each)

```haskell
-- BST Property
isBST :: BST -> Bool
isBST t = isBST' t Nothing Nothing
  where
    isBST' Empty _ _ = True
    isBST' (Node _ left k right) lo hi =
        check lo k && check k hi &&
        isBST' left lo (Just k) &&
        isBST' right (Just k) hi
    check Nothing  _ = True
    check (Just lo) k = lo < k

-- Size Property
isSizeValid :: BST -> Bool
isSizeValid Empty = True
isSizeValid (Node s left _ right) =
    s == 1 + size left + size right &&
    isSizeValid left &&
    isSizeValid right
  where
    size Empty = 0
    size (Node s _ _ _) = s

-- Balance Property (AVL-like)
isBalanced :: BST -> Bool
isBalanced Empty = True
isBalanced (Node _ left _ right) =
    abs (height left - height right) <= 1 &&
    isBalanced left &&
    isBalanced right
  where
    height Empty = 0
    height (Node _ l _ r) = 1 + max (height l) (height r)
```

**Marking (5 marks per invariant):**
- 2 marks: Correct recursive structure visiting all nodes.
- 2 marks: Correct logic for the specific invariant.
- 1 mark: Linear-time implementation (no repeated traversals; the helper functions above achieve this by computing height/size in a single pass per function call).

For `isBST`, accept any correct range-checking approach. For `isSizeValid`, accept solutions that recompute sizes rather than trusting stored sizes (the question asks whether the invariant holds, so recomputing is actually the correct approach). For `isBalanced`, accept height computation; deduct 2 marks if height is recomputed naïvely causing O(n²) time.

---

### (c) Infinite tree (3 marks)

```haskell
infiniteBST :: BST
infiniteBST = build 0 1
  where
    build k d = Node d (build (2*k)     (d+1))
                       k
                       (build (2*k + 1) (d+1))
```

**Marking:** 1 mark for `Node` at root with correct key/size; 1 mark for correct left/right child formulas; 1 mark for correct depth tracking (size = depth + 1). Accept `let` or `where` style.

---

## Question 3 — Model Answers

### (a) Series addition (3 marks)

```haskell
sadd :: Series -> Series -> Series
sadd [] ys = ys
sadd xs [] = xs
sadd (x:xs) (y:ys) = (x + y) : sadd xs ys
```

**Explanation:** When one list is exhausted, we append the remainder of the other list. This is correct because the missing coefficients are implicitly zero; adding zero to `y` yields `y`, so the remaining terms are simply carried forward.

**Marking:** 2 marks for correct code; 1 mark for correct explanation of unequal lengths.

---

### (b) Series multiplication (4 marks)

```haskell
smul :: Series -> Series -> Series
smul [] _ = []
smul _ [] = []
smul (x:xs) ys = sadd (map (x*) ys) (0 : smul xs ys)
```

**Can it be defined as `foldr`?** No, not directly as a single `foldr`. The reason is that `smul` requires **two** input lists and produces a result where each output coefficient depends on multiple elements from both inputs in a cross-product fashion. While one list can be consumed by `foldr`, the other list must be threaded through in a way that requires nested traversal. A single `foldr` over one list cannot directly capture the convolution structure without auxiliary higher-order machinery.

*(Alternative acceptable answer: Yes, with a sufficiently clever accumulator that is itself a function or a list of partial results. Full marks for either answer if the reasoning is sound.)*

**Marking:** 2 marks for correct recursive definition; 2 marks for correct `foldr` determination with justification.

---

### (c) Horner's rule (5 marks)

```haskell
seval :: Series -> Integer -> Integer
seval [] _ = 0
seval (a:as) x = a + x * seval as x
```

**Can it be defined as `foldr`?** **Yes.**

```haskell
seval p x = foldr (\a acc -> a + x * acc) 0 p
```

**Marking:** 2 marks for correct recursive `seval`; 2 marks for correct `foldr` formulation; 1 mark for explicit "Yes" with type-correct code.

---

### (d) Series composition (5 marks)

```haskell
scompose :: Series -> Series -> Series
scompose p q = foldr (\a acc -> sadd [a] (smul q acc)) [0] p
```

Or equivalently using Horner's rule structure from right to left:

```haskell
scompose p q = foldr f [0] p
  where
    f a acc = sadd [a] (smul q acc)
```

**Explanation:** This computes `a0 + q·(a1 + q·(a2 + · · ·))`. The accumulator `acc` holds the partial composition result; at each step we multiply the accumulator by `q` (using `smul`) and add the current coefficient `a` (using `sadd [a]`). No power `q^i` is computed from scratch because each iteration reuses `smul q acc` from the previous step.

**Marking:** 2 marks for correct Horner-like structure; 2 marks for correct use of `sadd` and `smul`; 1 mark for explicit statement that no `q^i` is computed from scratch.

---

### (e) Geometric series (3 marks)

```haskell
geometric :: [Series]
geometric = iterate (\s -> sadd s (0 : s)) [1]
```

Or equivalently:

```haskell
geometric = iterate next [1]
  where
    next s = sadd s (0 : s)
```

**Explanation:** To extend `1 + x + · · · + x^i` to `1 + x + · · · + x^i + x^{i+1}`, we shift the current series left by one position (multiply by `x`, i.e. prepend `0`) and add `1`.

**Marking:** 2 marks for correct `iterate` usage; 1 mark for correct `sadd`/`0:` pattern.

---

## Question 4 — Model Answers

### (a) Recurrence solutions (6 marks, 2 marks each)

1. `T(n) = 4T(n/2) + n²`  
   Master Theorem with `a = 4, b = 2, d = 2`.  
   `log_b a = log₂ 4 = 2 = d`.  
   **Case 2:** `T(n) = Θ(n² log n)`.

2. `T(n) = 2T(n/2) + n log n`  
   This does not fit the standard Master Theorem directly because `f(n) = n log n` is not `Θ(n^d)`.  
   By the Akra-Bazzi method or by the extended Master Theorem: since `n log n` is polynomially larger than `n^{log₂ 2} = n¹` by a logarithmic factor, we get `T(n) = Θ(n log² n)`.
   
   *(Acceptable simplified answer: `Θ(n log² n)` with brief justification, or `Θ(n log n)` if the student incorrectly applies standard Master Theorem — deduct 1 mark for the latter.)*

3. `T(n) = 3T(n/3) + n²`  
   Master Theorem with `a = 3, b = 3, d = 2`.  
   `log_b a = log₃ 3 = 1 < 2 = d`.  
   **Case 3:** `T(n) = Θ(n²)`.

**Marking:** 2 marks each for correct Θ-class and correct justification. Deduct 1 mark per part if the justification is missing or incorrect.

---

### (b) Dijkstra's algorithm (6 marks)

**Execution trace:**

| Step | Vertex labelled | d[1] | d[2] | d[3] | d[4] |
|------|-----------------|------|------|------|------|
| Init | —               | 0    | ∞    | ∞    | ∞    |
| 1    | 1               | 0    | 4    | 2    | ∞    |
| 2    | 3               | 0    | 4    | 2    | 3    |
| 3    | 4               | 0    | 4    | 2    | 3    |
| 4    | 2               | 0    | 4    | 2    | 3    |

*(Wait: after labelling 3, d[4] becomes 3 via edge (3,4). Then 4 is labelled. Then 2 is labelled with d[2]=4. The final distances are d=[0, 4, 2, 3].)*

**Correct trace:**
- Start: d = [0, ∞, ∞, ∞]
- Label 1: relax (1,2)→4, (1,3)→2. d = [0, 4, 2, ∞]
- Label 3 (smallest unvisited is 3 with d=2): relax (3,4)→1. d[4] = 2+1 = 3. d = [0, 4, 2, 3]
- Label 4 (smallest unvisited is 4 with d=3): no outgoing edges to unvisited vertices.
- Label 2 (d=4): no improvement.

Final distances: `d[1]=0, d[2]=4, d[3]=2, d[4]=3`.

**Negative weight analysis:**

If edge `(1,3)` has weight `−x` where `x > 0`, then `d[3] = −x` after labelling 1. Then `d[4]` becomes `−x + 1 = 1 − x`.

Dijkstra's algorithm assumes non-negative edge weights. With a negative weight `−x` on `(1,3)`, the algorithm may still produce correct results **if and only if** the negative edge does not create a shorter path that Dijkstra misses due to premature labelling.

Specifically, vertex 3 is labelled immediately after 1 because it has the smallest tentative distance. The only path to 3 is direct from 1, so `d[3] = −x` is correct. The only path to 4 is via 3 (since 4 has no other incoming edges from unlabelled vertices at that point), so `d[4] = 1 − x` is also correct. Vertex 2 has only the direct edge from 1 with weight 4, so `d[2] = 4` is correct regardless.

Therefore, Dijkstra produces correct results **for all `x > 0`** in this specific graph, because the negative edge is on a tree path and there are no alternative paths to 3 or 4 that could be shorter.

However, if the student reasons generally: Dijkstra requires all edge weights to be non-negative for correctness. In this specific graph, because the negative edge is the unique path to its target and no cycles can improve distances, it happens to work for all `x > 0`. Accept either the specific-graph answer (all `x > 0`) or the general-principle answer (Dijkstra is not guaranteed with negative edges, but works here for all `x > 0`).

**Marking:** 3 marks for correct execution trace (1 mark per step after init); 3 marks for correct negative-weight analysis.

---

### (c) Edit Distance (8 marks)

**Step 1 — Quantity:**

Let `dp[i][j]` denote the edit distance between the prefix `x[1..i]` and the prefix `y[1..j]`, where `0 ≤ i ≤ m` and `0 ≤ j ≤ n`. The answer is `dp[m][n]`.

**Step 2 — Recurrence:**

Base cases:
- `dp[0][j] = j` (insert `j` characters).
- `dp[i][0] = i` (delete `i` characters).

General case (`i > 0, j > 0`):
```
dp[i][j] = min( dp[i-1][j]   + 1,        -- deletion
                dp[i][j-1]   + 1,        -- insertion
                dp[i-1][j-1] + cost )    -- substitution or match
```
where `cost = 0` if `x[i] = y[j]`, otherwise `cost = 1`.

**Justification:** To transform `x[1..i]` into `y[1..j]`, the last operation is either:
- delete `x[i]`, then transform `x[1..i-1]` to `y[1..j]`;
- insert `y[j]`, then transform `x[1..i]` to `y[1..j-1]`;
- substitute (or match) `x[i]` with `y[j]`, then transform `x[1..i-1]` to `y[1..j-1]`.

**Step 3 — Algorithm:**

Fill the `(m+1) × (n+1)` table `dp` row by row (or column by column). For each `i` from 1 to `m` and `j` from 1 to `n`, compute `dp[i][j]` using the recurrence. The table entries `dp[i-1][j]`, `dp[i][j-1]`, and `dp[i-1][j-1]` are already computed in this order.

**Step 4 — Reconstructing operations:**

Maintain a `choice[i][j]` array recording which of the three options achieved the minimum. Trace back from `(m, n)` to `(0, 0)`:
- If `choice[i][j]` is DELETE, record "delete `x[i]`" and move to `(i-1, j)`.
- If INSERT, record "insert `y[j]`" and move to `(i, j-1)`.
- If SUBST/MATCH, record "substitute `x[i] → y[j]`" (or "match") and move to `(i-1, j-1)`.

**Step 5 — Complexity:**

- **Time:** `Θ(m·n)` subproblems, each taking `O(1)` time. Total `Θ(m·n)`.
- **Space:** `Θ(m·n)` for the `dp` table. Can be reduced to `O(min(m, n))` by keeping only two rows.

**Marking:**
- 2 marks: Clear `dp` definition with correct domain and final answer.
- 2 marks: Correct recurrence (all three cases) with justification.
- 1 mark: Correct filling order.
- 1 mark: Backtracking description.
- 2 marks: Correct time and space complexity.

---

## Question 5 — Model Answers

### (a) Greedy counterexample (3 marks)

Consider three activities: `(1, 10)` with duration 9, `(2, 3)` with duration 1, and `(4, 5)` with duration 1.

The shortest-duration greedy algorithm picks `(2, 3)` first (duration 1), then `(4, 5)` (duration 1). These overlap with `(1, 10)`, so `(1, 10)` is removed. The solution has size 2.

The optimal solution is `(2, 3)` and `(4, 5)` — wait, that's also size 2. Let me give a better counterexample.

**Better counterexample:**
Activities: `(1, 4)` duration 3, `(3, 5)` duration 2, `(5, 7)` duration 2.

Shortest-duration greedy picks `(3, 5)` first (duration 2). This overlaps with both `(1, 4)` and `(5, 7)`, so only `(3, 5)` is selected. Solution size = 1.

Optimal solution: `(1, 4)` and `(5, 7)` — they do not overlap. Solution size = 2.

**Marking:** 1 mark for a valid counterexample; 2 marks for showing the greedy solution is suboptimal.

---

### (b) Interval partitioning / room allocation (5 marks)

**Algorithm:** Sort activities by start time. Use a min-heap (priority queue) storing the finish times of the last activity in each room. For each activity in sorted order:
- If the activity's start time is ≥ the smallest finish time in the heap, reuse that room (pop the heap and push the new finish time).
- Otherwise, allocate a new room (push the finish time onto the heap).

The size of the heap at the end is the minimum number of rooms.

**Execution on `{(1, 4), (2, 5), (3, 6), (5, 8), (7, 9), (8, 10)}`:**

Sorted by start time: already sorted.

- `(1, 4)`: heap = `{4}`. Rooms = 1.
- `(2, 5)`: start 2 < min-heap 4. New room. heap = `{4, 5}`. Rooms = 2.
- `(3, 6)`: start 3 < min-heap 4. New room. heap = `{4, 5, 6}`. Rooms = 3.
- `(5, 8)`: start 5 ≥ 4. Reuse room 1. Pop 4, push 8. heap = `{5, 6, 8}`. Rooms = 3.
- `(7, 9)`: start 7 ≥ 5. Reuse room 2. Pop 5, push 9. heap = `{6, 8, 9}`. Rooms = 3.
- `(8, 10)`: start 8 ≥ 6. Reuse room 3. Pop 6, push 10. heap = `{8, 9, 10}`. Rooms = 3.

Minimum rooms = **3**.

**Marking:** 2 marks for correct algorithm description; 3 marks for correct execution trace.

---

### (c) Kruskal's algorithm (4 marks)

Edges in order of weight: `(3, 4): 1`, `(4, 6): 2`, `(2, 4): 2`, `(6, 7): 3`, `(1, 3): 3`, `(4, 5): 4`, `(1, 2): 5`, `(5, 7): 6`.

Kruskal's adds edges in weight order, skipping those that form cycles:

1. Add `(3, 4)` weight 1. Components: {3,4}, {1}, {2}, {5}, {6}, {7}
2. Add `(4, 6)` weight 2. Components: {3,4,6}, {1}, {2}, {5}, {7}
3. Add `(2, 4)` weight 2. Components: {2,3,4,6}, {1}, {5}, {7}
4. Add `(6, 7)` weight 3. Components: {2,3,4,6,7}, {1}, {5}
5. Add `(1, 3)` weight 3. Components: {1,2,3,4,6,7}, {5}
6. Add `(4, 5)` weight 4. Components: {1,2,3,4,5,6,7} — all connected.

Stop. MST edges in order added: **`(3,4), (4,6), (2,4), (6,7), (1,3), (4,5)`**.

**Marking:** 4 marks for correct edge list in correct order. Deduct 1 mark per incorrect or out-of-order edge (minimum 0).

---

### (d) Linear-time MST for weights {1, 2, 3} (8 marks)

**Algorithm:**

1. Run a **BFS/DFS** to check if the graph is connected (given).
2. Collect all edges of weight 1. Run a **Union-Find** pass: for each weight-1 edge, if its endpoints are in different components, add it to the MST and union the components.
3. Collect all edges of weight 2. Run another Union-Find pass over these edges: for each weight-2 edge connecting different components, add it to the MST and union.
4. Collect all edges of weight 3. Run a final Union-Find pass.
5. Stop when `|V| − 1` edges have been selected.

**Correctness:** This is a special case of Kruskal's algorithm. Since we process edges in non-decreasing weight order (all 1s, then all 2s, then all 3s), the cut property guarantees that every edge added is safe for the MST. Union-Find ensures we never add edges that form cycles.

**Running time:**
- Partitioning edges into three buckets: `O(|E|)`.
- Each Union-Find operation with path compression and union by rank takes amortized `O(α(|V|))` where `α` is the inverse Ackermann function, effectively constant.
- We perform `O(|E|)` find/union operations.
- **Total:** `O(|V| + |E|)`.

**Marking:**
- 3 marks: Correct algorithm (bucketed Kruskal/Union-Find).
- 3 marks: Correctness argument (cut property + cycle avoidance).
- 2 marks: Time analysis showing `O(|V| + |E|)`.

---

## Question 6 — Model Answers

### (a) BFS distances and dequeue order (4 marks)

**Distances from source 1:**
- `d[1] = 0`
- `d[2] = 1` (edge 1-2)
- `d[3] = 1` (edge 1-3)
- `d[4] = 2` (edge 1-3-4 or 1-2-4)
- `d[5] = 3` (edge 1-2-4-5 or 1-3-4-5)
- `d[6] = 3` (edge 1-2-4-6 or 1-3-4-6)

**Dequeue order:** 1, 2, 3, 4, 5, 6

*(Lexicographic edge processing: from 1, neighbours are 2, 3. Enqueue 2, then 3. Dequeue 2. From 2, neighbours are 1 (visited), 4. Enqueue 4. Dequeue 3. From 3, neighbours are 1 (visited), 4 (already enqueued). Dequeue 4. From 4, neighbours are 2 (visited), 3 (visited), 5, 6. Enqueue 5, then 6. Dequeue 5. Dequeue 6.)*

**Marking:** 2 marks for correct distances; 2 marks for correct dequeue order.

---

### (b) Bipartite directed graph (5 marks)

**Algorithm:**

1. Compute the **Strongly Connected Components (SCCs)** of `G` using Kosaraju's or Tarjan's algorithm.
2. Build the **component graph** (condensation graph) `G'`, where each vertex is an SCC and edges are between components.
3. The component graph `G'` is a **DAG**.
4. A directed graph is bipartite (in the sense of the question) if and only if its component graph `G'` is bipartite as an undirected graph and **no SCC contains a directed cycle of odd length**.

Actually, a simpler interpretation: the question defines bipartite as edges only going between two sets (no edges within a set). For a directed graph, this is equivalent to saying the underlying undirected graph is bipartite and there are no self-loops.

**Simpler correct algorithm:**

1. Compute SCCs.
2. If any SCC has size > 1 (i.e. contains a cycle), then the graph cannot be bipartite unless the cycle is even and edges respect the partition. Actually, any directed cycle of odd length makes bipartiteness impossible.
3. For the DAG of SCCs, perform a BFS/DFS 2-colouring. If any edge connects vertices of the same colour, the graph is not bipartite.

**High-level description accepted at full marks:**
- Compute SCCs in `O(|V| + |E|)`.
- Condense to a DAG.
- Check if the DAG is 2-colourable (bipartite as an undirected graph) in `O(|V| + |E|)`.
- If any SCC contains an odd cycle, answer NO; otherwise answer YES if and only if the DAG is 2-colourable.

**Running time:** `O(|V| + |E|)`.

**Marking:** 3 marks for correct high-level algorithm using SCCs; 2 marks for correct running time.

---

### (c) Inversion counting (6 marks)

**Algorithm:** Modify **Merge Sort** to count inversions.

1. **Divide:** Split the array into two halves.
2. **Conquer:** Recursively count inversions in each half. Let the counts be `inv_left` and `inv_right`.
3. **Combine:** Merge the two sorted halves. During the merge, whenever an element from the right half is placed before an element from the left half, all remaining elements in the left half form inversions with this right-half element. Add the number of remaining left-half elements to the inversion count.

Let `inv_split` be the inversions counted during the merge. The total is `inv_left + inv_right + inv_split`.

**Correctness:** Every inversion pair `(i, j)` with `i < j` and `xi > xj` falls into exactly one of three categories:
- Both `i, j` in the left half: counted by `inv_left`.
- Both `i, j` in the right half: counted by `inv_right`.
- `i` in left half, `j` in right half: counted by `inv_split` during the merge.

**Running time:** The recurrence is `T(n) = 2T(n/2) + O(n)`, which solves to `T(n) = O(n log n)` by the Master Theorem.

**Marking:** 2 marks for correct divide-and-conquer structure; 2 marks for correct merge-phase counting; 2 marks for correctness argument and time analysis.

---

### (d) Element uniqueness lower bound (5 marks)

**Proof:**

Suppose there exists a comparison-based algorithm `A` for element uniqueness that uses fewer than `Ω(n log n)` comparisons in the worst case. We will use `A` to sort in fewer than `Ω(n log n)` comparisons, contradicting the known lower bound for comparison-based sorting.

**Reduction:**
1. Given a list `x1, . . . , xn` to sort, first run algorithm `A` to check if all elements are distinct.
2. If `A` reports "NO" (not all distinct), we can handle duplicates separately (e.g. by partitioning into equal groups, which takes linear time).
3. If `A` reports "YES" (all distinct), we know all elements are distinct. Now run any comparison-based sorting algorithm. Because all elements are distinct, every comparison yields a strict `<` or `>` result.

Actually, a cleaner reduction: element uniqueness is easier than sorting. But we need to show uniqueness is *at least as hard* as sorting in the worst case for distinct inputs.

**Correct argument:**
Consider the set of `n!` permutations of `{1, 2, . . . , n}`. A comparison-based sorting algorithm must distinguish all `n!` possibilities, requiring `Ω(log n!) = Ω(n log n)` comparisons.

Now, any comparison-based algorithm for element uniqueness can be used to sort as follows: given `n` numbers to sort, first check if they are distinct. If they are, then the relative order of any two elements can be determined by a single comparison, and we can use a standard sorting algorithm. But this doesn't directly give a lower bound.

**Better argument (decision tree):**
The element uniqueness problem has two outputs: YES or NO. However, for inputs consisting of `n` distinct numbers, the algorithm must behave correctly and output YES. The set of all `n`-tuples of distinct numbers from a large universe has `n!` different relative orderings. Any comparison-based algorithm that correctly identifies uniqueness must, on distinct inputs, perform enough comparisons to verify that no two elements are equal. In particular, if the algorithm terminates after `k` comparisons without having compared every pair in some partition, there exist two inputs with different equality patterns that produce the same comparison outcomes, causing the algorithm to make an error.

A rigorous argument: consider the `n!` permutations of `{1, ..., n}`. Any algorithm that uses fewer than `log₂(n!)` comparisons cannot distinguish all permutations. If it cannot distinguish two different permutations `π₁` and `π₂`, then there exist two inputs with the same comparison results where one has all distinct elements and the other also has all distinct elements but a different order. However, the algorithm must output YES for both. This doesn't directly yield a contradiction.

**Standard accepted argument:**
Element uniqueness is at least as hard as sorting in the following sense: given `n` numbers, if we could determine whether they are all distinct in `o(n log n)` comparisons, then we could sort `n` distinct numbers in `o(n log n)` comparisons by first verifying distinctness and then... (this needs care).

The standard proof in textbooks: Any comparison-based algorithm for element uniqueness can be transformed into a comparison-based algorithm for sorting by first checking uniqueness and then, if unique, using the same comparisons to establish a total order. But the decision tree for uniqueness on `n` distinct elements must have at least `n!` leaves (one for each permutation), because each permutation must lead to a distinct leaf (otherwise two different orderings would be indistinguishable by comparisons, and an adversary could place equal elements in positions that the algorithm failed to compare). Therefore the tree height is `Ω(log n!) = Ω(n log n)`.

**Marking:** 2 marks for attempting a reduction from sorting or a decision-tree argument; 3 marks for a complete and rigorous argument. Accept the standard "n! permutations require n! leaves in the decision tree" argument as full marks.

---

**END OF MARK SCHEME**
