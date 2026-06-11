# DAA #4 — Minimum Spanning Trees: Kruskal, Prim, Cut Lemma

**Frequency: 10/16 real papers, every year 2022–2025 (trace + variant design in Q5). The Cut Lemma (state + prove + apply) is the most repeated proof in the whole DAA course.**
Appearances: 2010 Q5, 2012 Q6, 2014 Q8, 2016 Q8(a), 2017 Q7, 2018 Q8, 2022 Q8, 2023 Q7, 2024 Q5, 2025 Q5(c)(d); also 2026 mock Q5(c)(d).

---

## How it is asked

1. **Definitions (2–5 marks):** spanning tree; MST; prove all spanning trees have |V|−1 edges.
2. **Trace (4–8 marks):** run Kruskal and/or Prim on a given graph, list edges in the order added, lexicographic tie-breaking.
3. **Cut Lemma (3–8 marks):** state precisely, prove it, use it to prove Kruskal/Prim (or a novel procedure, e.g. Borůvka-style `MyProcedure` 2017) correct.
4. **Variants (4–8 marks):** maximum spanning tree (negate weights); widest/maximin path (2014); MST uniqueness for distinct weights (2010, 2024 "stability"); minimum u-tree / leaves-constrained MST (2022); O(|V|+|E|) MST when weights ∈ {1,2} (2026).

---

## Memorise verbatim

**Spanning tree** of connected undirected G=(V,E): a subgraph that is a tree (connected, acyclic) containing every vertex. Every spanning tree has exactly **|V|−1 edges** (proof: a tree on k vertices has k−1 edges, by induction — adding an edge without creating a cycle joins two components, reducing the component count by 1 from |V| down to 1).

**MST:** a spanning tree minimising total edge weight.

**Cut:** a partition (S, V∖S) of the vertices. An edge **crosses** the cut if its endpoints lie on opposite sides. A set of edges A **respects** the cut if no edge of A crosses it. A **light edge** is a crossing edge of minimum weight.

**Cut Lemma:** Let A be a subset of some MST T, let (S, V∖S) be a cut that A respects, and let e be a light edge crossing the cut. Then A ∪ {e} is also a subset of some MST.
**Proof (exchange argument — write all 4 steps):**
1. If e ∈ T, done. Otherwise T ∪ {e} contains a unique cycle C.
2. C crosses the cut at e and at least once more, at some edge e′ ∈ T, e′ ≠ e; e′ crosses the cut so e′ ∉ A.
3. T′ = T − e′ + e is a spanning tree (removing an edge of the cycle keeps connectivity), and w(T′) = w(T) − w(e′) + w(e) ≤ w(T) since e is light. So T′ is an MST.
4. A ∪ {e} ⊆ T′. ∎

**Kruskal:** sort edges by increasing weight; scan, adding an edge iff it joins two different components (union–find). O(|E| log |E|).
Correctness via Cut Lemma: when (u,v) is added, take the cut (component of u, rest); the current forest respects it, and (u,v) is light for it (all lighter edges were rejected for forming cycles, i.e. don't cross).

**Prim:** grow a tree from a root, always adding the lightest edge leaving the tree (priority queue keyed by lightest connecting edge). O(|E| log |V|) with a binary heap. Correctness: the chosen edge is light for the cut (tree, rest).

**Greedy:** both are greedy — they make a locally optimal (lightest safe edge) choice that is never revoked; the Cut Lemma proves the choice is safe.

---

## Variant answers (rehearsed)

- **Maximum spanning tree:** run Kruskal on weights w′ = −w (or sort decreasing). One sentence of justification: maximising Σw = minimising Σ(−w).
- **Order-invariance / uniqueness (2010 Q5d, 2024 Q5c):** Kruskal's decisions depend only on the *order* of weights (the sorted sequence and the union–find tests), not the values. With distinct weights the MST is unique: suppose T ≠ T′, take the lightest edge in the symmetric difference, exchange — contradiction.
- **Widest path / maximin (2014 Q8d):** the path in the *maximum* spanning tree between the two vertices maximises the minimum edge; prove by Cut-Lemma-style exchange: any path leaving the max-tree crosses a cut where the tree edge is at least as wide.
- **Weights in {1, 2} (2026 mock):** two-pass approach: first take a spanning forest of the weight-1 edges (DFS/BFS), then connect components with weight-2 edges — O(|V|+|E|); correctness via Cut Lemma / exchange.
- **Is edge e=(u,v) in SOME MST? (2025 Q5d, distinct weights ⇒ "the" MST):** e ∈ MST ⟺ there is **no path from u to v using only edges strictly lighter than w(e)**. Algorithm: DFS/BFS on the subgraph of edges with weight < w(e); answer NO iff u,v are connected there. O(|V|+|E|). Correctness both ways: if no lighter path, take the cut (component of u in the lighter-subgraph, rest) — e is the light crossing edge, so safe (Cut Lemma); if a lighter path exists, e is the heaviest edge on the cycle (path + e), and the heaviest cycle edge is in no MST (exchange: remove e from any tree containing it, reconnect via the cycle).
- **MST with required leaves L (2022):** compute MST of G ∖ L, then attach each vertex of L by its cheapest edge to the rest.
- **True/false statements about MSTs (2018 Q8b):** "T∪{e} has a unique cycle" — TRUE (tree + extra edge); "every tree edge is light for some cut" — TRUE for distinct weights (cut = the two components of T − e); be careful with ties.

---

## Traps

- In traces, list edges **in the order added** and apply the stated tie-breaking rule; for Prim, start at the specified node.
- Kruskal works fine with negative weights (2024 Q5a-ii: YES — correctness never uses positivity).
- The Cut Lemma needs "A respects the cut" — do not omit it, and use it where it is used in the proof (step 2: e′ ∉ A).
- "Show all spanning trees have the same number of edges" is a 5-mark proof, not a statement — give the induction/component-counting argument.
- Union–find is the expected data structure name for Kruskal; mention "amortised near-constant (inverse Ackermann)" or just O(|E| log |V|) overall.
