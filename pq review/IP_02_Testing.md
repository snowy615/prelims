# IP-02 — Testing: Equivalence Partitioning, Unit Tests, Boundary Cases

**Frequency:** historically light, but **7/20 marks of 2025 Q1** (parts d–g). Treat as guaranteed in Q1.

---

## 1. Definitions (2-mark template)

**Black-box testing:** test cases are derived only from the *specification* (pre/post-conditions), without looking at the implementation.

**Equivalence partitioning (the 2025 wording):**
> The input domain is divided into equivalence classes such that, according to the specification, the program is expected to behave in the same way on all inputs within one class. It then suffices to test **one representative per class**, which gives good coverage of behaviours with few tests.

**White-box testing:** test cases derived from the code structure (e.g. covering every branch/path).

**Boundary case:** an input at the edge between equivalence classes or at the limit of the precondition (empty array, single element, value at `0`/`a.length−1`), where off-by-one errors are most likely — that is *why* testing it matters (this sentence is the 1 mark).

## 2. "Identify five natural equivalence classes" template (3 marks)

Pick classes by *behaviourally distinct outcomes*. For a function on `Array[Int]` returning a length/index, the standard five:

1. **Empty array** (precondition edge; special return value).
2. **Single element**.
3. **Input where the property holds everywhere** (e.g. fully increasing array → answer = length).
4. **Input where the property holds nowhere** (e.g. strictly decreasing / all equal → answer = 1 or 0).
5. **Mixed/general input** where the answer is strict in the middle (neither trivial).

Add one sentence of rationale per class: "these distinguish the initialisation, the two branches of the loop, and both extremes of the answer's range."

## 3. ScalaTest one-liner format (asked verbatim in 2025)

```scala
test("emptyArray")     { assert(lics(Array[Int]()) == 0) }
test("singleElement")  { assert(lics(Array(5)) == 1) }
test("allIncreasing")  { assert(lics(Array(1,2,3,4)) == 4) }
test("allEqual")       { assert(lics(Array(2,2,2)) == 1) }
test("mixed")          { assert(lics(Array(3,1,2,5,4)) == 3) }
```

Rules: one test per equivalence class, descriptive name, the assertion computes the **expected output by hand** — make sure your hand-computed values are actually correct, this is where marks die.

## 4. "Comprehensive unit tests" longer format (2023 Q1(d))

Cover: each equivalence class, each boundary (first/last index, target smaller/larger than all elements, duplicates → leftmost occurrence), and (for search) present vs absent values. State expected result for each.

## 5. Mark-scheme checklist

- [ ] Definition mentions *specification*, *same behaviour within class*, *one representative suffices*.
- [ ] Exactly the number of classes asked for, each with rationale.
- [ ] Tests in the exact syntactic form requested (`test(name){assert(...)}`).
- [ ] Boundary case + why it is dangerous (off-by-one / empty input).
