# Induction Proof Template (Exam-Ready)

> **Scope**: List induction, tree induction, and structural induction over recursive datatypes in FP & DAA. Missing any of the three mandatory components below will cost marks.

---

## The Three Mandatory Components

Every induction proof must contain these three explicitly labelled sections. If any one is missing or implicit, examiners will deduct marks for rigour.

| Component | Label in Answer | What Must Appear |
|-----------|-----------------|------------------|
| **1. Base Case** | "Base case" | Explicit calculation showing LHS = RHS for the smallest structure. Write out the expressions fully before simplifying. |
| **2. Inductive Hypothesis (IH)** | "Inductive hypothesis" | A **declarative sentence** stating exactly what is assumed. Must include the quantifier ("for all f, e, xs, ..."). |
| **3. Inductive Step** | "Inductive step" | A chain of equalities LHS = ... = RHS where **every step is annotated** with the justification (definition, IH, lemma, or algebraic law). |

---

## Detailed Template

### Step 1 — State the Proposition

Before beginning, write the exact property to be proved, with universal quantifiers.

> **Proposition**: For all functions `f :: a -> b -> b`, all values `e :: b`, and all lists `xs :: [a]`,
> ```
> map (fold f e) (tails xs) = scanr f e xs
> ```

---

### Step 2 — Base Case

**Rule**: Write the LHS and RHS **in full** for the base structure, then simplify each side independently until they are syntactically identical.

> **Base case** (`xs = []`):
> ```
> LHS = map (fold f e) (tails [])
>     = map (fold f e) [[]]                 -- by definition of tails
>     = [fold f e []]                       -- by definition of map
>     = [e]                                 -- by definition of fold
>
> RHS = scanr f e []
>     = [e]                                 -- by definition of scanr
> ```
> Therefore LHS = RHS.

**Checklist**
- [ ] Did you write `LHS = ...` and `RHS = ...` separately?
- [ ] Is every simplification step annotated with "by definition of ..."?
- [ ] Did you conclude with "Therefore LHS = RHS"?

---

### Step 3 — Inductive Hypothesis (IH)

**Rule**: This must be a **single declarative sentence** that assumes the property for the smaller structure. It must mirror the Proposition exactly, with the same quantifiers.

> **Inductive hypothesis**: Assume that for all `f`, `e`, and some list `xs`,
> ```
> map (fold f e) (tails xs) = scanr f e xs
> ```
> holds.

**Common Mistakes**
- ❌ "Assume it holds for n." (Too vague; state the exact equation.)
- ❌ "Assume `map (fold f e) (tails xs) = scanr f e xs`." (Missing quantifiers.)
- ✅ "Assume that for all `f`, `e`, and some list `xs`, `map (fold f e) (tails xs) = scanr f e xs` holds."

---

### Step 4 — Inductive Step (The Equality Chain)

**Rule**: Start from LHS, transform step-by-step to RHS. **Every equality sign must have a justification on the right.** Use the following annotation vocabulary:

| Annotation | Meaning |
|------------|---------|
| `(def. of ...)` | unfolding or folding a function definition |
| `(IH)` | application of the Inductive Hypothesis |
| `(Lemma N)` | application of a previously proved auxiliary lemma |
| `(assoc.)` | associativity of some operator |
| `(case ...)` | case analysis (e.g. `p x = True` vs `False`) |

> **Inductive step** (`xs = x : xs'`):
> We show `map (fold f e) (tails (x:xs')) = scanr f e (x:xs')`.
>
> ```
> LHS = map (fold f e) (tails (x:xs'))
>     = map (fold f e) ((x:xs') : tails xs')       (def. of tails)
>     = fold f e (x:xs') : map (fold f e) (tails xs')  (def. of map)
>     = f x (fold f e xs') : map (fold f e) (tails xs') (def. of fold)
>     = f x (fold f e xs') : scanr f e xs'              (IH)
>
> RHS = scanr f e (x:xs')
>     = f x (head ys) : ys  where ys = scanr f e xs'  (def. of scanr)
> ```
>
> To complete the proof we need:
> ```
> head (scanr f e xs') = fold f e xs'
> ```
> which is Lemma 1 (proved below). Substituting:
> ```
> RHS = f x (fold f e xs') : scanr f e xs'
> ```
> Hence LHS = RHS.

**Checklist**
- [ ] Is there a justification for **every** equality step?
- [ ] Are justifications written on the **same line** or immediately to the right?
- [ ] Did you explicitly state where the IH is used?
- [ ] If an auxiliary fact is needed, did you flag it as a lemma and prove it separately?

---

## Auxiliary Lemmas: Required Protocol

If the inductive step needs a fact that is not part of the main proposition, you **must**:

1. **State the lemma explicitly** before or during the main proof.
2. **Give it a name/number** (e.g. "Lemma 1", "Claim A").
3. **Prove it separately** using the same three-component structure.

