# Introduction to Proof Systems — Lecture Notes
**Oxford TT 2026 | Christoph Haase** *(based on notes by James Worrell)*

---

## Table of Contents
1. [L1: Syntax of Propositional Logic](#l1-syntax-of-propositional-logic)
2. [L2: Semantics of Propositional Logic](#l2-semantics-of-propositional-logic)
3. [L3: Minimal Logic (Hilbert-style)](#l3-minimal-logic-hilbert-style)
4. [L4: Equational Reasoning](#l4-equational-reasoning)
5. [L5: Resolution](#l5-resolution)
6. [L6: Natural Deduction and Sequent Calculus](#l6-natural-deduction-and-sequent-calculus)
7. [L7: The Compactness Theorem](#l7-the-compactness-theorem)
8. [L8: First-Order Logic — Syntax and Semantics](#l8-first-order-logic--syntax-and-semantics)
9. [L9: Skolem Form](#l9-skolem-form)
10. [L10: Herbrand's Theorem and Ground Resolution](#l10-herbrands-theorem-and-ground-resolution)
11. [L11: Natural Deduction and Sequent Calculus for FOL](#l11-natural-deduction-and-sequent-calculus-for-fol)

---

## Course Overview

A **logic** comprises:
- an unambiguous language to formally express statements, and
- a mathematical framework to determine whether a statement is true.

The course covers **propositional logic** and **first-order logic** (FOL), together with proof systems for each: Hilbert-style calculi, equational reasoning, resolution, natural deduction, and sequent calculus.

Two schools of thought:
- **Tarski school**: assign truth values to atomic propositions; determine meaning of compound formulas compositionally.
- **Hilbert school**: treat formulas as purely syntactic objects; a formula is valid if derivable from known valid formulas by fixed rules.

---

## L1: Syntax of Propositional Logic

### 1.1 Formulas

**Definition 1.** Let $X = \{x_1, x_2, \ldots\}$ be a countably infinite set of **propositional variables**. **Formulas** of propositional logic are defined inductively:
1. $\mathit{true}$ and $\mathit{false}$ are formulas.
2. Every propositional variable $x_i$ is a formula.
3. If $F$ is a formula, then $\neg F$ is a formula.
4. If $F$ and $G$ are formulas, then $(F \wedge G)$ and $(F \vee G)$ are formulas.

We call $\neg F$ the **negation**, $(F \wedge G)$ the **conjunction**, and $(F \vee G)$ the **disjunction** of $F$ and $G$. The connectives $\neg, \wedge, \vee$ are **primitive**. We denote by $\mathcal{F}(X)$ the set of all formulas over $X$.

### 1.2 Derived Connectives

These are syntactic abbreviations (macros), not part of the official language:

| Connective | Definition |
|------------|------------|
| $F_1 \to F_2$ (implication) | $\neg F_1 \vee F_2$ |
| $F_1 \leftrightarrow F_2$ (bi-implication) | $(F_1 \to F_2) \wedge (F_2 \to F_1)$ |
| $F_1 \oplus F_2$ (exclusive or) | $(F_1 \wedge \neg F_2) \vee (\neg F_1 \wedge F_2)$ |
| $\bigwedge_{i=1}^n F_i$ | $(\cdots((F_1 \wedge F_2) \wedge F_3) \wedge \cdots \wedge F_n)$ |
| $\bigvee_{i=1}^n F_i$ | $(\cdots((F_1 \vee F_2) \vee F_3) \vee \cdots \vee F_n)$ |

**Operator precedences** (tightest first): $\neg$, then $\wedge$ and $\vee$, then $\to$ and $\leftrightarrow$. Indexed conjunction/disjunction bind weakest. Example: $\neg x \wedge y \to z$ parses as $((\neg x) \wedge y) \to z$.

The left-hand side of $\to$ is the **antecedent**; the right-hand side is the **consequent**.

### 1.3 Normal Forms

A **literal** is an atomic formula ($x$) or its negation ($\neg x$).

- **Conjunctive Normal Form (CNF):** $F = \bigwedge_{i=1}^n \bigvee_{j=1}^{m_i} L_{i,j}$, a conjunction of disjunctions of literals. Each disjunction is a **clause**.
- **Disjunctive Normal Form (DNF):** $F = \bigvee_{i=1}^n \bigwedge_{j=1}^{m_i} L_{i,j}$, a disjunction of conjunctions of literals.

If each clause has exactly $k$ literals, $F$ is in **$k$-CNF** (resp. **$k$-DNF**).

**CNF as sets:** A CNF formula is represented as a set of clauses $\{\{L_1, \ldots\}, \{L_2, \ldots\}, \ldots\}$, where each clause is a set of literals. Associativity, commutativity, and idempotence of $\wedge$ and $\vee$ are implicit. The **empty clause** $\square$ (empty disjunction) is equivalent to $\mathit{false}$. The **empty set of clauses** is equivalent to $\mathit{true}$.

### 1.4 Structural Induction

Functions on formulas are defined by **structural induction**: specify on base cases ($\mathit{true}$, $\mathit{false}$, variables) and on each connective. Example: the subformula function $\mathit{sub}: \mathcal{F}(X) \to \mathcal{P}(\mathcal{F}(X))$:
$$\mathit{sub}(\mathit{true}) := \{\mathit{true}\}, \quad \mathit{sub}(x) := \{x\}, \quad \mathit{sub}(\neg F) := \mathit{sub}(F) \cup \{\neg F\}$$
$$\mathit{sub}(F \wedge G) := \mathit{sub}(F) \cup \mathit{sub}(G) \cup \{F \wedge G\}$$

---

## L2: Semantics of Propositional Logic

### 2.1 Assignments and Satisfaction

**Definition 2.** An **assignment** is a function $\mathcal{A}: X \to \{0,1\}$ (where $0 = \mathit{false}$, $1 = \mathit{true}$). It extends to all formulas $\hat{\mathcal{A}}: \mathcal{F}(X) \to \{0,1\}$ by structural induction:
1. $\hat{\mathcal{A}}(\mathit{false}) = 0$, $\hat{\mathcal{A}}(\mathit{true}) = 1$
2. $\hat{\mathcal{A}}(x) := \mathcal{A}(x)$ for $x \in X$
3. $\hat{\mathcal{A}}(\neg F) := 1$ if $\hat{\mathcal{A}}(F) = 0$, else $0$
4. $\hat{\mathcal{A}}(F \wedge G) := 1$ if $\hat{\mathcal{A}}(F) = 1$ and $\hat{\mathcal{A}}(G) = 1$, else $0$
5. $\hat{\mathcal{A}}(F \vee G) := 1$ if $\hat{\mathcal{A}}(F) = 1$ or $\hat{\mathcal{A}}(G) = 1$, else $0$

We write $\mathcal{A} \models F$ ("$\mathcal{A}$ satisfies $F$", "$\mathcal{A}$ is a **model** of $F$") when $\hat{\mathcal{A}}(F) = 1$.

**Update notation:** $\mathcal{A}_{[y \to b]}(x) := b$ if $x = y$, else $\mathcal{A}(x)$.

**Key observations on derived connectives:**
- $\mathcal{A}(F \to G) = 0$ only when $\mathcal{A}(F) = 1$ and $\mathcal{A}(G) = 0$. An implication with a false antecedent is **vacuously** true (*ex falso quodlibet*).
- $\mathcal{A}(F \oplus G) = \mathcal{A}(F) + \mathcal{A}(G) \pmod{2}$ (addition mod 2).

### 2.2 Key Semantic Notions

**Definition 4.** Let $F, G \in \mathcal{F}(X)$, $\mathcal{S} \subseteq \mathcal{F}(X)$:

| Term | Definition |
|------|-----------|
| **Satisfiable** | $F$ has at least one model |
| **Unsatisfiable** | $F$ has no model |
| **Valid / Tautology** ($\models F$) | every assignment satisfies $F$ |
| **$\mathcal{S}$ entails $F$** ($\mathcal{S} \models F$) | every model of $\mathcal{S}$ is a model of $F$ |
| **Logically equivalent** ($F \equiv G$) | $F \models G$ and $G \models F$ |
| **Equi-satisfiable** | $F$ satisfiable iff $G$ satisfiable |

**Key relationships:** $F$ is a tautology iff $\neg F$ is unsatisfiable. $F \equiv G$ iff $F \leftrightarrow G$ is a tautology.

The **Boolean satisfiability problem (SAT)**: determine whether a given propositional formula is satisfiable. It is the prototypical NP-complete problem; no polynomial-time algorithm is known.

### 2.3 Encoding Constraint Satisfaction

SAT can encode combinatorial problems as propositional formulas:

**Hamiltonian path:** For graph $G = (V, E)$ with $V = \{1,\ldots,n\}$, introduce variable $x_{i,j}$ ("vertex $i$ is the $j$th on the path") and $e_{i,j}$ ("there is an edge between $i$ and $j$"). The formula $F = F_1 \wedge F_2 \wedge F_3 \wedge F_4$ is satisfiable iff $G$ has a Hamiltonian path:
$$F_1 = \bigwedge_{i=1}^n \bigvee_{j=1}^n x_{i,j} \quad (\text{each vertex visited at least once})$$
$$F_2 = \bigwedge_{i=1}^n \bigwedge_{1 \leq j \neq k \leq n} \neg(x_{i,j} \wedge x_{i,k}) \quad (\text{not at two positions})$$
$$F_3 = \bigwedge_{i=1}^n \bigwedge_{k=1}^n \bigwedge_{j=1}^{n-1} x_{i,j} \wedge x_{k,j+1} \to e_{i,k} \quad (\text{path goes along edges})$$

**Sudoku:** For a $9 \times 9$ grid, $x_{i,j,k}$ means "position $(i,j)$ contains number $k$". The conjunction of row/column/block/uniqueness constraints $F_1 \wedge \cdots \wedge F_5$ is satisfiable iff the puzzle has a solution. The Sudoku formula has $9^3 = 729$ variables (truth table size $> 10^{200}$) but modern SAT solvers find solutions in milliseconds.

---

## L3: Minimal Logic (Hilbert-style)

### 3.1 The Minimal Calculus $\mathbf{M}_0$

The **Hilbert school** treats formulas syntactically. The **minimal calculus $\mathbf{M}_0$** consists of axioms and one inference rule. $\to$ is treated as a syntactic primitive here (not a derived connective).

**Axioms of $\mathbf{M}_0$** (template variables $A, B, C$ stand for arbitrary formulas):

| Axiom | Formula |
|-------|---------|
| PL1 | $A \to (A \wedge A)$ |
| PL2 | $(A \wedge B) \to (B \wedge A)$ |
| PL3 | $(A \to B) \to [(A \wedge C) \to (B \wedge C)]$ |
| PL4 | $[(A \to B) \wedge (B \to C)] \to (A \to C)$ |
| PL5 | $B \to (A \to B)$ |
| PL6 | $(A \wedge (A \to B)) \to B$ |
| PL7 | $A \to (A \vee B)$ |
| PL8 | $(A \vee B) \to (B \vee A)$ |
| PL9 | $[(A \to C) \wedge (B \to C)] \to [(A \vee B) \to C]$ |
| PL10 | $[(A \to B) \wedge (A \to \neg B)] \to \neg A$ |

**Inference rule — Modus Ponens (MP):** From $A$ and $A \to B$, infer $B$.

### 3.2 Derivations and Theorems

A **derivation** in $\mathbf{M}_0$ is a finite sequence of formulas $A_1, \ldots, A_n$ where each $A_i$ is either an axiom or obtained by MP from $A_j$ and $A_k$ for some $j,k < i$.

A formula $B$ is a **theorem** (provable) in $\mathbf{M}_0$, written $\vdash_{\mathbf{M}_0} B$, if $B = A_n$ for some derivation. Formulas not in the derivation are **hypotheses**.

**Example — $\wedge$-Introduction:** The following derivation from hypotheses $C$ and $D$ proves $C \wedge D$:

| Step | Formula | Justification |
|------|---------|---------------|
| 1 | $\vdash C$ | hypothesis |
| 2 | $\vdash C \to (D \to C)$ | PL5 |
| 3 | $\vdash D \to C$ | MP 1,2 |
| 4 | $\vdash (D \to C) \to [(D \wedge D) \to (C \wedge D)]$ | PL3 |
| 5 | $\vdash (D \wedge D) \to (C \wedge D)$ | MP 3,4 |
| 6 | $\vdash D \to (D \wedge D)$ | PL1 |
| 7 | $\vdash D$ | hypothesis |
| 8 | $\vdash D \wedge D$ | MP 6,7 |
| 9 | $\vdash C \wedge D$ | MP 5,8 |

This yields the derived rule $\wedge_\text{Intro}$: if $\vdash_{\mathbf{M}_0} C$ and $\vdash_{\mathbf{M}_0} D$, then $\vdash_{\mathbf{M}_0} C \wedge D$.

Using $\wedge_\text{Intro}$, another derivation proves $\to_\text{Trans}$: if $\vdash A \to B$ and $\vdash B \to C$, then $\vdash A \to C$ (using PL4).

### 3.3 Limits of $\mathbf{M}_0$

$\mathbf{M}_0$ is **minimal**: none of its axioms can be derived from the others. Crucially, the following are **not derivable**:
- *ex falso quodlibet*: $\neg A \to (A \to B)$
- *tertium non datur*: $\neg\neg A \to A$

**Proof that $\nvdash_{\mathbf{M}_0} \neg A \to (A \to B)$** (Proposition 7): Define a valuation $h$ assigning 0-1 values to formulas via the tables:

| $h(F)$ | $h(G)$ | $h(F \to G)$ | $h(F \wedge G)$ | $h(F \vee G)$ | $h(\neg F)$ |
|--------|--------|--------------|-----------------|----------------|-------------|
| 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 1 | 0 | — |
| 1 | 0 | 0 | 1 | 0 | — |
| 1 | 1 | 0 | 1 | 1 | 0 |

*(Note: in $h$, negation and $\to$ behave differently from truth-table semantics.)*

Set $h(A) = 0$ and $h(B) = 1$. One can verify: all axioms PL1–PL10 evaluate to 0 under $h$, and MP preserves 0 (if $h(A) = 0$ and $h(A \to B) = 0$ then $h(B) = 0$). But $h(\neg A \to (A \to B)) = h(0 \to (0 \to 1)) = 1 \neq 0$. By induction on derivation length, every theorem evaluates to 0 under $h$, so $\neg A \to (A \to B)$ is not a theorem. $\square$

### 3.4 Extensions: Intuitionistic and Classical Logic

Adding axioms to $\mathbf{M}_0$:
- **PL11.** $\neg A \to (A \to B)$ — gives **intuitionistic logic $\mathbf{J}_0$**
- **PL12.** $\neg\neg A \to A$ — adding this to $\mathbf{J}_0$ gives **classical logic $\mathbf{K}_0$**

**Theorem 8.** For any propositional formula $F$: $\models F$ if and only if $\vdash_{\mathbf{K}_0} F$.

That is, $\mathbf{K}_0$ is sound and complete for classical propositional logic.

**Why minimalism matters:** The classical principle PL12 (excluded middle) enables highly non-constructive proofs:

> **Theorem 6.** *There exist irrational numbers $a$ and $b$ such that $a^b$ is rational.*
>
> *Proof.* Consider $\sqrt{2}^{\sqrt{2}}$. By excluded middle, it is either rational or irrational.
> - If $\sqrt{2}^{\sqrt{2}}$ is rational: take $a = b = \sqrt{2}$. Then $a^b = \sqrt{2}^{\sqrt{2}}$ is rational. $\square$
> - If $\sqrt{2}^{\sqrt{2}}$ is irrational: take $a = \sqrt{2}^{\sqrt{2}}$ and $b = \sqrt{2}$. Then $a^b = (\sqrt{2}^{\sqrt{2}})^{\sqrt{2}} = \sqrt{2}^2 = 2$, which is rational. $\square$

This proof doesn't tell us *which* case holds — it is non-constructive.

---

## L4: Equational Reasoning

### 4.1 Substitution

Equational reasoning transforms a formula into a sequence of equivalent formulas using Boolean algebra axioms. The key operation is **substitution of equals for equals**.

$G[F/H]$ denotes $G$ with all occurrences of subformula $H$ replaced by $F$. Formally, writing $=$ for syntactic equality:
$$G[F/H] := F \quad \text{if } G = H$$
If $G \neq H$: $x[F/H] := x$; $(\neg G)[F/H] := \neg(G[F/H])$; $(G_1 \wedge G_2)[F/H] := G_1[F/H] \wedge G_2[F/H]$; etc.

**Theorem 9 (Substitution Theorem).** If $G' = G[F/H]$ and $F \equiv H$, then $G' \equiv G$.

*Proof.* By structural induction on $G$. If $G = H$, then $G' = F \equiv H = G$. Otherwise, induct on the structure: for $G = \neg J$, $G' = \neg(J[F/H])$; by inductive hypothesis $J[F/H] \equiv J$; hence $G' = \neg J' \equiv \neg J = G$. Cases $G = G_1 \wedge G_2$ and $G = G_1 \vee G_2$ are analogous. $\square$

### 4.2 Boolean Algebra Axioms

The following logical equivalences hold for all formulas $A, B, C$:

| Equivalence | Name |
|-------------|------|
| $A \wedge A \equiv A$, $A \vee A \equiv A$ | **Idempotence** |
| $A \wedge B \equiv B \wedge A$, $A \vee B \equiv B \vee A$ | **Commutativity** |
| $(A \wedge B) \wedge C \equiv A \wedge (B \wedge C)$, $(A \vee B) \vee C \equiv A \vee (B \vee C)$ | **Associativity** |
| $A \wedge (A \vee B) \equiv A$, $A \vee (A \wedge B) \equiv A$ | **Absorption** |
| $A \wedge (B \vee C) \equiv (A \wedge B) \vee (A \wedge C)$ | **Distributivity** |
| $A \vee (B \wedge C) \equiv (A \vee B) \wedge (A \vee C)$ | **Distributivity** |
| $\neg\neg A \equiv A$ | **Double negation** |
| $\neg(A \wedge B) \equiv \neg A \vee \neg B$, $\neg(A \vee B) \equiv \neg A \wedge \neg B$ | **De Morgan's laws** |
| $A \vee \neg A \equiv \mathit{true}$, $A \wedge \neg A \equiv \mathit{false}$ | **Complementation** |
| $A \vee \mathit{true} \equiv \mathit{true}$, $A \wedge \mathit{false} \equiv \mathit{false}$ | **Zero laws** |
| $A \vee \mathit{false} \equiv A$, $A \wedge \mathit{true} \equiv A$ | **Identity laws** |

The Boolean algebra axioms come in **dual pairs**: each equivalence for $\wedge$ has a counterpart for $\vee$ (interchange $\wedge \leftrightarrow \vee$ and $\mathit{true} \leftrightarrow \mathit{false}$).

### 4.3 Equational Proofs

An **equational proof** of $F \equiv G$ is a sequence $F_1, F_2, \ldots, F_n$ with $F = F_1$, $G = F_n$, and each $F_{i+1}$ obtained from $F_i$ by substituting a subformula with an equivalent one (by a Boolean algebra axiom).

**Example:** Equational proof of $(x \vee (z \wedge y)) \wedge (y \vee \neg x) \equiv y \vee (\neg x \wedge z)$:

$$\begin{aligned}
(x \vee (z \wedge y)) \wedge (y \vee \neg x) &\equiv ((x \vee z) \wedge (x \vee y)) \wedge (y \vee \neg x) &&\text{(distributivity)}\\
&\equiv (y \vee (x \wedge z)) \wedge (y \vee \neg x) &&\text{(commutativity)}\\
&\equiv y \vee ((x \wedge z) \wedge \neg x) &&\text{(distributivity)}\\
&\equiv y \vee (\neg x \wedge (x \wedge z)) &&\text{(commutativity)}\\
&\equiv y \vee ((\neg x \wedge x) \wedge z) &&\text{(associativity)}\\
&\equiv y \vee (\mathit{false} \wedge z) &&\text{(complementation)}\\
&\equiv y \vee (\neg x \wedge z) &&\text{(identity laws)}
\end{aligned}$$

### 4.4 Soundness and Completeness

**Soundness:** Since all Boolean algebra axioms hold in classical logic, each step $F_i \equiv F_{i+1}$ is a valid equivalence. Hence equational reasoning is sound: an equational proof of $F \equiv G$ proves $F \equiv G$.

**Completeness (Theorem 11):** Equational reasoning is also complete: if $F \equiv G$, there exists an equational proof of $F \equiv G$.

*Key lemma (Lemma 10): Every formula $F$ over $n$ variables has an equivalent formula in **full DNF**, where every term is a conjunction of exactly $n$ literals (one per variable).*

*Proof of Lemma 10:*
1. Apply De Morgan's laws and $\neg\mathit{true} \equiv \mathit{false}$, $\neg\mathit{false} \equiv \mathit{true}$ to push all negations to atomic level (obtain **negation normal form**).
2. Apply distributivity to obtain DNF.
3. For each disjunct $D$ missing variable $x_i$, expand: $D \equiv D \wedge \mathit{true} \equiv D \wedge (x_i \vee \neg x_i) \equiv (D \wedge x_i) \vee (D \wedge \neg x_i)$. $\square$

*Proof of Theorem 11:* The full DNF is a canonical form: two logically equivalent formulas have the same full DNF (up to reordering). Given $F \equiv G$, equationally transform $F$ to its full DNF $H$ (Lemma 10), and reverse the steps from $G$ to $H$. This gives an equational proof $F \equiv H \equiv G$. $\square$

---

## L5: Resolution

### 5.1 Setup

**Resolution** is a proof calculus with a single inference rule. It operates on CNF formulas (represented as sets of clauses).

Given a literal $L$, its **complementary literal** is:
$$\overline{L} := \begin{cases} \neg p & \text{if } L = p \\ p & \text{if } L = \neg p \end{cases}$$

**Definition 12.** Let $C_1$ and $C_2$ be clauses. A clause $R$ is a **resolvent** of $C_1$ and $C_2$ if there exists a literal $L \in C_1$ with $\overline{L} \in C_2$ such that:
$$R = (C_1 \setminus \{L\}) \cup (C_2 \setminus \{\overline{L}\})$$
We write this as $\dfrac{C_1 \quad C_2}{R}$.

**Example:** $\{p_1, p_3, \neg p_4\}$ and $\{\neg p_2, p_3\}$ resolve to $\{p_1, p_3, \neg p_4\}$? No — they share no complementary literals. But $\{p_1\}$ and $\{\neg p_1\}$ resolve to $\square$ (the empty clause, equivalent to $\mathit{false}$).

### 5.2 Derivations and Refutations

A **derivation** of clause $C$ from a set of clauses $F$ is a sequence $C_1, C_2, \ldots, C_m$ with $C_m = C$ and each $C_i$ either in $F$ or a resolvent of two earlier clauses. A derivation of $\square$ from $F$ is a **refutation** of $F$.

**Definition 14.** The set of all clauses derivable from $F$ by resolution:
$$\mathrm{Res}^0(F) = F, \quad \mathrm{Res}^{n+1}(F) = \mathrm{Res}(\mathrm{Res}^n(F)), \quad \mathrm{Res}^*(F) = \bigcup_{n \geq 0} \mathrm{Res}^n(F)$$
where $\mathrm{Res}(F) = F \cup \{R : R \text{ is a resolvent of two clauses in } F\}$.

**Proposition 15.** $C \in \mathrm{Res}^*(F)$ iff there is a derivation of $C$ from $F$.

### 5.3 Soundness and Completeness

**Lemma 16 (Resolution Lemma).** If $R$ is a resolvent of $C_1, C_2 \in F$, then $F \equiv F \cup \{R\}$.

*Proof.* The $\supseteq$ direction is obvious. For $\subseteq$: suppose $\mathcal{A} \models F$ and $R = (C_1 \setminus \{L\}) \cup (C_2 \setminus \{\overline{L}\})$.
- If $\mathcal{A}(L) = 1$: then since $\mathcal{A} \models C_2$, we must satisfy some literal in $C_2 \setminus \{\overline{L}\}$, so $\mathcal{A} \models R$.
- If $\mathcal{A}(\overline{L}) = 1$: then since $\mathcal{A} \models C_1$, we satisfy some literal in $C_1 \setminus \{L\}$, so $\mathcal{A} \models R$. $\square$

**Theorem 17 (Soundness).** If there is a derivation of $\square$ from $F$, then $F$ is unsatisfiable.

*Proof.* By repeated application of the Resolution Lemma, $F \equiv F \cup \{C_1, \ldots, C_m\}$. Since this set contains $\square$, it is unsatisfiable, so $F$ is unsatisfiable. $\square$

**Theorem 18 (Completeness).** If $F$ is unsatisfiable then there is a derivation of $\square$ from $F$.

*Proof.* By induction on the number $n$ of propositional variables in $F$.

*Base case* ($n = 0$): $F$ mentions no variables, so $F$ is either $\emptyset$ (equivalent to $\mathit{true}$, but $F$ is unsatisfiable — impossible) or $F = \{\square\}$, which is a one-step refutation.

*Inductive step*: Let $p_n$ be a variable in $F$. Define $F_0 = F[\mathit{false}/p_n]$ and $F_1 = F[\mathit{true}/p_n]$. Both are unsatisfiable (since $F$ is) and mention one fewer variable. By the inductive hypothesis, there exist resolution refutations of $F_0$ and $F_1$. Replace each $C_i$ in the $F_0$-proof derived from some $C \cup \{p_n\} \in F$ with the original clause and propagate $p_n$ through the proof. Either the proof already refutes $F$ (we get $\square$) or it derives $\{p_n\}$. Similarly, the $F_1$-proof either refutes $F$ or derives $\{\neg p_n\}$. Gluing the two derivations and resolving $\{p_n\}$ with $\{\neg p_n\}$ gives $\square$. $\square$

### 5.4 Tseitin Transformation (CNF Conversion)

The smallest logically equivalent CNF of $F$ can be exponentially large. Instead, we build an **equi-satisfiable** CNF of polynomial size using fresh variables:

For each non-literal subformula $G$ of $F$, introduce a fresh variable $p_G$. Write the formula $p_G \leftrightarrow G'$ (where $G'$ has immediate subformulas replaced by their proxy variables), convert each biconditional to CNF using equational reasoning. The conjunction of all these CNF formulas, plus the unit clause $\{p_F\}$ (asserting the whole formula is true), is equi-satisfiable with $F$.

**Example:** $F = \neg(p \wedge q) \wedge r$. Subformulas: $p \wedge q$, $\neg(p \wedge q)$, $\neg(p \wedge q) \wedge r$. Introduce $x_{(p \wedge q)}$, $x_{\neg(p \wedge q)}$, $x_{(\neg(p \wedge q)) \wedge r}$. From $x_{(p \wedge q)} \leftrightarrow (p \wedge q)$:
$$(\neg x_{(p \wedge q)} \vee p) \wedge (\neg x_{(p \wedge q)} \vee q) \wedge (\neg p \vee \neg q \vee x_{(p \wedge q)})$$

---

## L6: Natural Deduction and Sequent Calculus

### 6.1 Natural Deduction

**Natural deduction** mirrors the inference patterns of working mathematicians. It has **no axioms**; proofs start from **assumptions** (temporarily assumed formulas). Each connective has:
- an **introduction rule** (I-rule): how to prove a formula with that connective as main symbol
- an **elimination rule** (E-rule): how to use such a formula

Certain rules **discharge** assumptions: the assumption ceases to be open once the discharging rule is applied. In diagrams, square brackets $[A]$ denote a dischargeable assumption.

#### Inference Rules

**Conjunction:**
$$\frac{A \quad B}{A \wedge B}\ \wedge\text{I} \qquad \frac{A \wedge B}{A}\ \wedge\text{E} \qquad \frac{A \wedge B}{B}\ \wedge\text{E}$$

**Disjunction:**
$$\frac{A}{A \vee B}\ \vee\text{I} \qquad \frac{B}{A \vee B}\ \vee\text{I} \qquad \frac{A \vee B \quad \begin{matrix}{[}A{]} \\ \vdots \\ C\end{matrix} \quad \begin{matrix}{[}B{]} \\ \vdots \\ C\end{matrix}}{C}\ \vee\text{E}$$

The $\vee$E rule formalises **proof by cases**: both $[A]$ and $[B]$ are discharged.

**Implication:**
$$\frac{\begin{matrix}{[}A{]} \\ \vdots \\ B\end{matrix}}{A \to B}\ {\to}\text{I} \qquad \frac{A \to B \quad A}{B}\ {\to}\text{E (modus ponens)}$$

To prove $A \to B$: assume $[A]$, derive $B$, then discharge $[A]$.

**Negation and Falsehood:**
$$\frac{\begin{matrix}{[}A{]} \\ \vdots \\ \bot\end{matrix}}{\neg A}\ \neg\text{I} \qquad \frac{\neg A \quad A}{\bot}\ \neg\text{E}$$

**From $\bot$, two options govern which logic we are in:**
$$\frac{\bot}{D}\ \bot\text{J}\quad(\textit{ex falso}) \qquad \frac{\begin{matrix}{[}\neg A{]} \\ \vdots \\ \bot\end{matrix}}{A}\ \bot\text{K}\quad(\textit{reductio})$$

- The system $\{\to\text{I}, \to\text{E}, \wedge\text{I}, \wedge\text{E}, \vee\text{I}, \vee\text{E}, \neg\text{I}, \neg\text{E}, \bot\text{J}\}$ is **intuitionistic** natural deduction (corresponds to $\mathbf{J}_0$).
- Adding $\bot\text{K}$ gives the **classical** natural deduction system (corresponds to $\mathbf{K}_0$).

**Definition 21.** A **deduction** of $A$ is a finite tree of formulas where every leaf is an assumption and every internal node is the conclusion of a correct rule application. A deduction with **no open assumptions** is a **proof** of $A$; $A$ is then a **theorem**.

#### Examples

**Example 22 (Commutativity of $\wedge$).** Deduction of $A \wedge B$ from open assumption $B \wedge A$:
$$\frac{\dfrac{B \wedge A}{A}\ \wedge\text{E} \quad \dfrac{B \wedge A}{B}\ \wedge\text{E}}{A \wedge B}\ \wedge\text{I}$$
Since $B \wedge A$ remains open, this is a deduction but not a proof.

**Example 24 (Intuitionistic contraposition).** Proof of $(A \to B) \to (\neg B \to \neg A)$ using only intuitionistic rules:

$$\dfrac{\dfrac{\dfrac{\dfrac{\dfrac{[A \to B]^1 \quad [A]^3}{B}\ {\to}\text{E} \quad [\neg B]^2}{\bot}\ \neg\text{E}}{\neg A}\ \neg\text{I}^3}{\neg B \to \neg A}\ {\to}\text{I}^2}{(A \to B) \to (\neg B \to \neg A)}\ {\to}\text{I}^1$$

Reading bottom-up: ${\to}\text{I}^1$ assumes $[A \to B]^1$, then ${\to}\text{I}^2$ assumes $[\neg B]^2$, then $\neg\text{I}^3$ assumes $[A]^3$ and derives $\bot$ (from $B$ via ${\to}\text{E}$, and $[\neg B]^2$ via $\neg\text{E}$). All three assumptions are discharged.

### 6.2 Sequent Calculus

The **sequent calculus** (Gentzen, 1935) manipulates **sequents**:
$$A_1, \ldots, A_n \Rightarrow B_1, \ldots, B_m$$
where the left side is the **antecedent** and the right side the **succedent**.

**Definition 26 (Validity).** A sequent $A_1,\ldots,A_n \Rightarrow B_1,\ldots,B_m$ is **valid** if the following is a tautology:
$$\begin{cases} A_1 \wedge \cdots \wedge A_n \to B_1 \vee \cdots \vee B_m & \text{if } n \neq 0, m \neq 0 \\ B_1 \vee \cdots \vee B_m & \text{if } n = 0, m \neq 0 \\ \neg(A_1 \wedge \cdots \wedge A_n) & \text{if } n \neq 0, m = 0 \end{cases}$$

The sequent $A \Rightarrow A$ is always valid and serves as an **axiom**.

#### Structural Rules

$$\frac{\Gamma \Rightarrow \Theta}{A, \Gamma \Rightarrow \Theta}\ \text{WL} \qquad \frac{\Gamma \Rightarrow \Theta}{\Gamma \Rightarrow \Theta, A}\ \text{WR} \quad \text{(Weakening)}$$
$$\frac{A, A, \Gamma \Rightarrow \Theta}{A, \Gamma \Rightarrow \Theta}\ \text{CL} \qquad \frac{\Gamma \Rightarrow \Theta, A, A}{\Gamma \Rightarrow \Theta, A}\ \text{CR} \quad \text{(Contraction)}$$
$$\frac{\Delta, A, B, \Gamma \Rightarrow \Theta}{\Delta, B, A, \Gamma \Rightarrow \Theta}\ \text{IL} \qquad \frac{\Gamma \Rightarrow \Theta, A, B, \Lambda}{\Gamma \Rightarrow \Theta, B, A, \Lambda}\ \text{IR} \quad \text{(Interchange)}$$

Contraction + Interchange together justify treating antecedents and succedents as **sets** rather than lists.

#### Operational Rules

$$\frac{A, \Gamma \Rightarrow \Theta}{A \wedge B, \Gamma \Rightarrow \Theta}\ \wedge\text{L} \qquad \frac{B, \Gamma \Rightarrow \Theta}{A \wedge B, \Gamma \Rightarrow \Theta}\ \wedge\text{L} \qquad \frac{\Gamma \Rightarrow \Theta, A \quad \Gamma \Rightarrow \Theta, B}{\Gamma \Rightarrow \Theta, A \wedge B}\ \wedge\text{R}$$

$$\frac{\Gamma \Rightarrow \Theta, A}{\Gamma \Rightarrow \Theta, A \vee B}\ \vee\text{R} \qquad \frac{\Gamma \Rightarrow \Theta, B}{\Gamma \Rightarrow \Theta, A \vee B}\ \vee\text{R} \qquad \frac{A, \Gamma \Rightarrow \Theta \quad B, \Gamma \Rightarrow \Theta}{A \vee B, \Gamma \Rightarrow \Theta}\ \vee\text{L}$$

$$\frac{\Gamma \Rightarrow \Theta, A \quad B, \Delta \Rightarrow \Lambda}{A \to B, \Gamma, \Delta \Rightarrow \Theta, \Lambda}\ {\to}\text{L} \qquad \frac{A, \Gamma \Rightarrow \Theta, B}{\Gamma \Rightarrow \Theta, A \to B}\ {\to}\text{R}$$

$$\frac{\Gamma \Rightarrow \Theta, A}{\neg A, \Gamma \Rightarrow \Theta}\ \neg\text{L} \qquad \frac{A, \Gamma \Rightarrow \Theta}{\Gamma \Rightarrow \Theta, \neg A}\ \neg\text{R}$$

**Symmetry:** moving a formula across the $\Rightarrow$ arrow negates it, reflecting the duality between antecedent and succedent.

#### The Cut Rule

$$\frac{\Gamma \Rightarrow \Theta, A \quad A, \Delta \Rightarrow \Lambda}{\Gamma, \Delta \Rightarrow \Theta, \Lambda}\ \text{Cut}$$

The formula $A$ is the **cut formula** — it does not appear in the conclusion. Cut formalises the use of a lemma. Gentzen's **cut elimination theorem** states that any sequent provable with cut is also provable without it. A consequence: cut-free proofs only mention subformulas of the conclusion.

**Definition 27.** A **proof** in the sequent calculus is a finite tree of sequents where every leaf is an axiom $A \Rightarrow A$ and every internal node is obtained from its children by an inference rule. A sequent is **provable** if it is the root of such a proof.

**Theorem 29.** For any formula $F$ of propositional logic: $\models F$ if and only if the sequent $\Rightarrow F$ is provable. (The sequent calculus is sound and complete.)

---

## L7: The Compactness Theorem

### 7.1 Statement and Proof

A **partial assignment** $\mathcal{A}: D \to \{0,1\}$ is defined on a domain $D \subseteq X$. It is **good** (for a set $\mathcal{S}$ of formulas) if it satisfies every $F \in \mathcal{S}$ that only mentions variables in $D$.

**Theorem 30 (Compactness Theorem).** A set of formulas $\mathcal{S}$ is satisfiable if and only if every finite subset of $\mathcal{S}$ is satisfiable.

*Proof.* The $\Rightarrow$ direction is obvious. For the converse:

Suppose every finite subset of $\mathcal{S}$ is satisfiable. We construct a sequence of good partial assignments $\mathcal{A}_0, \mathcal{A}_1, \mathcal{A}_2, \ldots$ with $\mathrm{dom}(\mathcal{A}_n) = \{x_1, \ldots, x_n\}$ and $\mathcal{A}_{n+1}$ extending $\mathcal{A}_n$, maintaining the induction hypothesis:

> **(*)** There are infinitely many good partial assignments that extend $\mathcal{A}_n$.

**Base case:** $\mathcal{A}_0$ has empty domain. For every $n$, there is a good assignment with domain $\{x_1,\ldots,x_n\}$ (since $\mathcal{S}$ restricted to those variables, though possibly infinite, contains only finitely many logically inequivalent formulas, hence the finite-satisfiability assumption applies). So infinitely many good partial assignments extend $\mathcal{A}_0$.

**Inductive step:** Given $\mathcal{A}_n$ satisfying (\*), consider the two extensions $\mathcal{B}$ and $\mathcal{B}'$ that set $x_{n+1} = 0$ and $x_{n+1} = 1$. Any good extension of $\mathcal{A}_n$ extends one of them. Since there are infinitely many, at least one of $\mathcal{B}$ or $\mathcal{B}'$ has infinitely many good extensions. Let $\mathcal{A}_{n+1}$ be that one.

Define the total assignment $\mathcal{A}(x_n) := \mathcal{A}_n(x_n)$. For any $F \in \mathcal{S}$ mentioning only $x_1, \ldots, x_n$: $\mathcal{A}_n$ is good so $\mathcal{A}_n \models F$, and since $\mathcal{A}$ extends $\mathcal{A}_n$, also $\mathcal{A} \models F$. Hence $\mathcal{A}$ satisfies all of $\mathcal{S}$. $\square$

**Contrapositive:** $\mathcal{S}$ is unsatisfiable if and only if some finite subset is already unsatisfiable.

### 7.2 Application: Infinite Graph Colouring

**Theorem 31.** Let $G = (V, E)$ be a graph with $V = \{v_i : i \in \mathbb{N}\}$. If every finite subgraph of $G$ is $k$-colourable, then $G$ is $k$-colourable.

*Proof.* Introduce propositional variables $x_{v,i}$ for each vertex $v \in V$ and colour $1 \leq i \leq k$ (meaning "vertex $v$ has colour $i$"). Define formulas:
- $F_v := \bigvee_{i=1}^k x_{v,i}$ (vertex $v$ has some colour)
- $G_v := \bigwedge_{i=1}^k \bigwedge_{j=i+1}^k \neg x_{v,i} \vee \neg x_{v,j}$ (at most one colour)
- $H_{u,v} := \bigwedge_{i=1}^k \neg x_{u,i} \vee \neg x_{v,i}$ (adjacent vertices have different colours)

Let $\mathcal{S} = \{F_v, G_v : v \in V\} \cup \{H_{u,v} : (u,v) \in E\}$.

Every finite subset of $\mathcal{S}$ involves finitely many vertices, forming a finite subgraph which (by assumption) is $k$-colourable, giving a satisfying assignment for that finite subset. By Compactness, $\mathcal{S}$ is satisfiable, giving a $k$-colouring of $G$. $\square$

**Remark:** The Compactness Theorem is non-constructive — it proves existence of a satisfying assignment without exhibiting one. It is equivalent to the statement that the product space $\{0,1\}^\mathbb{N}$ is compact (Tychonoff's theorem for finite discrete spaces).

---

## L8: First-Order Logic — Syntax and Semantics

### 8.1 Syntax

First-order logic (FOL) extends propositional logic with **predicates** (structured atomic formulas) and **quantifiers** ($\forall$, $\exists$).

**Signature:** A **signature** $\sigma$ specifies:
- a set of **constant symbols** (arity 0)
- a set of **function symbols**, each with arity $k > 0$
- a set of **predicate symbols** (also called **relations**), each with arity $k > 0$

We also have a countably infinite set of **variables** $x_0, x_1, x_2, \ldots$

**Definition 32 ($\sigma$-terms, defined inductively):**
- Every variable is a term.
- Every constant symbol is a term.
- If $t_1, \ldots, t_k$ are terms and $f$ is a $k$-ary function symbol, then $f(t_1, \ldots, t_k)$ is a term.

**Formulas (defined inductively):**
- If $t_1, \ldots, t_k$ are terms and $P$ is a $k$-ary predicate symbol, then $P(t_1, \ldots, t_k)$ is a formula (**atomic**).
- If $F$ is a formula, $\neg F$ is a formula.
- If $F, G$ are formulas, $(F \vee G)$ and $(F \wedge G)$ are formulas.
- If $F$ is a formula and $x$ is a variable, then $\exists x\, F$ and $\forall x\, F$ are formulas.

**Quantifier depth:** $\mathrm{qd}(\text{atomic}) = 0$; $\mathrm{qd}(\neg F) = \mathrm{qd}(F)$; $\mathrm{qd}(F \wedge G) = \mathrm{qd}(F \vee G) = \max(\mathrm{qd}(F), \mathrm{qd}(G))$; $\mathrm{qd}(\exists x\, F) = \mathrm{qd}(\forall x\, F) = \mathrm{qd}(F) + 1$.

**Bound and free variables:** In $\exists x\, F$ or $\forall x\, F$, $F$ is the **scope** of the quantifier. An occurrence of variable $x$ is **bound** if it lies within the scope of a quantifier $\exists x$ or $\forall x$; otherwise it is **free**. A formula with no free variables is **closed** (a **sentence**).

**FOL with equality** admits $t_1 = t_2$ as an additional atomic formula for any terms $t_1, t_2$; equality is always interpreted as actual equality.

### 8.2 Semantics

**Definition 36 ($\sigma$-structure).** A $\sigma$-**structure** $\mathcal{A}$ consists of:
- a non-empty set $U_\mathcal{A}$ called the **universe**;
- for each $k$-ary predicate $P$ in $\sigma$: a relation $P_\mathcal{A} \subseteq U_\mathcal{A}^k$;
- for each $k$-ary function symbol $f$ in $\sigma$: a function $f_\mathcal{A}: U_\mathcal{A}^k \to U_\mathcal{A}$;
- for each constant symbol $c$ in $\sigma$: an element $c_\mathcal{A} \in U_\mathcal{A}$;
- for each variable $x$: an element $x_\mathcal{A} \in U_\mathcal{A}$.

The variable interpretation can be overwritten: $\mathcal{A}_{[x \to a]}$ is the same as $\mathcal{A}$ except $x_\mathcal{A} := a$.

**Value of a term:** $\mathcal{A}(c) := c_\mathcal{A}$; $\mathcal{A}(x) := x_\mathcal{A}$; $\mathcal{A}(f(t_1,\ldots,t_k)) := f_\mathcal{A}(\mathcal{A}(t_1),\ldots,\mathcal{A}(t_k))$.

**Satisfaction relation** $\mathcal{A} \models F$:
1. $\mathcal{A} \models P(t_1,\ldots,t_k)$ iff $(\mathcal{A}(t_1),\ldots,\mathcal{A}(t_k)) \in P_\mathcal{A}$
2. $\mathcal{A} \models F \wedge G$ iff $\mathcal{A} \models F$ and $\mathcal{A} \models G$
3. $\mathcal{A} \models F \vee G$ iff $\mathcal{A} \models F$ or $\mathcal{A} \models G$
4. $\mathcal{A} \models \neg F$ iff $\mathcal{A} \not\models F$
5. $\mathcal{A} \models \exists x\, F$ iff there exists $a \in U_\mathcal{A}$ such that $\mathcal{A}_{[x \to a]} \models F$
6. $\mathcal{A} \models \forall x\, F$ iff $\mathcal{A}_{[x \to a]} \models F$ for all $a \in U_\mathcal{A}$
7. (With equality) $\mathcal{A} \models t_1 = t_2$ iff $\mathcal{A}(t_1) = \mathcal{A}(t_2)$

Satisfiability, validity, entailment, and logical equivalence carry over from propositional logic with $\sigma$-structures replacing assignments.

---

## L9: Skolem Form

### 9.1 Relevance and Substitution

**Lemma 39 (Relevance Lemma).** If $\mathcal{A}$ and $\mathcal{A}'$ are $\sigma$-structures with the same universe and same interpretation of all predicate, function, and constant symbols, and they agree on all variables occurring free in $F$, then $\mathcal{A} \models F$ iff $\mathcal{A}' \models F$.

*Corollary:* For closed formulas, the interpretation of variables does not matter.

**Substitution $F[t/x]$** ($t$ substituted for all free occurrences of $x$ in $F$):
$$c[t/x] = c, \quad y[t/x] = y \text{ (}y \neq x\text{)}, \quad x[t/x] = t$$
$$(\neg F)[t/x] = \neg(F[t/x]), \quad (F \wedge G)[t/x] = F[t/x] \wedge G[t/x]$$
$$(Qy\, F)[t/x] = Qy\, (F[t/x]) \text{ if } y \neq x, \quad (Qx\, F)[t/x] = Qx\, F$$

**Warning:** *Variable capture* — if $y$ occurs free in $t$ and becomes bound by a quantifier in $F$, the substitution is invalid. Always rename bound variables to avoid capture.

**Lemma 43 (Translation Lemma).** If $t$ is a term and no variable in $t$ occurs bound in $F$, then $\mathcal{A} \models F[t/x]$ iff $\mathcal{A}_{[x \to \mathcal{A}(t)]} \models F$.

**Proposition 44 (Renaming bound variables).** If $Q \in \{\forall, \exists\}$ and $y$ does not occur in $G$, then $Qx\, G \equiv Qy\, (G[y/x])$.

### 9.2 Quantifier Equivalences

**Proposition 40.** Key equivalences for manipulating quantifiers:

**(A)** $\neg \forall x\, F \equiv \exists x\, \neg F$ and $\neg \exists x\, F \equiv \forall x\, \neg F$

**(B)** If $x$ does not occur free in $G$:
$$(\forall x\, F) \wedge G \equiv \forall x\, (F \wedge G), \quad (\exists x\, F) \wedge G \equiv \exists x\, (F \wedge G)$$
$$(\forall x\, F) \vee G \equiv \forall x\, (F \vee G), \quad (\exists x\, F) \vee G \equiv \exists x\, (F \vee G)$$

**(C)** $(\forall x\, F \wedge \forall x\, G) \equiv \forall x\, (F \wedge G)$ and $(\exists x\, F \vee \exists x\, G) \equiv \exists x\, (F \vee G)$

**(D)** $\forall x\, \forall y\, F \equiv \forall y\, \forall x\, F$ and $\exists x\, \exists y\, F \equiv \exists y\, \exists x\, F$

### 9.3 Prenex Form

**Definition 41.** A formula is in **prenex form** (or prenex normal form) if it has the shape $Q_1 y_1\, Q_2 y_2 \cdots Q_n y_n\, F$ where $Q_i \in \{\exists, \forall\}$ and $F$ contains no quantifiers. $F$ is the **matrix**.

**Theorem 46.** Every formula is logically equivalent to a formula in rectified prenex form (RPF).

A formula is **rectified** if no variable occurs both free and bound, and all quantifiers bind distinct variables. Rectification is achieved by renaming bound variables (Proposition 44), then pulling quantifiers to the front using Proposition 40.

### 9.4 Skolem Form

A formula in RPF is in **Skolem form** if it contains no existential quantifiers (i.e., all quantifiers are $\forall$).

**Skolemisation:** Eliminate existential quantifiers by introducing fresh **Skolem functions**.

**Proposition 47.** Let $F = \forall y_1 \forall y_2 \cdots \forall y_n\, \exists z\, G$ be a rectified formula. Let $f$ be a fresh $n$-ary function symbol. Then $F$ and $F' = \forall y_1 \cdots \forall y_n\, G[f(y_1,\ldots,y_n)/z]$ are **equisatisfiable** (but not necessarily logically equivalent).

*Proof sketch:* If $\mathcal{A} \models F$: for each tuple $a_1, \ldots, a_n \in U_\mathcal{A}$, pick $a$ such that $\mathcal{A}_{[y_1 \to a_1]\cdots[y_n \to a_n][z \to a]} \models G$. Define $f_\mathcal{A}(a_1,\ldots,a_n) := a$. Extend $\mathcal{A}$ to $\mathcal{A}'$ with this interpretation of $f$. Then $\mathcal{A}' \models F'$. $\square$

**Full Skolemisation algorithm:**
1. Apply De Morgan's laws to push negations to atomic level (**negation normal form**).
2. Rename bound variables to make the formula **rectified**.
3. Pull all quantifiers to the front using Proposition 40 (obtain **prenex form**).
4. Repeatedly eliminate the outermost $\exists$ by introducing a fresh Skolem function until no $\exists$ remains.

**Example 48:** Skolemise $\forall x\, \exists y\, \forall z\, \exists w\, (\neg P(a,w) \vee Q(f(x),y))$:
1. Eliminate $\exists y$ with function $g(x)$: $\forall x\, \forall z\, \exists w\, (\neg P(a,w) \vee Q(f(x),g(x)))$.
2. Eliminate $\exists w$ with function $h(x,z)$: $\forall x\, \forall z\, (\neg P(a,h(x,z)) \vee Q(f(x),g(x)))$.

---

## L10: Herbrand's Theorem and Ground Resolution

### 10.1 Herbrand Structures

**Definition 49.** Let $\sigma$ be a signature with at least one constant symbol. A $\sigma$-structure $\mathcal{H}$ is a **Herbrand structure** if:
1. $U_\mathcal{H}$ is the set of all **ground terms** (variable-free terms) over $\sigma$.
2. For every constant $c \in \sigma$: $c_\mathcal{H} = c$.
3. For every $k$-ary function $f \in \sigma$ and ground terms $t_1,\ldots,t_k$: $f_\mathcal{H}(t_1,\ldots,t_k) = f(t_1,\ldots,t_k)$.

A Herbrand structure is determined solely by the interpretation of its predicate symbols.

**Proposition 51.** Let $\mathcal{H}$ be a Herbrand structure and $t$ a ground term. Then $\mathcal{H}(t) = t$.

*Proof.* By structural induction on terms. Base case: $t = c$ (constant), then $\mathcal{H}(c) = c_\mathcal{H} = c$. Inductive step: $t = f(t_1,\ldots,t_k)$, then $\mathcal{H}(f(t_1,\ldots,t_k)) = f_\mathcal{H}(\mathcal{H}(t_1),\ldots,\mathcal{H}(t_k)) = f_\mathcal{H}(t_1,\ldots,t_k) = f(t_1,\ldots,t_k)$. $\square$

**Lemma 52 (Translation Lemma for Herbrand).** $\mathcal{H} \models F[t/x]$ iff $\mathcal{H}_{[x \to t]} \models F$.

*Proof.* By the standard Translation Lemma and Proposition 51: $\mathcal{H} \models F[t/x]$ iff $\mathcal{H}_{[x \to \mathcal{H}(t)]} \models F$ iff $\mathcal{H}_{[x \to t]} \models F$. $\square$

### 10.2 Herbrand's Theorem

**Theorem 53 (Herbrand's Theorem).** Let $F = \forall x_1 \cdots \forall x_n\, F^*$ be a closed formula in Skolem form. Then $F$ is satisfiable if and only if it has a Herbrand model.

*Proof.*
($\Leftarrow$) Obvious: a Herbrand model is a structure.

($\Rightarrow$) Suppose $\mathcal{A} \models F$. Construct a Herbrand structure $\mathcal{H}$ by setting $(t_1,\ldots,t_k) \in P_\mathcal{H}$ iff $\mathcal{A} \models P(t_1,\ldots,t_k)$ for each predicate $P$ and ground terms $t_1,\ldots,t_k$.

Claim: for all subformulas $G = \forall y_1 \cdots \forall y_k\, G^*$ of $F$ in Skolem form, if $\mathcal{A} \models G$ then $\mathcal{H} \models G$.

Proof by induction on $k$:
- Base case ($k=0$): $G$ is a Boolean combination of atoms $P(t_1,\ldots,t_m)$ for ground terms $t_i$. By construction, $\mathcal{A}$ and $\mathcal{H}$ agree on each such atom, so $\mathcal{A} \models G$ iff $\mathcal{H} \models G$.
- Inductive step: $G = \forall y\, G'$. $\mathcal{A} \models G$ iff $\mathcal{A} \models G'[t/y]$ for all ground $t$ (by the Translation Lemma). Each $G'[t/y]$ is closed, so by inductive hypothesis $\mathcal{H} \models G'[t/y]$ for all ground $t$, i.e., $\mathcal{H} \models G$. $\square$

### 10.3 Herbrand Expansion and Ground Resolution

**Herbrand expansion:** For $F = \forall x_1 \cdots \forall x_n\, F^*$ in Skolem form:
$$E(F) := \{F^*[t_1/x_1]\cdots[t_n/x_n] : t_1,\ldots,t_n \text{ are ground } \sigma\text{-terms}\}$$

$E(F)$ is a (potentially infinite) set of quantifier-free formulas.

**Theorem 55.** A closed formula $F$ in Skolem form is satisfiable iff $E(F)$ is satisfiable when considered as a set of propositional formulas.

*Proof.* By Herbrand's Theorem, $F$ is satisfiable iff it has a Herbrand model $\mathcal{H}$. One can verify that $\mathcal{H} \models F$ iff $\mathcal{H} \models F^*[t_1/x_1]\cdots[t_n/x_n]$ for all ground $t_i$ (by the Herbrand Translation Lemma), i.e., iff $\mathcal{H} \models E(F)$ as propositional formulas. $\square$

**Theorem 56 (Ground Resolution).** A closed formula $F$ in Skolem form is unsatisfiable iff there is a propositional resolution proof of $\square$ from $E(F)$.

*Proof.* By Theorem 55, $F$ unsatisfiable iff $E(F)$ propositionally unsatisfiable. By Compactness, iff some finite subset $S \subseteq E(F)$ is unsatisfiable. By Resolution Completeness (Theorem 18), iff $\square$ is derivable from $S$ (hence from $E(F)$) by resolution. $\square$

**Corollary:** To check validity of a FOL formula $F$:
1. Negate: $G = \neg F$.
2. Skolemise $G$ to obtain $H$ in Skolem form.
3. Compute $E(H)$ and systematically generate resolvents.
4. If $\square$ is derived, $H$ is unsatisfiable, so $F$ is valid.
5. If $F$ is satisfiable, the process may run forever (FOL validity is undecidable).

---

## L11: Natural Deduction and Sequent Calculus for FOL

### 11.1 Natural Deduction for First-Order Logic

We extend natural deduction with rules for the universal and existential quantifiers. Let $c$ be a variable and $t$ be a term.

**Universal quantification:**
$$\frac{A(c)}{\forall x\, A[x/c]}\ \forall\text{I} \qquad \frac{\forall x\, A(x)}{A[t/x]}\ \forall\text{E}$$

- $\forall\text{E}$: from $\forall x\, A(x)$, instantiate at any term $t$.
- $\forall\text{I}$: from $A(c)$ where **$c$ does not occur in any undischarged assumption** on which $A(c)$ depends, conclude $\forall x\, A[x/c]$.

The side-condition on $\forall\text{I}$ is essential. Without it, we could "prove" $A(c) \to \forall x\, A(x)$ (a property holding for a single element implies it holds universally), which is unsound.

**Existential quantification:**
$$\frac{A[t/c]}{\exists x\, A[x/c]}\ \exists\text{I} \qquad \frac{\exists x\, A(x) \quad \begin{matrix}{[}A(c){]} \\ \vdots \\ C\end{matrix}}{C}\ \exists\text{E}$$

- $\exists\text{I}$: exhibit a specific term $t$ satisfying $A$, then conclude $\exists x\, A[x/c]$.
- $\exists\text{E}$: "there exists some $c$ satisfying $A(c)$; call it $c$; if we can derive $C$ from $A(c)$, then $C$ holds." Two side-conditions:
  - $c$ must not occur in $C$ (conclusion does not depend on the specific witness).
  - All assumptions of the form $A(c)$ must be discharged at the $\exists\text{E}$ step.

**Example 58.** Proof of $(\forall x\,(P(x) \to Q(x)) \wedge \exists x\, P(x)) \to \exists x\, Q(x)$: assume the conjunction, extract $\exists x\, P(x)$ (by $\wedge\text{E}$) and $\forall x\,(P(x) \to Q(x))$ (by $\wedge\text{E}$). Apply $\exists\text{E}$ to assume $P(c)$ for fresh $c$; apply $\forall\text{E}$ to obtain $P(c) \to Q(c)$; use $\to\text{E}$ to get $Q(c)$; apply $\exists\text{I}$ to get $\exists x\, Q(x)$.

### 11.2 Sequent Calculus for First-Order Logic

Extend the propositional sequent calculus with operational rules for quantifiers:

**Universal quantification:**
$$\frac{A(t), \Gamma \Rightarrow \Theta}{\forall x\, A(x), \Gamma \Rightarrow \Theta}\ \forall\text{L} \qquad !\ \frac{\Gamma \Rightarrow \Theta, A(a)}{\Gamma \Rightarrow \Theta, \forall x\, A(x)}\ \forall\text{R}$$

**Existential quantification:**
$$!\ \frac{A(a), \Gamma \Rightarrow \Theta}{\exists x\, A(x), \Gamma \Rightarrow \Theta}\ \exists\text{L} \qquad \frac{\Gamma \Rightarrow \Theta, A(t)}{\Gamma \Rightarrow \Theta, \exists x\, A(x)}\ \exists\text{R}$$

**Rules marked with ! have freshness conditions:**
- $\forall\text{R}$: $a$ must not appear in $\Gamma$, $\Theta$, or $\forall x\, A(x)$.
- $\exists\text{L}$: $a$ must not appear in $\Gamma$, $\Theta$, or $\exists x\, A(x)$.

The **duality** between $\forall$ and $\exists$:
- Both $\forall\text{R}$ and $\exists\text{L}$ require **fresh variables** (strong, universal-like rules).
- Both $\forall\text{L}$ and $\exists\text{R}$ allow **arbitrary terms** (weak, existential-like rules).

Without freshness, one could falsely prove $\Rightarrow \exists x\, A(x) \to \forall x\, A(x)$ (swapping $\forall\text{R}$ and $\exists\text{L}$ steps violates side-conditions).

**Example 59.** Sequent calculus proof of $\Rightarrow \forall x\, P(x) \to \exists x\, P(x)$:

$$\frac{\dfrac{P(a) \Rightarrow P(a)}{P(a) \Rightarrow \exists x\, P(x)}\ \exists\text{R}}{\dfrac{\forall x\, P(x) \Rightarrow \exists x\, P(x)}{\Rightarrow \forall x\, P(x) \to \exists x\, P(x)}}\ {\forall\text{L},\ {\to}\text{R}}$$

Here $a$ is used as a term in $\forall\text{L}$ (any term allowed) and as a witness in $\exists\text{R}$ (any term suffices).

---

## Summary of Key Results

| Result | Statement |
|--------|-----------|
| **Theorem 8** | $\models F$ iff $\vdash_{\mathbf{K}_0} F$ (classical logic is sound and complete) |
| **Theorem 11** | Equational reasoning is sound and complete for propositional logic |
| **Theorem 17** | Resolution soundness: refutation of $F$ implies $F$ unsatisfiable |
| **Theorem 18** | Resolution completeness: $F$ unsatisfiable implies refutation exists |
| **Theorem 29** | Sequent calculus: $\models F$ iff $\Rightarrow F$ is provable |
| **Theorem 30** | **Compactness**: $\mathcal{S}$ satisfiable iff every finite subset is satisfiable |
| **Theorem 53** | **Herbrand**: $F$ (Skolem form) satisfiable iff it has a Herbrand model |
| **Theorem 56** | **Ground Resolution**: $F$ (Skolem form) unsatisfiable iff $\square$ derivable from $E(F)$ |

---

## The Hierarchy of Proof Systems

| System | Logic | Key feature |
|--------|-------|-------------|
| $\mathbf{M}_0$ | Minimal | No ex falso, no excluded middle |
| $\mathbf{J}_0 = \mathbf{M}_0 + \text{PL11}$ | Intuitionistic | ex falso quodlibet, no excluded middle |
| $\mathbf{K}_0 = \mathbf{J}_0 + \text{PL12}$ | Classical | Double negation elimination ($\neg\neg A \to A$) |
| Equational reasoning | Classical | Transform $F$ to $G$ via Boolean algebra axioms |
| Resolution | Classical | Single inference rule on CNF; used in SAT solvers |
| Natural deduction | Intuitionistic or classical | Mirrors mathematical proof structure |
| Sequent calculus | Classical | Symmetric, bidirectional; cut elimination holds |

---

*Notes compiled from IPS lecture notes by Christoph Haase, Trinity Term 2026, University of Oxford.*
