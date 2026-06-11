# FP #7 — Types: Most General Types, Type Inference, type/data/newtype, Classes

**Frequency: 9/16 real papers. New-format slot: Q2 opens with a 2-mark "explain this keyword" (2025: `deriving`; 2024: data/newtype/type; mock: `type`).**
Appearances: 2010 Q1(c)(d) (FP 2015 paper variant), 2012 Q1(a), 2013 Q1(b), 2015 Q1(c)(d), 2017 Q1(d), 2019 Q1(f), 2021 Q4, 2024 Q4(a), 2025 Q2(a); also 2026 mock Q2(a).

---

## How it is asked

1. **"Give the most general type of f, explain your reasoning"** where f composes its arguments in a tricky way (4–5 marks).
2. **"Explain why f has no type"** — occurs-check failures (2015 Q1c).
3. **type vs data vs newtype** (2024 Q4a, 2026 Q2a, 2011 Q3a).
4. **Type classes:** what `Eq a =>` tells you; write instance declarations (2021 Q4).

---

## The inference method (write it out — the marks are for reasoning)

For `f g x = g (g x) x`-style questions:
1. Assign type variables: `x :: a`, `g :: t`.
2. Generate constraints from each application, innermost first: `g x` forces `t = a -> b`; `g (g x) x` forces `g :: b -> a -> c` … unify step by step **on paper**.
3. Conclude `f :: (final type of g) -> (type of x) -> result`, with the most general variables remaining.

**Worked examples to rehearse:**
- `f g x = g (g x) x` — **no type**: `g :: a -> b` from `g x`, but `g (g x) x` needs `g :: b -> a -> c`, so `a = b -> (a -> c)`… unification gives `a` occurring in its own definition (**occurs check fails** — infinite type). Name the occurs check.
- `f g x = g . g x` (2015 Q1d): `g x :: c -> d`-shaped; composing `g . (g x)` forces `g :: (c -> d)`-compatible domains; answer `f :: ((c -> d) -> c -> d)`-family — derive: `g :: a -> (c -> d)` and `g` also applied to result of `g x`'s output `d`, so `a = d`; result `f :: (d -> c -> d) -> d -> c -> d`.
- `f x y z = x (x z) y` (2013): `x :: t -> (y's type) -> …` derive `f :: (a -> b -> a)-ish` — practice it.
- `f g h x y = g . h x . g y` (2017): chase each composition's domains/codomains.
- `k s x f y = f (s y x)` (2019): `k :: (a -> b -> c) -> b -> (c -> d) -> a -> d`.
- `map concat :: [[[a]]] -> [[a]]` (2013): specialise `map :: (x -> y) -> [x] -> [y]` with `x = [[a]], y = [a]`.

**Rule:** the most general type keeps every unconstrained variable distinct; add class contexts only where operations force them (`==` → `Eq`, `<` → `Ord`, arithmetic → `Num`).

---

## type vs data vs newtype (memorise the 3-way contrast, 2024 Q4a)

- `type Nat = Int` — **synonym**: same type, interchangeable everywhere, no constructor, no extra safety, purely documentation.
- `data Nat = N Int` — **new type with constructor N**: distinct from Int (type-checked separately); values are wrapped; adds laziness layer (`N ⊥ ≠ ⊥`); runtime cost of the wrapper.
- `newtype Nat = N Int` — distinct type like data, but **exactly one constructor with one field**; no runtime cost; `N ⊥ = ⊥` (the constructor is transparent at runtime) — semantic difference from data shows up with ⊥/pattern matching.
- `type` declarations cannot be recursive; `data`/`newtype` can.
- **`deriving` (2025 Q2a):** asks the compiler to generate standard instances automatically — `deriving Eq` produces the structural-equality `instance Eq Colour` (constructors equal iff same constructor and equal fields), avoiding a hand-written instance; similarly Ord/Show/etc.

## Type classes (2021 Q4, 2018 Q2)

- `the :: Eq a => [a] -> a`: the context tells you the function may only *compare* elements — it cannot invent a value of type a, so on `[]` it must be ⊥/error; with ≥2 distinct values it can detect inequality (and may error) — reasoning-from-the-type questions: what CAN a function of this type do?
- `(==) :: Eq a => a -> a -> Bool` means (==) is **overloaded**: defined per-instance via `instance Eq Bool where …`; write instances:
```haskell
instance Eq Bool where
  True  == True  = True
  False == False = True
  _     == _     = False
instance Eq a => Eq (Maybe a) where
  Nothing == Nothing = True
  Just x  == Just y  = x == y
  _       == _       = False
```
- Parametricity flavour ("what does the type tell you"): a polymorphic function cannot inspect values of variable type — answers must come from the structure (e.g. `f :: [a] -> [a]` can only rearrange/drop/duplicate elements).

---

## Traps

- "Most general" — do not over-specialise; `f1 = foldr (++) []` is `[[a]] -> [a]` (concat), not `[[Int]] -> [Int]`.
- When two usages of the same variable force different types → unify; if unification loops → say "occurs check / infinite type, hence no (finite) type".
- Strictness difference between data and newtype is the 4th mark of the 2024 question — mention ⊥.
- In instance declarations, the context `Eq a =>` on the Maybe instance is required — forgetting it is the classic error.
