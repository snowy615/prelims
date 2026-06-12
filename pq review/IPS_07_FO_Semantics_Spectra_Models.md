# IPS-07 — First-Order Semantics, Spectra & Models

**Frequency:** IFP 2022, 2023 and 2025 Q5(d). 4–7 marks per paper, sometimes in the compulsory question. Must prepare.

---

## 1. FO over structures — the vocabulary

A **signature** Σ = (function symbols, predicate symbols, constants, with arities).
A **structure** A = (U, interpretations) over Σ has a domain U and interpretations of each symbol.

**The key structures to know:**
- (0,1) with < : open unit interval, dense, no endpoints, no least/greatest.
- [0,1] with < : closed unit interval, dense, has least 0 and greatest 1.
- Z with < : discrete, no endpoints, has immediate successors.
- [0,1] ∪ [2,3] with < : two dense components with a gap.
- Graphs: signature ⟨E⟩ with E binary (undirected: symmetric, irreflexive).

## 2. Distinguishing structures with FO sentences (2–5 marks)

**Template:** for each desired true/false pattern, pick a structural property that one structure has and the other lacks.

- **Least element:** `∃x·∀y·(x = y ∨ x < y)` — true in [0,1], false in (0,1).
- **Greatest element:** `∃x·∀y·(x = y ∨ y < x)` — same.
- **Density:** `∀x·∀y·(x < y → ∃z·(x < z ∧ z < y))` — true in intervals, false in Z.
- **Discreteness / successor:** `∀x·∃y·(x < y ∧ ∀z·(¬(x < z) ∨ z = y ∨ y < z))` — true in Z, false in intervals.
- **Gap:** `∃x·∀y·(y ≤ x ∨ x+1 ≤ y)` — not FO with just <, but can express two dense pieces separated: `∃x·∃y·(x < y ∧ ∀z·(z ≤ x ∨ y ≤ z))` — true in [0,1]∪[2,3], false in connected intervals.

**Graph properties:**
- Independent set of size k: `∃x₁…∃xₖ·(⋀_{i≠j} xᵢ ≠ xⱼ ∧ ⋀_{i,j} ¬E(xᵢ, xⱼ))`
- Vertex cover of size k: `∃x₁…∃xₖ·(⋀_{i≠j} xᵢ ≠ xⱼ ∧ ∀y·∀z·(E(y,z) → ⋁_{i} (y = xᵢ ∨ z = xᵢ)))`

## 3. Finite vs infinite models (2023 Q8(c), 2 marks)

**Template argument:** an irreflexive transitive relation with ∀x·∃y·Q(x,y) has no finite model.
- Proof: start at any x₁. By ∀x·∃y·Q(x,y), get x₂ with Q(x₁,x₂). By transitivity and irreflexivity, x₁ ≠ x₂. Repeat: get x₃ with Q(x₂,x₃), transitivity gives Q(x₁,x₃), irreflexivity gives x₃ ≠ x₁, x₂. In a finite domain, pigeonhole forces a repeat, contradiction with irreflexivity.
- Infinite model: (N, <) works.

## 4. Spectra (2025 Q5(d))

The **spectrum** S(F) = {|U| : A ⊨ F, |U| finite}.

- **Finite or co-finite sets are spectra:** if M is finite, say M = {n₁,…,nₖ}, let F be the disjunction over nᵢ of a sentence saying "the domain has exactly nᵢ elements" (e.g. ∃x₁…∃xₙ·(all distinct ∧ ∀y·⋁ y = xⱼ)). If co-finite, say N \ M is finite, then take the negation of the finite-disjunction sentence for the complement.
- **Even numbers {2n : n ∈ N}:** use a sentence stating "there exists a binary relation that is a bijection between the domain and a proper subset of the domain disjoint from itself" — i.e. a pairing relation. More concretely: ∃R·(R is a bijection without fixed points and R∘R = id). The domain can be partitioned into pairs.
- **Factorials {n! : n ∈ N}:** the number of linear orderings over n elements is n!. A sentence saying "the domain can be linearly ordered, and there is a bijection between the domain and the set of all linear orderings of the domain" has models exactly of size n!.

## 5. Substitution, free-for, NNF (2022–2023)

**Free for substitution:** term T is free for variable x in φ if no free occurrence of x in φ lies in the scope of a quantifier that binds a variable of T.
- If not free-for: **rename bound variables** (e.g. change ∀y·… to ∀z·…), then substitute.

**Negation Normal Form (NNF):** ¬ appears only in front of atoms. Push ¬ inward with:
- ¬(φ ∧ ψ) → ¬φ ∨ ¬ψ
- ¬(φ ∨ ψ) → ¬φ ∧ ¬ψ
- ¬∀x·φ → ∃x·¬φ
- ¬∃x·φ → ∀x·¬φ
- ¬(φ → ψ) → φ ∧ ¬ψ

## 6. Mark-scheme checklist

- [ ] FO sentence syntax matches the signature exactly (right symbols, no constants if forbidden).
- [ ] Distinguishing sentences: briefly explain *why* the property holds in one structure and fails in the other.
- [ ] Finite-model impossibility: infinite chain argument, pigeonhole explicitly invoked.
- [ ] Spectra: sentence explicitly relates domain size to the combinatorial property (bijection, linear orderings).
- [ ] Substitution: check each free occurrence of x, identify scope violations, rename if needed.
- [ ] NNF: push ¬ through systematically; final formula has ¬ only on atoms.
