# Discrete Mathematics — Lecture Notes Summary
**Oxford MT 2025 | Andreas Galanis, Department of CS**

---

## Table of Contents
1. [L01: Sets and Induction](#l01-sets-and-induction)
2. [L02: Functions and Relations (Snippet)](#l02-functions-and-relations-snippet)
3. [L03: Counting I — Subsets and Permutations](#l03-counting-i--subsets-and-permutations)
4. [L04: Counting II — Double Counting and Inclusion-Exclusion](#l04-counting-ii--double-counting-and-inclusion-exclusion)
5. [L05: Counting Using Recurrences](#l05-counting-using-recurrences)
6. [L06: Recurrences via Generating Functions](#l06-recurrences-via-generating-functions)
7. [L07: Asymptotic Notation](#l07-asymptotic-notation)
8. [L08: Asymptotic Estimation](#l08-asymptotic-estimation)
9. [L09: Number Theory I — Divisibility and Euclid's Algorithm](#l09-number-theory-i--divisibility-and-euclids-algorithm)
10. [L10: Number Theory II — Modular Arithmetic and Congruences](#l10-number-theory-ii--modular-arithmetic-and-congruences)
11. [L11: Number Theory III — Arithmetic Functions and Primes](#l11-number-theory-iii--arithmetic-functions-and-primes)
12. [L12: The Pigeonhole Principle](#l12-the-pigeonhole-principle)
13. [L13: Relations](#l13-relations)
14. [L14: Graphs I — Basic Notions and Connectivity](#l14-graphs-i--basic-notions-and-connectivity)
15. [L15: Graphs II — Bipartite Graphs and Colourings](#l15-graphs-ii--bipartite-graphs-and-colourings)
16. [L16: Graphs III — Planar Graphs](#l16-graphs-iii--planar-graphs)
17. [Revision Overview](#revision-overview)

---

## Course Overview

The course covers five major themes (16 lectures total):
- **Sets and Functions** (L01–L02): set operations, induction, functions, bijections
- **Counting** (L03–L06): combinations, recurrences, generating functions
- **Asymptotics** (L07–L08): Big-O notation, Stirling's formula
- **Number Theory + Pigeonhole** (L09–L12): divisibility, modular arithmetic, primes, pigeonhole
- **Relations and Graphs** (L13–L16): equivalences, orders, connectivity, planarity

---

## L01: Sets and Induction

### 1.1 Sets

A **set** is an unordered collection of distinct objects (elements). Notation:
- $a \in A$: $a$ is an element of $A$
- $A \subseteq B$: every element of $A$ is in $B$ (subset)
- $A = B$: $A \subseteq B$ and $B \subseteq A$

**Standard sets:** $\emptyset$ (empty), $\mathbb{N}$, $\mathbb{Z}$, $\mathbb{Q}$, $\mathbb{R}$, $\mathbb{C}$.

### 1.2 Set Operations

| Operation | Notation | Definition |
|-----------|----------|------------|
| Union | $A \cup B$ | $\{x : x \in A \text{ or } x \in B\}$ |
| Intersection | $A \cap B$ | $\{x : x \in A \text{ and } x \in B\}$ |
| Difference | $A \setminus B$ | $\{x : x \in A \text{ and } x \notin B\}$ |
| Complement | $\overline{A}$ or $A^c$ | $U \setminus A$ (relative to universe $U$) |
| Power set | $\mathcal{P}(A)$ | $\{S : S \subseteq A\}$; has $2^{\lvert A\rvert }$ elements |
| Cartesian product | $A \times B$ | $\{(a,b) : a \in A,\, b \in B\}$ |

**De Morgan's Laws:**
$$\overline{A \cup B} = \overline{A} \cap \overline{B}, \qquad \overline{A \cap B} = \overline{A} \cup \overline{B}$$

**Distributivity:**
$$A \cap (B \cup C) = (A \cap B) \cup (A \cap C), \qquad A \cup (B \cap C) = (A \cup B) \cap (A \cup C)$$

**Inclusion-Exclusion (2 sets):** $\lvert A \cup B\rvert  = \lvert A\rvert  + \lvert B\rvert  - \lvert A \cap B\rvert $

### 1.3 Mathematical Induction

**Weak Induction:** To prove $P(n)$ for all $n \geq n_0$:
1. **Base case:** Prove $P(n_0)$.
2. **Inductive step:** Assume $P(k)$ (inductive hypothesis), prove $P(k+1)$.

**Strong Induction:** Same structure, but the inductive hypothesis is: $P(n_0), P(n_0+1), \ldots, P(k)$ all hold — then prove $P(k+1)$.

**Well-Ordering Principle:** Every non-empty subset of $\mathbb{N}$ has a least element. (Equivalent to induction.)

**Key example — Fundamental Theorem of Arithmetic (existence):** Every integer $n \geq 2$ has a prime factorisation. Proof by strong induction: if $n$ is prime we are done; otherwise $n = ab$ with $1 < a, b < n$, and by inductive hypothesis both $a$ and $b$ have prime factorisations.

---

## L02: Functions and Relations (Snippet)

### 2.1 Functions

A **function** $f: A \to B$ assigns to each $a \in A$ a unique $b \in B$.
- **Domain:** $A$
- **Codomain:** $B$
- **Image/range:** $\text{Im}(f) = f(A) = \{f(a) : a \in A\} \subseteq B$

**Types of functions:**

| Type | Definition | Equivalent condition |
|------|------------|----------------------|
| Injective (one-to-one) | $f(a_1) = f(a_2) \Rightarrow a_1 = a_2$ | $\lvert \text{Im}(f)\rvert  = \lvert A\rvert $ |
| Surjective (onto) | $\forall b \in B, \exists a: f(a) = b$ | $\text{Im}(f) = B$ |
| Bijective | injective and surjective | has a two-sided inverse |

**Composition:** $(g \circ f)(a) = g(f(a))$. If $f: A \to B$ and $g: B \to C$, then $g \circ f: A \to C$.

**Inverse:** $f: A \to B$ has an inverse $f^{-1}: B \to A$ iff $f$ is bijective.

### 2.2 Counting via Functions

- **Injections $A \to B$** exist iff $\lvert A\rvert  \leq \lvert B\rvert $.
- **Bijections $A \to B$** exist iff $\lvert A\rvert  = \lvert B\rvert $.
- **Surjections $A \to B$** exist iff $\lvert A\rvert  \geq \lvert B\rvert $ (finite case).

This connects directly to the Pigeonhole Principle (L12): no injection $f: A \to B$ when $\lvert A\rvert  > \lvert B\rvert $.

### 2.3 Cardinality

For finite sets: $\lvert A \times B\rvert  = \lvert A\rvert  \cdot \lvert B\rvert $ and $\lvert \mathcal{P}(A)\rvert  = 2^{\lvert A\rvert }$.

For infinite sets, $A$ and $B$ have the **same cardinality** if there exists a bijection $A \to B$. $\mathbb{N}$, $\mathbb{Z}$, $\mathbb{Q}$ are countably infinite; $\mathbb{R}$ is uncountable (Cantor's diagonal argument).

---

## L03: Counting I — Subsets and Permutations

### 3.1 Basic Counting Principles

- **Product rule:** If task 1 has $m$ outcomes and task 2 has $n$ outcomes (independent), total = $mn$.
- **Sum rule:** If $A$ and $B$ are disjoint sets of options, total = $\lvert A\rvert  + \lvert B\rvert $.

### 3.2 Permutations

An **ordered** selection of $k$ elements from $n$ distinct elements (without repetition):
$$P(n, k) = n \cdot (n-1) \cdots (n-k+1) = \frac{n!}{(n-k)!}$$
Special case: $k = n$ gives $n!$ permutations of $n$ objects.

### 3.3 Combinations (Binomial Coefficients)

An **unordered** selection of $k$ elements from $n$:
$$\binom{n}{k} = \frac{n!}{k!\,(n-k)!}$$

**Key identities:**
$$\binom{n}{k} = \binom{n}{n-k}, \qquad \binom{n}{0} = \binom{n}{n} = 1$$

**Pascal's Identity:** $\binom{n}{k} = \binom{n-1}{k-1} + \binom{n-1}{k}$

**Proof:** Fix one element $x$. Subsets of size $k$ either include $x$ (choose remaining $k-1$ from $n-1$) or exclude $x$ (choose $k$ from $n-1$). $\square$

**Binomial Theorem:**
$$(x + y)^n = \sum_{k=0}^{n} \binom{n}{k} x^k y^{n-k}$$
Setting $x = y = 1$: $\sum_{k=0}^n \binom{n}{k} = 2^n$.
Setting $x = 1, y = -1$: $\sum_{k=0}^n (-1)^k\binom{n}{k} = 0$.

### 3.4 Multisets and Stars-and-Bars

**Number of ways to choose $k$ items from $n$ types with repetition allowed (multiset of size $k$):**
$$\binom{n + k - 1}{k}$$
Equivalently: non-negative integer solutions to $x_1 + x_2 + \cdots + x_n = k$ is $\binom{n+k-1}{k}$.

---

## L04: Counting II — Double Counting and Inclusion-Exclusion

### 4.1 Double Counting

Count the same quantity in two different ways to derive identities.

**Example — Handshaking lemma:** In a graph $G = (V, E)$,
$$\sum_{v \in V} d(v) = 2\lvert E\rvert $$
Count endpoint-pairs: $\lvert E\rvert $ edges, each with 2 endpoints = $2\lvert E\rvert $ endpoints. Also, vertex $v$ contributes $d(v)$ endpoints. $\square$

**Example — Vandermonde's identity:**
$$\sum_{j=0}^{k} \binom{m}{j}\binom{n}{k-j} = \binom{m+n}{k}$$
**Proof:** Count committees of $k$ from $m$ men + $n$ women.

### 4.2 Inclusion-Exclusion Principle

$$\lvert A_1 \cup \cdots \cup A_n \rvert = \sum_i \lvert A_i \rvert - \sum_{i < j}\lvert A_i \cap A_j \rvert + \sum_{i<j<k}\lvert A_i \cap A_j \cap A_k \rvert - \cdots + (-1)^{n+1}\lvert A_1 \cap \cdots \cap A_n \rvert$$

In general:
$$\left\lvert\bigcup_{i=1}^n A_i\right\rvert = \sum_{\emptyset \neq S \subseteq [n]} (-1)^{\lvert S\rvert+1} \left\lvert\bigcap_{i \in S} A_i\right\rvert$$

### 4.3 Derangements

A **derangement** of $\{1, \ldots, n\}$ is a permutation $\sigma$ with $\sigma(i) \neq i$ for all $i$. The number of derangements is:
$$D(n) = n! \sum_{k=0}^{n} \frac{(-1)^k}{k!} \approx \frac{n!}{e}$$

**Proof via inclusion-exclusion:** Let $A_i$ = set of permutations fixing $i$. Derangements $= n! - \lvert A_1 \cup \cdots \cup A_n \rvert$.

For any $k$-element index set $\{i_1, \ldots, i_k\}$, the permutations fixing all $k$ positions number $(n-k)!$, so $\lvert A_{i_1} \cap \cdots \cap A_{i_k} \rvert = (n-k)!$. There are $\binom{n}{k}$ such subsets. By inclusion-exclusion:
$$\lvert A_1 \cup \cdots \cup A_n \rvert = \sum_{k=1}^n (-1)^{k-1} \binom{n}{k}(n-k)!$$

Therefore:
$$D(n) = n! - \sum_{k=1}^n (-1)^{k-1}\binom{n}{k}(n-k)! = \sum_{k=0}^n (-1)^k \binom{n}{k}(n-k)! = n!\sum_{k=0}^n \frac{(-1)^k}{k!} \qquad \square$$

---

## L05: Counting Using Recurrences

### 5.1 Recurrence Relations

A **recurrence relation** expresses $a_n$ in terms of earlier terms. Combined with initial conditions, it determines the sequence uniquely.

**Examples:**
- Factorial: $a_n = n \cdot a_{n-1}$, $a_0 = 1$
- Fibonacci: $F_n = F_{n-1} + F_{n-2}$, $F_0 = 0$, $F_1 = 1$

### 5.2 Linear Recurrences with Constant Coefficients

A **homogeneous linear recurrence of order $r$**:
$$a_n = c_1 a_{n-1} + c_2 a_{n-2} + \cdots + c_r a_{n-r}$$

**Characteristic equation:** $x^r = c_1 x^{r-1} + \cdots + c_r$

**Method:** If characteristic roots $\lambda_1, \ldots, \lambda_r$ are distinct, then:
$$a_n = A_1 \lambda_1^n + A_2 \lambda_2^n + \cdots + A_r \lambda_r^n$$
with constants determined by initial conditions.

If a root $\lambda$ has multiplicity $m$: contributes $(B_0 + B_1 n + \cdots + B_{m-1}n^{m-1})\lambda^n$.

**Fibonacci closed form:** Characteristic equation $x^2 = x + 1$, roots $\phi = \frac{1+\sqrt{5}}{2}$ (golden ratio) and $\hat\phi = \frac{1-\sqrt{5}}{2}$. Thus:
$$F_n = \frac{\phi^n - \hat\phi^n}{\sqrt{5}}$$

### 5.3 Non-Homogeneous Recurrences

For $a_n = c_1 a_{n-1} + \cdots + c_r a_{n-r} + f(n)$:
**General solution** = homogeneous solution + particular solution.

Find a particular solution by guessing a form for $f(n)$ (polynomial, exponential, etc.).

---

## L06: Recurrences via Generating Functions

### 6.1 Ordinary Generating Functions

The **ordinary generating function (OGF)** of sequence $(a_n)_{n \geq 0}$ is:

$$A(x) = \sum_{n=0}^{\infty} a_n x^n$$

Treated as a formal power series (convergence not required for combinatorial purposes).

**Standard OGFs:**
$$\frac{1}{1-x} = \sum_{n \geq 0} x^n, \qquad \frac{1}{(1-x)^k} = \sum_{n \geq 0} \binom{n+k-1}{k-1} x^n$$
$$(1+x)^n = \sum_{k=0}^n \binom{n}{k} x^k$$

### 6.2 Operations

- **Addition:** $(a_n + b_n) \leftrightarrow A(x) + B(x)$
- **Multiplication by $x^k$:** $(a_{n-k}) \leftrightarrow x^k A(x)$ (shifts sequence)
- **Differentiation:** $(n\, a_n) \leftrightarrow x A'(x)$

### 6.3 Solving Recurrences with GFs

**Method:**
1. Multiply both sides of the recurrence by $x^n$ and sum over valid $n$.
2. Express the sum as operations on $A(x)$.
3. Solve for $A(x)$ algebraically.
4. Expand $A(x)$ (partial fractions etc.) to extract $a_n$.

**Fibonacci derivation:** Multiply $F_n = F_{n-1} + F_{n-2}$ by $x^n$ and sum for $n \geq 2$:
$$F(x) - F_0 - F_1 x = x(F(x) - F_0) + x^2 F(x)$$
$$F(x) - x = xF(x) + x^2 F(x) \implies F(x)(1 - x - x^2) = x$$
$$F(x) = \frac{x}{1 - x - x^2}$$

Partial fractions using roots $\phi = \frac{1+\sqrt{5}}{2}$, $\hat\phi = \frac{1-\sqrt{5}}{2}$ of $1-x-x^2$:
$$F(x) = \frac{1}{\sqrt{5}}\left(\frac{1}{1-\phi x} - \frac{1}{1-\hat\phi x}\right) = \frac{1}{\sqrt{5}}\sum_{n\geq 0}(\phi^n - \hat\phi^n)x^n$$
giving Binet's formula $F_n = \dfrac{\phi^n - \hat\phi^n}{\sqrt{5}}$.

---

## L07: Asymptotic Notation

### 7.1 Definitions

Let $f, g: \mathbb{N} \to \mathbb{R}_{>0}$.

| Notation | Definition | Meaning |
|----------|------------|---------|
| $f = O(g)$ | $\exists c > 0, n_0$: $f(n) \leq c\,g(n)$ for all $n \geq n_0$ | $f$ grows **at most** as fast as $g$ |
| $f = \Omega(g)$ | $\exists c > 0, n_0$: $f(n) \geq c\,g(n)$ for all $n \geq n_0$ | $f$ grows **at least** as fast as $g$ |
| $f = \Theta(g)$ | $f = O(g)$ and $f = \Omega(g)$ | $f$ and $g$ grow at the **same rate** |
| $f = o(g)$ | $f(n)/g(n) \to 0$ as $n \to \infty$ | $f$ grows **strictly slower** than $g$ |
| $f \sim g$ | $f(n)/g(n) \to 1$ as $n \to \infty$ | $f$ and $g$ are **asymptotically equal** |

**Note:** $f = O(g)$ means $g$ is an **asymptotic upper bound** on $f$; $f = \Theta(g)$ is a tight bound.

### 7.2 Common Growth Rates (slowest to fastest)

$$1 \ll \log n \ll \sqrt{n} \ll n \ll n\log n \ll n^2 \ll n^3 \ll \cdots \ll 2^n \ll n!$$

### 7.3 Properties

- **Transitivity:** $f = O(g)$ and $g = O(h)$ $\Rightarrow$ $f = O(h)$.
- **Sum rule:** $O(f) + O(g) = O(f + g) = O(\max(f, g))$.
- **Product rule:** $O(f) \cdot O(g) = O(fg)$.
- If $\lim_{n\to\infty} f(n)/g(n) = L \in (0, \infty)$ then $f = \Theta(g)$.
- If $L = 0$ then $f = o(g)$; if $L = \infty$ then $g = o(f)$.

---

## L08: Asymptotic Estimation

### 8.1 Stirling's Approximation

$$n! \sim \sqrt{2\pi n} \left(\frac{n}{e}\right)^n$$
More precisely: $n! = \Theta\!\left(\sqrt{n}\,(n/e)^n\right)$.

In logarithms: $\log_2(n!) = n\log_2 n - n\log_2 e + O(\log n) = \Theta(n \log n)$.

### 8.2 Estimating Binomial Coefficients

**Central binomial coefficient:**
$$\binom{2n}{n} \sim \frac{4^n}{\sqrt{\pi n}}$$

**General case:** For $0 < \alpha < 1$ and $k = \alpha n$:
$$\binom{n}{k} \sim \frac{2^{n H(\alpha)}}{\sqrt{2\pi n \alpha(1-\alpha)}}$$
where $H(\alpha) = -\alpha\log_2\alpha - (1-\alpha)\log_2(1-\alpha)$ is the **binary entropy function**.

This gives the tight bound $\binom{n}{\alpha n} = \Theta\!\left(2^{nH(\alpha)} / \sqrt{n}\right)$.

**Useful inequalities:**
$$\frac{2^{nH(\alpha)}}{n+1} \leq \binom{n}{\alpha n} \leq 2^{nH(\alpha)}$$

---

## L09: Number Theory I — Divisibility and Euclid's Algorithm

### 9.1 Divisibility

$a \mid b$ (read: "$a$ divides $b$") if $\exists k \in \mathbb{Z}: b = ka$.

**Properties:**
- Reflexive: $a \mid a$
- Transitive: $a \mid b$ and $b \mid c$ $\Rightarrow$ $a \mid c$
- Antisymmetric (for positives): $a \mid b$ and $b \mid a$ $\Rightarrow$ $a = b$
- $a \mid b$ and $a \mid c$ $\Rightarrow$ $a \mid (sb + tc)$ for any $s, t \in \mathbb{Z}$

**Division algorithm:** $\forall a \in \mathbb{Z}, b \in \mathbb{N}^+$: $\exists! \, q, r$ with $0 \leq r < b$ and $a = qb + r$.

### 9.2 GCD and LCM

$\gcd(a, b)$ = largest positive integer dividing both $a$ and $b$.
$\text{lcm}(a, b) = ab / \gcd(a, b)$.

**Euclid's Algorithm:** $\gcd(a, b) = \gcd(b,\, a \bmod b)$, base case $\gcd(a, 0) = a$.

Terminates in $O(\log(\min(a,b)))$ steps (Fibonacci worst case).

### 9.3 Bezout's Theorem

**Theorem:** For any $a, b \in \mathbb{Z}$ (not both zero), $\exists s, t \in \mathbb{Z}$ with $\gcd(a, b) = sa + tb$.

This expresses $\gcd(a,b)$ as an **integer linear combination** of $a$ and $b$ (found by back-substituting through Euclid's algorithm — the Extended Euclidean Algorithm).

**Corollary:** $d \mid a$ and $d \mid b$ $\Rightarrow$ $d \mid \gcd(a,b)$ (i.e., $\gcd$ is the least positive element of $\{sa + tb : s, t \in \mathbb{Z}\}$).

**Cancellation lemma:** If $\gcd(a, n) = 1$ and $a \mid bc$, then $a \mid c$.

### 9.4 Fundamental Theorem of Arithmetic

Every integer $n \geq 2$ can be written uniquely (up to ordering) as a product of primes:
$$n = p_1^{e_1} p_2^{e_2} \cdots p_k^{e_k}$$

**Existence:** By strong induction. If $n$ is prime, done. Otherwise $n = ab$ with $1 < a, b < n$; by hypothesis both $a$ and $b$ have prime factorisations, so $n$ does too.

**Uniqueness:** Suppose $n = p_1 p_2 \cdots p_r = q_1 q_2 \cdots q_s$ (listing with repetition). Since $p_1 \mid q_1 \cdots q_s$, by the cancellation lemma applied repeatedly (using that each $q_j$ is prime), $p_1 = q_j$ for some $j$. Cancel $p_1 = q_j$ from both sides; repeat by induction to get $r = s$ and the factorisations agree up to ordering. $\square$

---

## L10: Number Theory II — Modular Arithmetic and Congruences

### 10.1 Congruences

$a \equiv b \pmod{n}$ iff $n \mid (a - b)$.

Congruence mod $n$ is an **equivalence relation** on $\mathbb{Z}$.

**Arithmetic properties:** If $a \equiv a'$ and $b \equiv b'$ (mod $n$), then:
$$a + b \equiv a' + b' \pmod{n}, \qquad ab \equiv a'b' \pmod{n}$$

**Cancellation:** $ac \equiv bc \pmod{n}$ and $\gcd(c, n) = 1$ $\Rightarrow$ $a \equiv b \pmod{n}$.

### 10.2 Linear Congruences

The equation $ax \equiv b \pmod{n}$ has a solution iff $\gcd(a,n) \mid b$. When solvable, it has exactly $\gcd(a,n)$ distinct solutions mod $n$.

**Modular inverse:** If $\gcd(a,n)=1$, then $a$ has a unique inverse $a^{-1}$ mod $n$ (found via Bezout/Extended Euclid).

### 10.3 Chinese Remainder Theorem (CRT)

**Theorem:** If $m_1, m_2, \ldots, m_k$ are pairwise coprime, then the system
$$x \equiv a_1 \pmod{m_1}, \quad x \equiv a_2 \pmod{m_2}, \quad \ldots, \quad x \equiv a_k \pmod{m_k}$$
has a unique solution modulo $M = m_1 m_2 \cdots m_k$.

**Proof (for $k=2$):** Let $\gcd(m,n)=1$. By Bezout, $\exists s,t \in \mathbb{Z}$ with $sm + tn = 1$. Set
$$x_0 = a \cdot tn + b \cdot sm$$
Then $x_0 \equiv a \cdot tn = a(1 - sm) \equiv a \pmod{m}$, and similarly $x_0 \equiv b \pmod{n}$.

**Uniqueness:** If $x, x'$ are both solutions, then $m \mid x'-x$ and $n \mid x'-x$, so $mn \mid x'-x$ (since $\gcd(m,n)=1$). Hence $x \equiv x' \pmod{mn}$.

For $k > 2$: apply the $k=2$ case iteratively (each step is valid since the moduli remain coprime). $\square$

**Application:** Reduces computations mod $M$ to computations mod each $m_i$ separately.

### 10.4 Fermat's Little Theorem

**Theorem:** For prime $p$ and $\gcd(a, p) = 1$:
$$a^{p-1} \equiv 1 \pmod{p}$$

**Proof:** Consider the $p-1$ nonzero residues $S = \{1, 2, \ldots, p-1\}$ and the set $aS = \{a, 2a, \ldots, (p-1)a\} \pmod{p}$. Since $\gcd(a,p)=1$, multiplication by $a$ is a bijection on $\mathbb{Z}/p\mathbb{Z} \setminus \{0\}$, so $aS = S \pmod{p}$. Multiplying all elements:
$$a \cdot 2a \cdots (p-1)a \equiv 1 \cdot 2 \cdots (p-1) \pmod{p}$$
$$a^{p-1} \cdot (p-1)! \equiv (p-1)! \pmod{p}$$
Cancelling $(p-1)!$ (which is coprime to $p$): $a^{p-1} \equiv 1 \pmod{p}$. $\square$

**Corollary:** For any prime $p$ and any $a$: $a^p \equiv a \pmod{p}$.

**Application:** Computing large powers mod a prime. E.g., $2^{100} \pmod{7}$: $2^6 \equiv 1$, $100 = 16 \cdot 6 + 4$, so $2^{100} \equiv 2^4 = 16 \equiv 2 \pmod 7$.

---

## L11: Number Theory III — Arithmetic Functions and Primes

### 11.1 Euler's Totient Function

$$\varphi(n) = \lvert \{k : 1 \leq k \leq n,\, \gcd(k, n) = 1\}\rvert $$

**Key values:**
- $\varphi(p) = p - 1$ for prime $p$
- $\varphi(p^k) = p^k - p^{k-1}$ for prime $p$, $k \geq 1$
- $\varphi$ is **multiplicative**: $\gcd(m,n) = 1 \Rightarrow \varphi(mn) = \varphi(m)\varphi(n)$

**Product formula:**
$$\varphi(n) = n \prod_{p \mid n,\, p \text{ prime}} \left(1 - \frac{1}{p}\right)$$

### 11.2 Euler's Theorem

**Theorem:** If $\gcd(a, n) = 1$, then $a^{\varphi(n)} \equiv 1 \pmod{n}$.

**Proof:** Let $S = \{r_1, r_2, \ldots, r_{\varphi(n)}\}$ be the set of residues mod $n$ that are coprime to $n$. Since $\gcd(a,n)=1$, for each $r_i \in S$ we have $\gcd(ar_i, n)=1$, so $ar_i \bmod n \in S$. The map $r_i \mapsto ar_i \bmod n$ is injective (if $ar_i \equiv ar_j$ then $r_i \equiv r_j$ by cancellation), hence a bijection on $S$. Therefore $\{ar_1, \ldots, ar_{\varphi(n)}\} \equiv S \pmod{n}$ and:
$$a^{\varphi(n)} \cdot r_1 r_2 \cdots r_{\varphi(n)} \equiv r_1 r_2 \cdots r_{\varphi(n)} \pmod{n}$$
Cancelling $r_1 \cdots r_{\varphi(n)}$ (each $r_i$ is coprime to $n$, so their product is too): $a^{\varphi(n)} \equiv 1 \pmod{n}$. $\square$

(Fermat's Little Theorem is the special case $n = p$ prime, $\varphi(p) = p-1$.)

**Example:** Last two digits of $3^{121}$ (i.e., $3^{121} \bmod 100$):
- $100 = 2^2 \cdot 5^2$, so $\varphi(100) = 100(1 - \tfrac{1}{2})(1 - \tfrac{1}{5}) = 40$
- $3^{40} \equiv 1 \pmod{100}$
- $3^{121} = (3^{40})^3 \cdot 3^1 \equiv 1^3 \cdot 3 = 3 \pmod{100}$
- Last two digits: **03**

*Caveat:* Computing $\varphi(n)$ for general $n$ requires factoring $n$, which is hard.

### 11.3 Prime Numbers

**Euclid's Theorem:** There are infinitely many primes.

**Proof:** For contradiction, suppose $p_1, \ldots, p_N$ are all primes. Let $A = p_1 p_2 \cdots p_N + 1$. No $p_i$ divides $A$ (since $A \equiv 1 \pmod{p_i}$), so $A$ has no prime divisor — contradiction. $\square$

**Related results (stated without proof):**
- **Bertrand's postulate:** For every $n > 1$, there is a prime between $n$ and $2n$.
- **Dirichlet's theorem:** For $\gcd(a, d) = 1$, the arithmetic progression $a, a+d, a+2d, \ldots$ contains infinitely many primes.

### 11.4 Distribution of Primes

Let $\pi(n)$ = number of primes $\leq n$.

**Prime Number Theorem:** $\pi(n) \sim \dfrac{n}{\ln n}$

**Corollary:** The number of primes between $n$ and $2n$ is $\pi(2n) - \pi(n) \sim \dfrac{n}{\ln n}$.

**Algorithm for generating a large prime:**
1. Pick $k$ uniformly at random from $\{n+1, \ldots, 2n\}$.
2. Test if $k$ is prime.
3. Expected number of trials until success: $\Theta(\log n)$.

### 11.5 Primality Testing

**Miller-Rabin Primality Test** *(non-examinable)*

Based on:
- Fermat's theorem: for prime $p$, $a^{p-1} \equiv 1 \pmod{p}$ if $\gcd(a,p)=1$
- Lemma: for prime $p$, $x^2 \equiv 1 \pmod{p} \Rightarrow x \equiv \pm 1 \pmod{p}$

Write $n - 1 = 2^r k$ ($k$ odd). For prime $n$, any $a$ with $\gcd(a,n)=1$ must satisfy either:
1. $a^{2^s k} \equiv -1 \pmod{n}$ for some $s \in \{1, \ldots, r\}$, or
2. $a^k \equiv 1 \pmod{n}$

**If $n$ is composite**, at least half of $a \in \{1, \ldots, n-1\}$ fail conditions (1) and (2).

**Test:** Pick $a$ randomly; if $a$ fails, output "composite". Repeat 50 times; if all pass, output "prime". False positive probability $< 2^{-50}$.

---

## L12: The Pigeonhole Principle

### 12.1 Statement

**Pigeonhole Principle:** You cannot place $n+1$ pigeons into $n$ boxes without at least two pigeons in the same box.

Equivalently: there is **no injection** $f: A \to B$ when $\lvert A\rvert  > \lvert B\rvert $.

**The hard part:** Identifying what the "pigeons" and "boxes" are.

### 12.2 Classic Examples

**Example I — Divisibility in a collection:**
Any $n+1$ integers contain distinct $a, b$ with $n \mid a - b$.

*Boxes:* remainders mod $n$ (boxes $0, 1, \ldots, n-1$). By pigeonhole, two numbers share a box, so $n$ divides their difference. $\square$

**Example II — Equal number of friends:**
Among $n$ persons in a room, two have the same number of friends.

*Key observation:* Cannot simultaneously have someone with 0 friends and someone with $n-1$ friends, so there are only $n-1$ nonempty boxes. By pigeonhole, two persons share a friend-count. $\square$

**Example III — Relatively prime pair:**
Any $(n+1)$-element subset $S$ of $\{1, 2, \ldots, 2n\}$ contains a relatively prime pair.

*Boxes:* $n$ consecutive pairs $(1,2), (3,4), \ldots, (2n-1,2n)$. By pigeonhole, $S$ contains two consecutive integers $k, k+1$ which have $\gcd = 1$. $\square$

**Example IV — Divisible pair:**
Any $(n+1)$-element subset $S$ of $\{1, \ldots, 2n\}$ contains $a, b$ with $a \mid b$.

*Boxes:* Write each $k = 2^r \rho$ ($\rho$ odd); $n$ boxes for odd parts $1, 3, 5, \ldots, 2n-1$. By pigeonhole, two elements share the same odd part: $a = 2^r(2j+1)$ and $b = 2^s(2j+1)$ with $r \neq s$; the smaller divides the larger. $\square$

**Example V — Tournament preparation:**
A football team plays $\geq 1$ and $\leq 50$ games over 30 days. Then some consecutive block of days had exactly 8 games.

*Setup:* Let $a_i$ = total games up to day $i$. Consider $a_1, \ldots, a_{30}, a_1+8, \ldots, a_{30}+8$: 60 numbers, all in $\{1, \ldots, 58\}$. By pigeonhole, two are equal; the $a_i$'s are strictly increasing and the $a_j+8$'s are strictly increasing, so the equal pair must be $a_i = a_j + 8$ for some $i > j$: exactly 8 games on days $j+1, \ldots, i$. $\square$

**Example VI — Subset sum divisible by $n$:**
Given any $n$ integers $a_1, \ldots, a_n$, there is a nonempty subset with sum divisible by $n$.

*Proof:* Consider partial sums $s_k = a_1 + \cdots + a_k$. If any $s_k \equiv 0 \pmod{n}$, done. Otherwise, the $n$ values $s_1, \ldots, s_n$ take values in $\{1, \ldots, n-1\}$ (only $n-1$ residues), so by pigeonhole $s_i \equiv s_j \pmod{n}$ for some $i > j$: then $n \mid s_i - s_j = a_{j+1} + \cdots + a_i$. $\square$

### 12.3 Advanced Applications

**Weak Fermat/Euler (via pigeonhole):**
For $\gcd(a,n)=1$, $\exists k$ such that $a^k \equiv 1 \pmod{n}$.

*Proof:* The $n+1$ powers $1, a, a^2, \ldots, a^n$ have at most $n$ distinct residues mod $n$; by pigeonhole $a^i \equiv a^j$ for $i > j$, and since $\gcd(a,n)=1$ we can cancel to get $a^{i-j} \equiv 1$. $\square$

**Dirichlet's Approximation Theorem:**
For irrational $\alpha \in (0,1)$ and $N > 1$, $\exists$ integers $m, k$ with $1 \leq m \leq N$ and $\lvert m\alpha - k\rvert  \leq 1/N$.

*Proof:* Consider fractional parts $\{0\}, \{\alpha\}, \{2\alpha\}, \ldots, \{N\alpha\}$ — these are $N+1$ points in $[0,1]$. Divide $[0,1]$ into $N$ intervals of length $1/N$. By pigeonhole, two fractional parts land in the same interval: $\lvert \{i\alpha\} - \{j\alpha\}\rvert  \leq 1/N$ for $i > j$, giving $\lvert (i-j)\alpha - ({\lfloor i\alpha \rfloor - \lfloor j\alpha \rfloor})\rvert  \leq 1/N$. Set $m = i-j$, $k = \lfloor i\alpha\rfloor - \lfloor j\alpha\rfloor$. $\square$

**Erdős–Szekeres Theorem:**
Any sequence of $n \geq (r-1)(s-1)+1$ distinct real numbers contains either an increasing subsequence of length $r$ or a decreasing subsequence of length $s$.

*Proof:* For each index $i$, let $\ell_i$ = length of the longest increasing subsequence starting at $a_i$. If any $\ell_i \geq r$ we are done. Otherwise each $\ell_i \in \{1,\ldots,r-1\}$: $n \geq (r-1)(s-1)+1$ values in $r-1$ boxes, so by pigeonhole there are $s$ indices $i_1 < \cdots < i_s$ with $\ell_{i_1} = \cdots = \ell_{i_s}$. Then $a_{i_1} > a_{i_2} > \cdots > a_{i_s}$ (if $a_{i_j} \leq a_{i_{j+1}}$, we could extend the IS starting at $a_{i_{j+1}}$, contradicting equal $\ell$ values), giving a decreasing subsequence of length $s$. $\square$

---

## L13: Relations

### 13.1 Definition and Examples

A **relation** $R$ on a set $A$ is a subset $R \subseteq A \times A$. Write $aRb$ if $(a,b) \in R$.

**Examples:**
- $\leq$ on $\mathbb{R}$: $S = \{(a,b) \in \mathbb{R}^2 : a \leq b\}$
- $\mid$ on $\mathbb{Z}$: $T = \{(a,b) : b = ak \text{ for some } k\}$
- Congruence mod $n$ on $\mathbb{Z}$

### 13.2 Properties

| Property | Definition |
|----------|------------|
| Reflexive | $\forall a \in A: aRa$ |
| Irreflexive | $\forall a \in A: \neg(aRa)$ |
| Symmetric | $\forall a,b: aRb \Rightarrow bRa$ |
| Antisymmetric | $\forall a \neq b: aRb \Rightarrow \neg(bRa)$ |
| Transitive | $\forall a,b,c: aRb \text{ and } bRc \Rightarrow aRc$ |

**Examples:**
- $\leq$ on $\mathbb{R}$: reflexive, antisymmetric, transitive (partial order)
- $<$ on $\mathbb{R}$: irreflexive, antisymmetric, transitive
- $\equiv \pmod{n}$: reflexive, symmetric, transitive (equivalence)

### 13.3 Representations

**Matrix representation:** $M(R)$ with rows/columns indexed by $A$; $M_{a,b}(R) = 1$ if $aRb$, else 0.

**Graph representation:** For each $a \in A$ a vertex; draw a directed edge $a \to b$ if $aRb$.

### 13.4 Operations on Relations

Given relations $R, S$ on the same set $A$:
- **Intersection:** $a(R \cap S)b$ iff $aRb$ and $aSb$
- **Union:** $a(R \cup S)b$ iff $aRb$ or $aSb$
- **Complement:** $a\overline{R}b$ iff $\neg(aRb)$
- **Composition:** $a(R \circ S)b$ iff $\exists c: aSc$ and $cRb$ (apply $S$ first, then $R$)
  - Not commutative in general; is associative
- **Powers:** $R^n = R \circ R \circ \cdots \circ R$ ($n$ times)

### 13.5 Closures

The **closure** of $R$ under a property $P$ is the smallest relation $R' \supseteq R$ satisfying $P$.

- **Reflexive closure:** $R' = R \cup \{(a,a) : a \in A\}$ (add self-loops)
- **Symmetric closure:** $R' = R \cup \{(b,a) : (a,b) \in R\}$ (add reverse edges)
- **Transitive closure:** $R' = R \cup R^2 \cup R^3 \cup \cdots$ (add edge $a \to b$ whenever $b$ reachable from $a$)

### 13.6 Equivalence Relations

A relation $R$ is an **equivalence relation** if it is reflexive, symmetric, and transitive.

**Equivalence class** of $a$: $[a]_R = \{b \in A : aRb\}$

**Theorem:** The equivalence classes of $R$ form a **partition** of $A$ (disjoint, non-empty sets covering all of $A$).

**Proof sketch:** 
- Every $a$ lies in $[a]_R$ (reflexivity) → classes cover $A$.
- For distinct classes $[a]$ and $[b]$: if $c \in [a] \cap [b]$, then $cRa$ and $cRb$, so $aRb$ by symmetry + transitivity, meaning $[a] = [b]$ — contradiction. → classes are disjoint. $\square$

**Converse:** Every partition $\{A_1, \ldots, A_n\}$ of $A$ defines an equivalence relation ($(a,b) \in R$ iff $a,b$ in the same $A_i$).

**Key example:** Congruence mod $n$ on $\mathbb{Z}$; equivalence classes are the residue classes $[0],[1],\ldots,[n-1]$.

### 13.7 Partial Orders and Posets

A **partial order** on $A$ is a relation that is reflexive, antisymmetric, and transitive. A **partially ordered set (poset)** is a pair $(A, \preceq)$.

**Examples:**
- $(\mathbb{N}, \mid)$: divisibility (not a total order: $2 \nmid 3$ and $3 \nmid 2$)
- $(\mathcal{P}(A), \subseteq)$: subset relation
- $(\mathbb{R}, \leq)$: the standard order (also a total order)

### 13.8 Total Orders and Comparable Elements

Two elements $a, b \in A$ are **comparable** (in $(A, \preceq)$) if $a \preceq b$ or $b \preceq a$.

A **total (linear) order** is a partial order where every two elements are comparable. We call $A$ **totally/linearly ordered** by $\preceq$.

**Notation:** Posets as $(A, \preceq)$; totally ordered sets as $(A, \leq)$.

### 13.9 Minimal and Minimum Elements

- $a$ is **minimal** in $(A, \preceq)$ if there is no $b \in A$ with $b \preceq a$ and $b \neq a$.
- $a$ is a **minimum** (least) element if $a \preceq b$ for every $b \in A$.

A minimum element is always minimal, but minimal elements need not be unique or minimum.

**Lemma:** Every finite poset has at least one minimal element.

**Proof:** Start at any $x_0$. If not minimal, there is $x_1 \prec x_0$; repeat. Since $A$ is finite, this terminates at a minimal element. $\square$

### 13.10 Linear Extensions

**Theorem:** Every finite poset $(A, \preceq)$ can be extended to a total order $(A, \leq)$ such that $a \preceq b \Rightarrow a \leq b$.

**Proof:** Induction on $n = \lvert A\rvert $. Take a minimal element $a_1$; by inductive hypothesis extend the order on $A \setminus \{a_1\}$; set $a_1 \leq a_2 \leq \cdots \leq a_n$. $\square$

**Application:** Topological sorting (see DAA).

### 13.11 Lexicographic Order

Given a total order $(A, \leq)$, the **lexicographic order** on $A \cup A^2 \cup A^3 \cup \cdots$ (tuples of any length):
$$(a_1, \ldots, a_n) \leq^{\text{lex}} (a'_1, \ldots, a'_m)$$
iff either $a$ is a prefix of $a'$, or there exists the first index $i$ where they differ and $a_i < a'_i$.

---

## L14: Graphs I — Basic Notions and Connectivity

### 14.1 Basic Definitions

A **graph** $G = (V, E)$ where:
- $V$ = vertex set (also called nodes)
- $E \subseteq V \times V$ = edge set

For an edge $e = (u, v)$: $u, v$ are **endpoints**, $e$ is **incident** to $u$ and $v$; $u$ and $v$ are **adjacent** (neighbours).

**This course:** simple, undirected graphs (no self-loops; each pair connected by at most one edge). Edges written as $\{u,v\}$.

### 14.2 Standard Graphs

- **Complete graph $K_n$:** $n$ vertices, every pair connected. $\lvert E\rvert  = \binom{n}{2}$.
- **Cycle $C_n$:** vertices $v_1, \ldots, v_n$; edges $\{v_i, v_{i+1}\}$ for $i = 1, \ldots, n$ (with $v_{n+1} = v_1$).
- **Path graph $P_n$:** $n$ vertices in a line.

### 14.3 Graph Operations

- **Complement $\overline{G}$:** same vertex set; $\{u,v\} \in \overline{E}$ iff $\{u,v\} \notin E$ (for $u \neq v$).
- **Vertex deletion $G \setminus v$:** remove $v$ and all edges incident to $v$.
- **Edge deletion $G \setminus e$:** remove edge $e$ (keep its endpoints).
- **Graph union $G_1 \cup G_2$:** vertex set $V_1 \cup V_2$, edge set $E_1 \cup E_2$.

### 14.4 Subgraphs

$G' = (V', E')$ is a **subgraph** of $G$ if $V' \subseteq V$ and $E' \subseteq E \cap (V' \times V')$.

- **Spanning subgraph:** $V' = V$.
- **Induced subgraph $G[S]$:** for $S \subseteq V$, the graph $(S, E \cap (S \times S))$ — all edges of $G$ with both endpoints in $S$.

### 14.5 Degrees and the Handshaking Lemma

The **degree** $d(v) = \lvert N(v)\rvert $ where $N(v) = \{u : \{v,u\} \in E\}$ is the neighbourhood.

**Handshaking Lemma:**
$$\sum_{v \in V} d(v) = 2\lvert E\rvert $$

**Corollary:** The number of odd-degree vertices is even.

A graph is **$k$-regular** if every vertex has degree $k$. Both $K_n$ ($(n-1)$-regular) and $C_n$ (2-regular) are regular.

### 14.6 Paths, Cycles, and Walks

- **Path:** sequence of distinct vertices $v_1, v_2, \ldots, v_k$ with $\{v_i, v_{i+1}\} \in E$; **length** = $k-1$ (number of edges).
- **Cycle:** sequence $v_1, \ldots, v_k$ with $\{v_i, v_{i+1}\} \in E$ and $\{v_k, v_1\} \in E$.
- **Walk:** like a path but vertices may repeat. By removing repeated vertices, every walk contains a path with the same endpoints.
- **Hamiltonian path/cycle:** includes every vertex.

### 14.7 Connectivity

$G$ is **connected** if for every pair $u, v \in V$ there is a path from $u$ to $v$.

**Connected component:** maximal connected subgraph. The relation "$u \sim v$ if there is a path from $u$ to $v$" is an equivalence relation; connected components are its equivalence classes.

### 14.8 Trees and Forests

A **tree** is a connected graph with no cycles. A **forest** is a graph whose connected components are trees.

A **leaf** is a vertex of degree 1.

**Key Property:** Every tree $T = (V, E)$ with $\lvert V\rvert  > 1$ contains a leaf.

**Proof:** Take the longest path $v_1, \ldots, v_k$ in $T$ (with $k \geq 2$). Claim $v_1$ has degree 1. If $v_1$ has another neighbour $v_i$ (with $i \geq 3$), that gives a cycle. If it has a neighbour $u \notin \{v_2,\ldots,v_k\}$, we can extend the path — contradiction. $\square$

**Lemma:** Every tree with $n$ vertices has exactly $n - 1$ edges.

**Proof:** Induction on $n$. For $n > 1$, remove a leaf $v$. The resulting graph is still a tree with $n-1$ vertices (and $n-2$ edges by induction), so $T$ has $n-1$ edges. $\square$

---

## L15: Graphs II — Bipartite Graphs and Colourings

### 15.1 Bipartite Graphs

$G = (V, E)$ is **bipartite** if $V = A \cup B$ with $A \cap B = \emptyset$ and every edge has one endpoint in $A$ and one in $B$. The pair $\{A, B\}$ is a **bipartition** (or the parts).

**Complete bipartite graph $K_{m,n}$:** all $mn$ possible edges between $A$ ($\lvert A\rvert =m$) and $B$ ($\lvert B\rvert =n$).

**Equivalent characterisation:** $G$ is bipartite iff it can be properly 2-coloured (vertices coloured red/green so no edge is monochromatic).

### 15.2 Characterisation via Odd Cycles

**Theorem:** $G = (V, E)$ is bipartite **iff** $G$ contains no odd cycle.

**Proof ($\Rightarrow$):** Let $V = A \cup B$ be a bipartition. Any cycle must alternate between $A$ and $B$, so its length is even. $\square$

**Proof ($\Leftarrow$):** Assume $G$ is connected (handle each component separately). Fix a vertex $v$. For any $u$, there is a path from $v$ to $u$; the key claim is that $u$ cannot be reached via both an even-length and an odd-length path (otherwise we could construct an odd cycle). So define:
$$A = \{u : \text{shortest path from } v \text{ to } u \text{ has even length}\}, \quad B = V \setminus A$$
This is a valid bipartition: for any edge $\{u,w\}$, choosing the shortest paths to $u$ and $w$ shows they have paths of different parity (i.e., one is in $A$ and one in $B$). $\square$

### 15.3 Graph Colourings

A **$q$-colouring** of $G$ is a function $\sigma: V \to \{1, 2, \ldots, q\}$.

A **proper $q$-colouring** satisfies $\sigma(u) \neq \sigma(v)$ for every edge $\{u, v\}$.

$G$ is **$q$-colourable** if it admits a proper $q$-colouring.

**Examples:**
- $G$ is 2-colourable iff $G$ is bipartite.
- $C_{2k+1}$ (odd cycle) is 3-colourable, not 2-colourable.
- $K_n$ is $n$-colourable, not $(n-1)$-colourable.
- 3-colourability is NP-hard; best algorithm takes $2^{\Theta(n)}$ time (related to **P vs NP**).

### 15.4 Sufficient Conditions for Colourability

**Definition:** The **max-degree** $\Delta$ of $G$ is the maximum degree over all vertices.

**Proposition:** Every graph $G$ with max-degree $\Delta$ is $(\Delta + 1)$-colourable.

**Proof:** Order vertices $v_1, \ldots, v_n$ arbitrarily. Colour greedily: when colouring $v_i$, it has at most $\Delta$ neighbours already coloured, so at least one of the $\Delta+1$ colours is available. $\square$

**Proposition:** If $G$ is connected with max-degree $\leq \Delta$ and contains a vertex of degree $< \Delta$, then $G$ is $\Delta$-colourable.

**Proof:** Induction on $\lvert V\rvert $; find a vertex $u$ of degree $< \Delta$, remove it, colour $G - u$ by induction, then re-insert $u$ — it has $< \Delta$ neighbours so a colour is available. $\square$

**Brooks' Theorem:** A connected graph $G$ with max-degree $\Delta$ is $\Delta$-colourable unless $G = K_{\Delta+1}$ or ($\Delta = 2$ and $G$ is an odd cycle).

### 15.5 Edge Colourings

A **proper edge $q$-colouring** assigns colours to edges so that incident edges get different colours.

**Vizing's Theorem:** For any graph $G$ with max-degree $\Delta$, $G$ is either $\Delta$-edge-colourable or $(\Delta+1)$-edge-colourable. Determining which case applies is NP-hard.

---

## L16: Graphs III — Planar Graphs

### 16.1 Planar Graphs

$G$ is **planar** if it admits a **planar drawing** (embedding): drawn in the plane with no two edges crossing.

**Examples:** $K_4$ is planar; every map corresponds to a planar graph (country = vertex, shared border = edge).

**Non-planar:** $K_5$ and $K_{3,3}$.

### 16.2 Faces in Planar Graphs

A planar drawing divides the plane into regions called **faces** (including the unbounded outer face). Denote the face set by $F$.

Each face $f \in F$ is bounded by a **closed walk**; its **degree** $d(f)$ is the length of this walk.

**Face-Edge Formula:**
$$\sum_{f \in F} d(f) = 2\lvert E\rvert $$

**Proof:** Each edge is on the boundary of exactly two faces (or appears twice in one face's boundary), so each edge is counted twice. $\square$

### 16.3 Euler's Formula

**Theorem:** For any **connected** planar graph $G = (V, E)$ with face set $F$:
$$\lvert V\rvert  - \lvert E\rvert  + \lvert F\rvert  = 2$$

**Proof:** Induction on $\lvert E\rvert $.
- **Base case:** $G$ is a tree ($\lvert E\rvert  = \lvert V\rvert -1$, $\lvert F\rvert  = 1$): $\lvert V\rvert  - (\lvert V\rvert -1) + 1 = 2$. $\checkmark$
- **Inductive step:** $G$ contains a cycle; let $e$ be an edge on a cycle. Then $G \setminus e$ has $\lvert V\rvert $ vertices, $\lvert E\rvert -1$ edges, and $\lvert F\rvert -1$ faces (merging two faces). By induction $\lvert V\rvert  - (\lvert E\rvert -1) + (\lvert F\rvert -1) = 2$, so $\lvert V\rvert  - \lvert E\rvert  + \lvert F\rvert  = 2$. $\square$

### 16.4 Non-Planarity of $K_5$ and $K_{3,3}$

**$K_5$ is non-planar:**
$\lvert V\rvert =5$, $\lvert E\rvert =10$. If planar: each face has degree $\geq 3$, so $3\lvert F\rvert  \leq 2\lvert E\rvert  = 20$, giving $\lvert F\rvert  \leq 6$. But Euler gives $\lvert F\rvert  = 2 - 5 + 10 = 7 > 6$. Contradiction. $\square$

**$K_{3,3}$ is non-planar:**
$\lvert V\rvert =6$, $\lvert E\rvert =9$. Since $K_{3,3}$ is bipartite, every cycle has length $\geq 4$, so each face has degree $\geq 4$. Then $4\lvert F\rvert  \leq 2\lvert E\rvert  = 18$, so $\lvert F\rvert  \leq 4$. But Euler gives $\lvert F\rvert  = 2 - 6 + 9 = 5 > 4$. Contradiction. $\square$

### 16.5 Kuratowski's Theorem

A **subdivision** of a graph is obtained by replacing edges with paths of length $\geq 1$ (inserting vertices along edges).

**Kuratowski's Theorem:** $G$ is planar **iff** it does not contain a subdivision of $K_5$ or $K_{3,3}$ as a subgraph.

Useful for efficiently deciding whether a given graph is planar.

### 16.6 The Four-Colour Theorem

**Theorem:** Every planar graph is 4-colourable (vertices can be coloured with 4 colours so adjacent vertices have different colours).

5 colours suffice by a degree argument; 4 is the tight bound (the Four-Colour Theorem). The only known proofs use **computer assistance** to check ~1500 cases:
- Appel and Haken (1976): $\sim 1500$ cases.
- Robertson, Sanders, Seymour, Thomas (1998): simplified to $\sim 600$ cases.

---

## Revision Overview

*(From L16 revision slides)*

| Topic | Lectures | Key content |
|-------|----------|-------------|
| Sets and Functions | L01–L02 | set operations, bijections, induction |
| Counting | L03–L06 | combinations, double counting, recurrences, generating functions |
| Asymptotic Estimation | L07–L08 | Big-O notation, Stirling's formula, entropy bound on binomials |
| Number Theory + Pigeonhole | L09–L12 | modular arithmetic, Euclid's algorithm, Bezout, primes, Fermat/Euler theorem |
| Relations and Graphs | L13–L16 | equivalences, partial/total orders, graphs: connectivity, bipartiteness, colourings, planarity |

**Revision tips:**
- Review lecture slides + problem sheets.
- Exam questions will be easier than problem sheets.
- Past papers are good practice.
- **New topics** (compared to previous years): generating functions, Fermat/Euler theorems, graphs.
- **Not covered** (compared to previous years): some other types of relations.

---

*Notes compiled from lectures by Andreas Galanis, Department of CS, University of Oxford, Michaelmas Term 2025.*
