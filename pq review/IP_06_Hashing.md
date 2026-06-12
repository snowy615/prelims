# IP-06 — Hashing & Hash Tables

**Frequency:** only 5 old questions, but **17/20 marks of 2025 Q4** — the reform clearly promoted it. Expect it in Q4.

---

## 1. The three key properties (2025 Q4(a), 3 marks — answer verbatim)

For O(1) average-case lookup, a hash function must be:

1. **Deterministic** — the same key always yields the same hash; otherwise a stored key could be searched for in the *wrong bucket* and never found.
2. **Efficiently computable** — the hash is computed on *every* operation; if hashing cost exceeded O(1) (or scanning cost), the speed advantage over direct comparison/scan is lost.
3. **Near-uniformly distributed** — keys spread evenly over buckets, so expected bucket size is n/m = O(1) for m ∝ n buckets; without uniformity many keys collide into few buckets and lookup degenerates to O(n) linked-list scan.

## 2. Chained hash table (the recurring implementation, 2021/2023 Q5/Q3)

```scala
class Entry(val name: String, var number: String, var next: Entry)
val buckets = new Array[Entry](N)
// Abs: book = { (e.name, e.number) | e reachable from buckets(k), any k }
// DTI: every entry e in chain k satisfies hash(e.name) % N == k; names distinct

private def find(name: String): Entry = {        // helper: chain scan
  var e = buckets(hash(name) % N)
  while (e != null && e.name != name) e = e.next
  e                                                // null if absent
}
```

`store` = find-then-update-or-prepend; `recall`/`isInBook` = find + null check (say how you deal with absence: precondition vs Option vs exception).

**Diagnosis question (2021 Q5(f)):** if one chain holds a large fraction of keys, the hash function is not near-uniform for the key distribution (e.g. depends only on first letter); fix by hashing all characters with multiplier (e.g. `h = 31*h + c`) or rehashing into more buckets.

## 3. Structural hashing of trees (2025 Q4(b)) — the full pattern

```scala
def hash(): Int = {                       // h(null) = 0
  def h(n: Node): Int =
    if (n == null) 0
    else 31 * h(n.left) + n.value.hashCode + 31 * h(n.right)
  h(this)
}
```

**Avoiding recomputation (O(n) total):** compute hashes **bottom-up once** and cache them — either a `val hash` field computed at construction (immutable nodes), or one post-order pass filling a `Map[Node, Int]`. Then `findSubtree(t)`: compute `t`'s hash, walk the tree, and run the expensive `treeEquals` **only at nodes whose cached hash equals t's hash** (hash as *prefilter*).

**Limitations template (Q4(b)(iv)(v) style):**
- Hashes **collide**: equal hash ⇏ equal tree. A construction giving infinitely many false candidates: the hash is a fixed arithmetic combination, so distinct trees with permuted/compensating values share a hash (e.g. mirrored shapes whose `31·h(L)+31·h(R)` terms coincide, scaled to any size). Mitigate with asymmetric multipliers (`31·h(L) + 37·h(R)`) — reduces but never eliminates collisions (pigeonhole: infinitely many trees, finitely many Ints).
- Therefore the hash is **necessary-but-not-sufficient**: equal hashes must be confirmed by `treeEquals`; unequal hashes *do* certify inequality (with a fixed deterministic function).
- **Mutability hazard:** if fields become mutable, a cached hash goes stale after mutation — lookups then check the wrong candidates / a key changes bucket while stored. Rule: keys in hash structures must be immutable (or rehash on mutation).

## 4. Rabin–Karp / rolling hash (natural exam variant)

Hash of window `s[i..i+m)` updated in O(1) when sliding: `h' = b·(h − s(i)·b^{m−1}) + s(i+m)`. Same prefilter logic: compare strings only on hash match; expected O(n + m), worst O(nm) with adversarial collisions.

## 5. Mark-scheme checklist

- [ ] Three properties each linked to *why lookup breaks* without it.
- [ ] Abs + DTI for the bucket array.
- [ ] Caching argument for O(n) hash computation (no exponential recomputation).
- [ ] "Collisions exist by pigeonhole; hash filters candidates, equality check confirms."
- [ ] Mutability → stale hash answer ready.
