# IP-05 — Linked Lists, Trees & Recursion

**Frequency:** linked lists 17, trees 20, recursion 26 past questions; 2025 Q4(b) is built on a recursive binary-tree class. Standard material for Q3/Q4.

---

## 1. The standard node classes

```scala
// Singly-linked list
class Node(var datum: Int, var next: Node)          // next == null at end

// Binary tree (2025 style: immutable fields)
class Node(val left: Node, val value: String, val right: Node)
// absent child = null; assert(value != null)
```

With a **dummy header node** (state the explanation, asked 2021): a sentinel first node whose datum is unused; it guarantees every real node has a predecessor, so insertion/deletion at the front needs no special case and an empty list is representable uniformly (`header.next == null`).

Concrete state/Abs for a list-implemented ADT:
`// Abs: s = [n.datum | n reachable from header.next, in order]`
`// DTI(concrete): the next-chain from header is finite (acyclic, null-terminated)`

## 2. List traversal templates

```scala
// iterative scan
var cur = header.next
// I: processed = data of nodes strictly before cur (in list order)
while (cur != null) { ...; cur = cur.next }   // variant: #nodes from cur to end
```

Deletion needs the *predecessor*: keep `var prev = header; var cur = header.next` and relink `prev.next = cur.next`. Insertion after `p`: `p.next = new Node(x, p.next)` — order matters, no node is lost.

## 3. Recursive tree templates

```scala
def size(t: Node): Int =
  if (t == null) 0 else 1 + size(t.left) + size(t.right)

def inorder(t: Node): Unit =
  if (t != null) { inorder(t.left); print(t.value); inorder(t.right) }
```

**Correctness:** structural induction on the tree — state base case (`null`), assume for subtrees, conclude for the node. **Running time:** each node visited once ⇒ O(n).

**Building a (nearly) balanced tree from an inorder array `u[a..b)`** (2022): take midpoint as root, recurse on halves; `T(n) = 2T(n/2) + O(1) = O(n)`.

**treeEquals (2025 Q4(b)(ii)) — the null-safe recursion template:**

```scala
def treeEquals(other: Node): Boolean = {
  if (other == null) return false
  def eq(s: Node, t: Node): Boolean =
    if (s == null || t == null) s == t
    else s.value == t.value && eq(s.left, t.left) && eq(s.right, t.right)
  eq(this, other)
}
```

The repeated examiner trap is **null handling**: write the `null`/`null` and `null`/non-`null` cases first.

## 4. Stacks & queues as ADTs (frequent spec target)

`// state: s : seq Int` — stack: `push` post `s = ⟨x⟩ ⌢ s0`; `pop` pre `s ≠ ⟨⟩`, post `s = tail s0 ∧ returns head s0`. Queue: enqueue at one end, dequeue at the other. Discuss the choice between **pre-condition** (caller's duty, `require`/assert) and **defensive** (return Option / throw) styles when asked "does the spec need adapting?".

Array-based circular buffer: concrete state `(data, front, size)`, `Abs: s = ⟨data((front+k) mod N) | k < size⟩`, DTI `0 ≤ front < N ∧ 0 ≤ size ≤ N`.

## 5. Iterative traversal without recursion (asked 2022)

Inorder with an explicit stack: push left spine, pop-visit, switch to right child. Quote: "the explicit stack simulates the call stack; space O(height)".

## 6. Mark-scheme checklist

- [ ] Null cases handled first in every recursive function.
- [ ] Relinking order in list surgery stated (no lost nodes).
- [ ] Structural-induction sentence for correctness of recursion.
- [ ] O(n)/O(height) bounds tied to "each node visited once".
- [ ] Abs + concrete DTI when the structure implements an ADT.
