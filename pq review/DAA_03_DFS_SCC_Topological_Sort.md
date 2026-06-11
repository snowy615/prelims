# DAA #3 — Graph Traversal: DFS, SCCs, Topological Sort, BFS

**Frequency: 13/16 real papers. Since 2021 a mechanical "run DFS on this graph, give d/f times" + one SCC-graph reduction appears every single year (2025 Q6a/b) — free marks if you are fast and accurate.**
Appearances: 2010 Q8(c–f), 2011 Q8, 2013 Q8, 2015 Q7, 2016 Q6(c–e), 2017 Q6, 2018 Q7, 2020 Q4, 2021 Q7, 2022 Q6, 2023 Q6(a), 2024 Q7(b), 2025 Q6(a)(b); also 2026 mock Q6(a)(b).

---

## How it is asked

1. **Trace (4–7 marks):** run DFS in *lexicographic order*, give predecessor π, discovery d[v], finishing f[v]; classify one forward and one back edge; list the SCCs.
2. **Define + algorithm (3–6 marks):** topological sort definition + linear-time algorithm + why correct; SCC definition + Kosaraju-style algorithm + running time.
3. **Prove (2–6 marks):** topological sort possible ⟺ acyclic; reachability relation is an equivalence; parenthesis property statements true/false (2018 Q7b).
4. **Apply DFS/BFS to a new problem (5–8 marks):** bipartite testing / 2-colouring (2015, 2021), infinite trace from s (2017), semi-connected graph (2026), tree diameter via two BFS/DFS (2021), articulation points / biconnected components (2011), closest-to-source reachable vertex (2018).

---

## Memorise verbatim

**DFS:** visit each undiscovered vertex; `DFS-Visit(u)` stamps `d[u]` on discovery, recursively visits undiscovered neighbours (setting π), stamps `f[u]` when finished. Time **O(|V|+|E|)** with adjacency lists (each vertex and edge handled a constant number of times).

**Edge classification (directed):** tree (to undiscovered), back (to ancestor — exists ⟺ cycle), forward (to finished descendant), cross (everything else). Undirected: only tree and back edges.

**Parenthesis property:** intervals `[d[u],f[u]]` are either nested (descendant relation) or disjoint; never overlap partially.

**Topological sort:** a linear order of a DAG's vertices such that every edge (u,v) goes from earlier to later. Algorithm: run DFS, output vertices in **decreasing finishing time**. Correct because for any edge (u,v), v finishes before u (v is a descendant, or v already finished — no back edge in a DAG). Exists ⟺ graph acyclic (cycle ⇒ no valid order; acyclic ⇒ DFS order works). Alternative: repeatedly remove an in-degree-0 vertex (Kahn). Both O(|V|+|E|).

**SCC:** maximal set of vertices mutually reachable from each other. **Algorithm (Kosaraju):** (1) DFS on G, record finishing times; (2) compute Gᵀ (reverse all edges); (3) DFS on Gᵀ taking start vertices in decreasing f[·]; each DFS tree of pass 2 is one SCC. O(|V|+|E|). The **SCC (condensation) graph** — one node per SCC — is always a DAG.

**BFS:** layer-by-layer from s with a queue; computes unweighted shortest distances; O(|V|+|E|).

---

## Trace technique (do these mechanically)

- Keep a global clock starting at 1; every discovery and every finish increments it. With n vertices all timestamps are 1..2n.
- Lexicographic order means: outer loop tries vertices 1,2,3,…; inside, scan adjacency in increasing order.
- After the trace, sanity-check: d/f values all distinct, parenthesis property holds.
- For SCCs on small graphs you may also eyeball the cycles, but **show the method** (decreasing-f order on the reversed graph) — the question says "the algorithm from the lectures".

## Standard reductions (rehearse these)

- **Bipartite / 2-colouring:** BFS/DFS, colour alternately, fail on a same-colour edge. Correct because: graph bipartite ⟺ no odd cycle (prove both directions: odd cycle is not 2-colourable; if no odd cycle, distance-parity colouring works). O(|V|+|E|).
- **Cycle detection:** DFS, look for a back edge.
- **Semi-connected (2026 mock):** build SCC graph, topologically sort it; semi-connected ⟺ consecutive SCCs in the order are joined by an edge (a path through the condensation must be Hamiltonian).
- **Vertex reaching all others (2025 Q6b):** compute the SCC graph; such an s exists ⟺ the SCC-DAG has exactly **one source SCC** (in-degree 0): any vertex of that SCC works (everything is reachable from a source that is unique, since every SCC is reachable from some source). Verify by one DFS/BFS from a candidate. O(|V|+|E|).
- **Infinite trace from s (2017):** exists ⟺ some cycle is reachable from s ⟺ some SCC with ≥1 internal edge (size>1, or a self-loop) is reachable. DFS from s + SCC computation, O(|V|+|E|).
- **Hamiltonian path in a DAG (2016):** exists ⟺ the topological order is unique ⟺ consecutive vertices in the topological order are joined by edges.
- **Tree diameter (2021):** from any v, find farthest u; from u find farthest u′; (u,u′) realises the diameter. Prove via exchange/contradiction.
- **Edge cover on trees / leaf-based greedy (2024):** process leaves, take their incident edges.

---

## Traps

- "Lexicographical order" is part of the spec — a correct algorithm traced in the wrong order scores poorly.
- Back vs cross edge confusion: back goes to a *grey* (in-progress) vertex; cross goes to a *finished, non-ancestor* one.
- 2018 Q7(b)-style true/false: try a 3-vertex counterexample before attempting a proof. (e.g. "path u→v ⇒ f[u]>f[v]" is FALSE — v may be discovered first.)
- Decreasing finishing time, not increasing, for topological sort; on Gᵀ for Kosaraju pass 2.
- Always state O(|V|+|E|) and *why* (adjacency list, each edge looked at O(1) times). With an adjacency matrix DFS costs O(|V|²) — mention if the question gives a matrix.
