# DAA #6 — Heaps and Priority Queues

**Frequency: 8/16 real papers — but absent 2023–2025, so lower priority under the new format (still useful for Q5(b)-style room scheduling and as Dijkstra/Prim's data structure).**
Appearances: 2011 Q7, 2012 Q5(d), 2013 Q6(c-ii), 2014 Q7, 2016 Q7 (d-ary), 2019 Q6, 2021 Q5(b), 2022 Q5(b)(c).

---

## How it is asked

1. **Define + illustrate (2–4 marks):** what is a binary min/max-heap; array representation; illustrate Heapify on an example.
2. **Pseudocode (4–7 marks):** Heapify / Extract-Min / Decrease-Key / Insert / Make-Heap, with complexities. Variants: d-ary heap (2016).
3. **Heapsort (3–4 marks):** sort with a heap; O(n log n).
4. **Use a heap to solve X (4–10 marks):** kth smallest in O(n log k) (2014), maintain top-k online (2022), Extract-2Max (2021), remove arbitrary element (2019), i smallest elements (2013), Dijkstra/Prim's queue (everywhere).

---

## Memorise verbatim

**Binary max-heap:** an array A viewed as a nearly complete binary tree; children of index i are 2i+1, 2i+2 (0-based), parent ⌊(i−1)/2⌋; **heap property:** every node's key ≥ its children's keys (≤ for min-heap). Height ⌊log₂ n⌋.

**Max-Heapify(A,i):** assuming both subtrees of i are heaps, float A[i] down: swap with the larger child while smaller than it. **O(log n)** (one path down, O(1) per level). d-ary version: O(d · log_d n) — d comparisons per level, log_d n levels.

**Make-Heap (bottom-up):** for i from ⌊n/2⌋−1 down to 0, Heapify(A,i). **O(n)**, not O(n log n): Σ over heights h of (n/2^{h+1})·O(h) = O(n Σ h/2^h) = O(n). *State this — it is a favourite.*

**Priority queue ops via heap:** Max/Min O(1); Extract-Max: swap root with last, shrink, Heapify root — O(log n); Increase-Key/Decrease-Key: change and bubble up — O(log n); Insert: append, bubble up — O(log n).

**Heapsort:** Make-Heap O(n), then n × Extract — **O(n log n)**, in place. To sort *decreasing* with a *min*-heap: extract min repeatedly writing from the array's end (or note extraction order is increasing and reverse).

---

## Rehearsed applications

- **kth smallest in O(n log k) (2014):** keep a *max*-heap of size k holding the k smallest seen so far; for each remaining element, if smaller than the root, replace root and Heapify. n−k operations × O(log k) + build O(k). Root at the end = kth smallest. (Top-k online, 2022 Q5b is the same idea with a min-heap of the k largest.)
- **i smallest in sorted order (2013):** build min-heap O(n), Extract-Min i times → O(n + i log n). Compare with full sort Θ(n log n) and selection+sort Θ(n + i log i) — know all three bounds.
- **Extract-2Max (2021):** extract max O(log n), extract new max O(log n), or compare the two children of the root after one extraction; all ops stay O(log n) (Make-Max-Heap O(n), Insert O(log n)).
- **Remove arbitrary element at index i (2019):** replace A[i] with the last element, shrink, then Heapify down *or* bubble up (whichever the new key requires). O(log n). Mention both directions — that is where the marks are.
- **Minimum #exchanges to Extract-Max (2014 Q7c):** the swapped-in last element may already satisfy the heap property after the root swap: minimum is 1 exchange (root↔last); construct a 15-key example where the last leaf is large, e.g. the heap whose last leaf becomes a valid root child ordering.
- **Strong max-heap (2022 Q5c):** every value at height i+1 ≥ every value at height i ⇒ levels are sorted ranges; build by linear-time selection of the largest elements per level (sizes n/2, n/4, …) — total O(n).

---

## Traps

- Make-Heap is O(n) — saying O(n log n) loses the analysis marks.
- Heapify's precondition (subtrees already heaps) must be stated in pseudocode questions.
- Heaps give the *k* extremes in O(n log k); full sorting is wasteful — questions are usually engineered so the heap bound beats sorting; compare bounds explicitly.
- For d-ary heaps: Extract-Min costs O(d log_d n) (d−1 comparisons per level to find the min child); Decrease-Key costs O(log_d n) (parent chain only).
- Array indexing: be consistent (state 0-based or 1-based) — examiners accept either but not a mixture.