> **Lemma 1**: For all `f`, `e`, `xs`,
> ```
> head (scanr f e xs) = fold f e xs
> ```
>
> **Proof of Lemma 1** (by induction on `xs`):
>
> *Base case* (`xs = []`):
> ```
> LHS = head (scanr f e [])
>     = head [e]                (def. of scanr)
>     = e
> RHS = fold f e []
>     = e                         (def. of fold)
> ```
>
> *Inductive hypothesis*: Assume `head (scanr f e xs) = fold f e xs`.
>
> *Inductive step* (`xs = x : xs'`):
> ```
> LHS = head (scanr f e (x:xs'))
>     = head (f x (head ys) : ys)   where ys = scanr f e xs'  (def. of scanr)
>     = f x (head ys)                                        (def. of head)
>     = f x (fold f e xs')                                   (IH on ys)
> RHS = fold f e (x:xs')
>     = f x (fold f e xs')                                   (def. of fold)
> ```
> ∎

**Critical Rule**: Never say "it is obvious that ..." or "we can see that ...". Either prove it as a lemma, or cite a standard law (e.g. `(++)` is associative) with its name.

---

## Common Induction Schemes

| Structure | Induction Variable | Base Case | Typical IH Form |
|-----------|-------------------|-----------|-----------------|
| **Lists** | `xs :: [a]` | `xs = []` | Assume for `xs`, prove for `x:xs` |
| **Natural numbers** | `n :: Nat` | `n = 0` | Assume for `n`, prove for `n+1` |
| **Trees** | `t :: Tree a` | `t = Empty` | Assume for `left` and `right`, prove for `Fork r left v right` |
| **Multiple lists** | `(xs, ys)` | `xs = []` or `ys = []` | Often lexicographic or simultaneous induction |

---

## Marking Rubric (What Examiners Look For)

| Criterion | Marks | Penalty if Missing |
|-----------|-------|-------------------|
| Clear proposition statement | 1 | -1 for vague "prove the equation" |
| Explicit base case with calculation | 2 | -2 if only stated without calculation |
| Explicit inductive hypothesis sentence | 2 | -2 if IH is only used implicitly |
| Equality chain with step annotations | 4 | -1 per unannotated step; -2 if no chain |
| Auxiliary lemmas stated & proved | 1 | -1 if used without proof or statement |
| Conclusion sentence | 0–1 | Minor, but signals completeness |

---

## Full Worked Example: `map (fold f e) . tails = scanr f e`

This is the exact question from FP Q1e (10 marks). The answer below would receive full credit.

> **Proposition**: For all `f :: a -> b -> b`, `e :: b`, and `xs :: [a]`,
> ```
> map (fold f e) (tails xs) = scanr f e xs
> ```
>
> We first establish an auxiliary result.
>
> ---
> **Lemma A**: `head (scanr f e xs) = fold f e xs` for all `f, e, xs`.
>
> *Proof of Lemma A* (by structural induction on `xs`):
>
> **Base case** (`xs = []`):
> ```
> head (scanr f e [])
> = head [e]              (def. of scanr)
> = e
> = fold f e []           (def. of fold)
> ```
>
> **Inductive hypothesis**: Assume `head (scanr f e xs) = fold f e xs`.
>
> **Inductive step** (`xs = x : xs'`):
> Let `ys = scanr f e xs'`.
> ```
> head (scanr f e (x:xs'))
> = head (f x (head ys) : ys)     (def. of scanr)
> = f x (head ys)                  (def. of head)
> = f x (fold f e xs')             (IH)
> = fold f e (x:xs')               (def. of fold)
> ```
> ∎
>
> ---
> **Main proof** (by structural induction on `xs`):
>
> **Base case** (`xs = []`):
> ```
> LHS = map (fold f e) (tails [])
>     = map (fold f e) [[]]                 (def. of tails)
>     = [fold f e []]                       (def. of map)
>     = [e]                                 (def. of fold)
> RHS = scanr f e []
>     = [e]                                 (def. of scanr)
> ```
> Hence LHS = RHS.
>
> **Inductive hypothesis**: Assume that for all `f, e` and some list `xs`,
> ```
> map (fold f e) (tails xs) = scanr f e xs
> ```
>
> **Inductive step** (`xs = x : xs'`):
> Let `ys = scanr f e xs'`.
> ```
> LHS = map (fold f e) (tails (x:xs'))
>     = map (fold f e) ((x:xs') : tails xs')       (def. of tails)
>     = fold f e (x:xs') : map (fold f e) (tails xs')  (def. of map)
>     = f x (fold f e xs') : map (fold f e) (tails xs') (def. of fold)
>     = f x (fold f e xs') : scanr f e xs'              (IH)
>
> RHS = scanr f e (x:xs')
>     = f x (head ys) : ys                               (def. of scanr)
>     = f x (fold f e xs') : scanr f e xs'               (Lemma A, since ys = scanr f e xs')
> ```
> Therefore LHS = RHS.
>
> ∎

---

## Quick Checklist Before Submitting

- [ ] Did I write the **Proposition** with all quantifiers?
- [ ] Did I label **"Base case"**, **"Inductive hypothesis"**, and **"Inductive step"** explicitly?
- [ ] In the base case, did I compute **both sides independently** to a common expression?
- [ ] Is the IH a **complete sentence** with quantifiers?
- [ ] Does **every equality** in the inductive step have a justification?
- [ ] Did I **name and prove** any auxiliary lemmas separately?
- [ ] Did I end with **∎** or "QED" or a clear conclusion sentence?

---

*Template compiled for Oxford CS1 FP examination style. Applicable to any structural induction over lists, trees, or naturals.*
