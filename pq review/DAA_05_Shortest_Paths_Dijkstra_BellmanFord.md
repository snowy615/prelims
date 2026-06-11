# DAA #5 — Shortest Paths: Dijkstra, Bellman–Ford, DAG Shortest Paths

**Frequency: 9/16 real papers; a trace + "what if a weight changes/goes negative" twist every year 2022–2025, inside compulsory Q4 in 2023 and 2025.**
Appearances: 2011 Q7(d), 2012 Q5, 2014 Q8(d) (maximin), 2015 Q8, 2019 Q8, 2022 Q7, 2023 Q5, 2024 Q6(a), 2025 Q4(b); also 2026 mock Q4(b).

---

## How it is asked

1. **Trace (4–6 marks):** run Dijkstra showing d[v] per iteration, or Bellman–Ford showing updates per pass, edges in lexicographic order.
2. **Negative weights (3–6 marks):** small example where Dijkstra fails; does adding +W to every edge preserve shortest paths? (NO); for which x does a graph get a negative cycle and what does Bellman–Ford report?
3. **Implementation/complexity (3–6 marks):** Dijkstra with unsorted array O(|V|²) vs binary heap O((|V|+|E|) log |V|); Bellman–Ford O(|V||E|) and why (each pass relaxes all edges; |V|−1 passes suffice).
4. **Reductions / variants (6–9 marks):** most reliable path (multiply probabilities → maximise; use −log or adapt Dijkstra to max-product, 2011), shortest paths in DAGs with negative edges via topological order (2015, 2019 longest path), shortest cycle (2019), critical edges (2022), near-uniform weights → BFS-style O(|V|+|E|) (2023).

---

## Memorise verbatim

**Relaxation:** `if d[u] + w(u,v) < d[v] then d[v] := d[u] + w(u,v); π[v] := u`.

**Dijkstra (non-negative weights):** maintain set S of settled vertices; repeatedly extract the unsettled vertex u with minimum d[u], settle it, relax all edges out of u. Priority queue operations: |V| × Extract-Min, ≤|E| × Decrease-Key.
- Unsorted array: Extract-Min O(|V|), Decrease-Key O(1) → **O(|V|²)** (better for dense graphs).
- Binary heap: both O(log |V|) → **O((|V|+|E|) log |V|)**.
Correctness invariant: when u is extracted, d[u] is the true shortest distance (any other path leaves S over an unsettled vertex with d-value ≥ d[u], and weights are non-negative — *this is where negativity breaks it*).

**Bellman–Ford (arbitrary weights):** repeat |V|−1 passes; each pass relaxes every edge. After pass i, d[v] is correct for all v with a shortest path of ≤ i edges (induction — say this). A further pass that still relaxes some edge ⟺ **negative cycle reachable from the source**; the algorithm reports it. **O(|V||E|)**.

**DAG shortest paths:** topologically sort, relax vertices in topological order — one pass, **O(|V|+|E|)**, works with negative weights (no cycles). For **longest** path in a DAG: negate weights, or use max-relaxation in topological order.

---

## Trace technique

- **Dijkstra:** table with one row per iteration: settled vertex, and current d[] for all vertices. Initial: d[s]=0, rest ∞.
- **Bellman–Ford:** list updates in the required format "edge (i,j) improves d[j] to x", pass by pass, edges in lexicographic order. Stop early if a pass makes no change (note it).
- Double-check final d-values by eyeballing actual shortest paths.

## Rehearsed answers for the standard sub-questions

- **Dijkstra fails with a negative edge:** 3 vertices: s→a (2), s→b (3), b→a (−2). Dijkstra settles a with 2, but s→b→a costs 1. (Also give an example where it *succeeds* despite a negative edge, if asked — put the negative edge where it can't matter, e.g. out of a far vertex.)
- **Add +W to all edges?** Does NOT preserve shortest paths: paths with more edges are penalised more (k-edge path gains kW). Counterexample: direct edge cost 3 vs two-edge path 1+1=2; after +2: 5 vs 8 — the winner flips.
- **Most reliable path (2011):** maximise Π r(e). Either run "Dijkstra with max-product" (extract max, relax with ×; works since r∈[0,1] makes products non-increasing along paths — the same invariant survives), or take weights −log r(e) ≥ 0 and run standard Dijkstra.
- **Shortest cycle (2019):** for each vertex v (or each edge (u,v)), run Dijkstra without that edge / from v to v: min over v of (shortest path v→u + w(u,v)). O(|V|·|E| log |V|)-ish — match the bound given in the question.
- **All weights in [1, 1+1/n] (2023):** any shortest path has the fewest edges or close to it; BFS by edge count then compare — achieves O(|V|+|E|); argue: a path with k fewer edges saves ≥ k·1 but the longer path can only be cheaper by < (#edges)·(1/n) < 1.
- **Critical edges (2022):** (u,v) is critical⁻ ⟺ d(u) + w(u,v) = d(v) is *tight or within 1*… derive the condition `δ(u)+w(u,v) ≤ δ(v)+1`-style carefully from the definitions given; general method: characterise via the equality `δ(v) = δ(u) + w(u,v)` (edge on some shortest path).
- **"For which x does Dijkstra stay correct?" (2025 Q4b):** change w(u,v) to x. Dijkstra's run is correct as long as its settling order's invariant holds; analyse concretely: rerun mentally with x symbolic — typically correct for all x ≥ some bound (e.g. x ≥ 0 always safe; some negative x may *happen* to give correct distances because the edge is settled late or unused). Answer with the exact threshold for the given graph, justify by comparing the affected path lengths, and note that for x negative enough the true shortest path changes after v is already settled.

---

## Traps

- Bellman–Ford and Dijkstra produce the same *distances* on non-negative graphs but possibly **different shortest-path trees** (ties broken differently) — 2015 Q8b.
- Dijkstra is greedy; Bellman–Ford is DP-flavoured (per-pass induction). Saying which paradigm each belongs to earns marks in essay parts.
- Negative cycle ⇒ "shortest path" undefined for affected vertices; Bellman–Ford *detects* (extra-pass test), it does not fix.
- State priority-queue operations explicitly when asked to "make the priority queue operations explicit" (2012 Q5a) — Insert / Extract-Min / Decrease-Key with their counts.
