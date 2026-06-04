# Prelims Probability — Exam Summary
**Oxford MT 2025 | Matthias Winkel**

---

## Table of Contents
1. [Chapter 1: Events and Probability](#chapter-1-events-and-probability)
2. [Chapter 2: Discrete Random Variables](#chapter-2-discrete-random-variables)
3. [Chapter 3: Difference Equations and Random Walks](#chapter-3-difference-equations-and-random-walks)
4. [Chapter 4: Probability Generating Functions](#chapter-4-probability-generating-functions)
5. [Chapter 5: Continuous Random Variables](#chapter-5-continuous-random-variables)
6. [Chapter 6: Random Samples and the Weak Law of Large Numbers](#chapter-6-random-samples-and-the-weak-law-of-large-numbers)
7. [Quick Reference: Distributions](#quick-reference-distributions)

---

## Chapter 1: Events and Probability

### 1.1 Key Definitions

- **Sample space** Ω: set of all possible outcomes.
- **Event**: a subset A ⊆ Ω.
- **Complement**: A^c = Ω \ A. Set operations: A ∪ B, A ∩ B, A \ B.
- **Disjoint**: A ∩ B = ∅.

### 1.2 Counting

**Arrangements of n distinguishable objects**: n! orderings.

**Stirling's formula**: n! ≈ √(2π) · n^(n+½) · e^(−n)

**Multinomial coefficient** — arrangements of n objects where α_i appears m_i times (m₁+…+m_k = n):
$$\frac{n!}{m_1! m_2! \cdots m_k!}$$

**Binomial coefficient**: C(n,m) = n! / (m!(n−m)!)

**Binomial theorem**: (x+y)^n = Σ C(n,k) x^k y^(n−k)

**Stars and bars**: number of non-negative integer solutions to x₁+…+x_m = n is C(n+m−1, n).

#### Lemma 1.5 (Vandermonde's Identity) ★
$$\binom{m+n}{k} = \sum_{j=0}^{k} \binom{m}{j}\binom{n}{k-j}$$
**Proof**: Count committees of k from m men + n women; choose j men in C(m,j) ways then k−j women in C(n,k−j) ways. Sum over j. □

### 1.3 The Axiomatic Approach

**Probability space**: triple (Ω, F, P) where:
- **F** (σ-algebra): Ω ∈ F; closed under complements (F₂); closed under countable unions (F₃).
- **P** (probability measure): P(A) ≥ 0 (P₁); P(Ω) = 1 (P₂); countable additivity for disjoint events (P₃).

#### Theorem 1.8 ★ (Basic consequences of axioms)
1. P(A^c) = 1 − P(A)
2. A ⊆ B ⟹ P(A) ≤ P(B)

**Proof**:
1. A ∪ A^c = Ω and A ∩ A^c = ∅, so by P₃: P(Ω) = P(A) + P(A^c). By P₂: P(Ω)=1, giving P(A^c) = 1 − P(A). □
2. B = A ∪ (B ∩ A^c). Since B ∩ A^c ⊆ A^c it is disjoint from A. By P₃: P(B) = P(A) + P(B ∩ A^c) ≥ P(A). □

### 1.4 Conditional Probability

**Definition**: If P(B) > 0, then P(A|B) = P(A ∩ B) / P(B).

#### Theorem 1.11 ★ (Conditional probability is a probability measure)
Q(A) = P(A|B) defines a valid probability space on (Ω, F, Q).

**Proof**: Check axioms P₁–P₃:
- P₁: Q(A) = P(A ∩ B)/P(B) ≥ 0. ✓
- P₂: Q(Ω) = P(Ω ∩ B)/P(B) = P(B)/P(B) = 1. ✓
- P₃: For disjoint A_i, the sets A_i ∩ B are also disjoint, so Q(∪A_i) = P((∪A_i) ∩ B)/P(B) = Σ P(A_i ∩ B)/P(B) = Σ Q(A_i). □

**Multiplication rule**: P(A ∩ B) = P(A|B) P(B)

**Chain rule**: P(A₁ ∩ … ∩ A_n) = P(A₁) P(A₂|A₁) P(A₃|A₁∩A₂) … P(A_n|A₁∩…∩A_{n−1})

### 1.5 Law of Total Probability and Bayes' Theorem

**Partition**: {B₁, B₂, …} is a partition of Ω if Ω = ∪B_i and B_i ∩ B_j = ∅ for i ≠ j.

#### Theorem 1.16 ★ (Law of Total Probability)
If {B_i} is a partition with P(B_i) > 0 for all i, then for any A:
$$\mathbb{P}(A) = \sum_{i \geq 1} \mathbb{P}(A|B_i)\mathbb{P}(B_i)$$

**Proof**: P(A) = P(A ∩ Ω) = P(A ∩ ∪B_i) = P(∪(A ∩ B_i)) = Σ P(A ∩ B_i) (disjoint) = Σ P(A|B_i)P(B_i). □

#### Theorem 1.18 ★ (Bayes' Theorem)
With the same partition:
$$\mathbb{P}(B_k | A) = \frac{\mathbb{P}(A|B_k)\mathbb{P}(B_k)}{\sum_{i \geq 1} \mathbb{P}(A|B_i)\mathbb{P}(B_i)}$$

**Proof**: P(B_k|A) = P(B_k ∩ A)/P(A) = P(A|B_k)P(B_k)/P(A). Substitute the law of total probability for P(A). □

**Odds form of Bayes**: P(B|A)/P(B^c|A) = [P(A|B)/P(A|B^c)] · [P(B)/P(B^c)]

### 1.6 Independence

**Definition 1.22**:
1. A and B are **independent** if P(A ∩ B) = P(A)P(B).
2. A family {A_i} is **independent** if for all finite J: P(∩_{i∈J} A_i) = Π_{i∈J} P(A_i).
3. **Pairwise independent** ≠ independent (warning!).

#### Theorem 1.24 ★
If A and B are independent, then: (a) A and B^c are independent; (b) A^c and B^c are independent.

**Proof (a)**: A = (A∩B) ∪ (A∩B^c) with disjoint pieces, so P(A∩B^c) = P(A) − P(A∩B) = P(A) − P(A)P(B) = P(A)(1−P(B)) = P(A)P(B^c). □

### 1.7 Useful Calculation Rules

**AND**: P(A∩B) = P(A|B)P(B); generalise via chain rule.

**OR (disjoint)**: P(A₁∪…∪A_n) = P(A₁)+…+P(A_n).

**OR (complement trick)**: P(A₁∪…∪A_n) = 1 − P(A₁^c ∩ … ∩ A_n^c).

**Inclusion-exclusion**:
$$\mathbb{P}(A_1 \cup \cdots \cup A_n) = \sum_i \mathbb{P}(A_i) - \sum_{i<j}\mathbb{P}(A_i\cap A_j) + \cdots + (-1)^{n+1}\mathbb{P}(A_1\cap\cdots\cap A_n)$$

---

## Chapter 2: Discrete Random Variables

### 2.1 Classical Distributions

| Distribution | PMF | Mean | Variance |
|---|---|---|---|
| **Bernoulli** Ber(p) | P(X=1)=p, P(X=0)=1−p | p | p(1−p) |
| **Binomial** Bin(n,p) | C(n,k) p^k (1−p)^(n−k), k=0,…,n | np | np(1−p) |
| **Geometric** Geom(p) | (1−p)^(k−1) p, k=1,2,… | 1/p | (1−p)/p² |
| **Poisson** Po(λ) | λ^k e^(−λ)/k!, k=0,1,… | λ | λ |

**Note on Geometric**: alternative definition counts failures before first success: P(Y=k) = p(1−p)^k, k=0,1,… (then Y = X−1).

### 2.2 Expectation

**Definition**: E[X] = Σ_{x ∈ ImX} x · P(X=x), provided Σ|x|P(X=x) < ∞.

#### Theorem 2.8 ★ (Law of the unconscious statistician)
For h: ℝ → ℝ: **E[h(X)] = Σ_{x ∈ ImX} h(x) P(X=x)**

**Proof**: Let A = {y : y=h(x) for some x ∈ ImX}. Then:
Σ_x h(x)P(X=x) = Σ_y Σ_{x: h(x)=y} h(x)P(X=x) = Σ_y y · Σ_{x: h(x)=y} P(X=x) = Σ_y y P(h(X)=y) = E[h(X)]. □

**Proof that E[Poisson(λ)] = λ**:
E[X] = Σ_{k=0}^∞ k · λ^k e^{−λ}/k! = e^{−λ} Σ_{k=1}^∞ λ^k/(k−1)! = λ e^{−λ} Σ_{j=0}^∞ λ^j/j! = λ e^{−λ} e^λ = λ. □

#### Theorem 2.10 (Properties of Expectation)
(a) X ≥ 0 ⟹ E[X] ≥ 0. &nbsp; (b) E[aX + b] = aE[X] + b.

**Variance**: var(X) = E[(X − E[X])²] = E[X²] − (E[X])²

**Theorem 2.12**: var(aX + b) = a²var(X).

### 2.3 Conditional Distributions

**Conditional PMF** of X given B: P(X=x|B) = P({X=x} ∩ B)/P(B).

**Conditional expectation**: E[X|B] = Σ_x x P(X=x|B).

#### Theorem 2.14 ★ (Partition theorem for expectations)
If {B_i} is a partition with P(B_i) > 0:
$$\mathbb{E}[X] = \sum_{i \geq 1} \mathbb{E}[X|B_i]\mathbb{P}(B_i)$$

**Proof**: E[X] = Σ_x x P(X=x) = Σ_x x Σ_i P(X=x|B_i)P(B_i) = Σ_i P(B_i) Σ_x x P(X=x|B_i) = Σ_i E[X|B_i]P(B_i). □

### 2.4 Joint Distributions

**Joint PMF**: p_{X,Y}(x,y) = P(X=x, Y=y).

**Marginals**: p_X(x) = Σ_y p_{X,Y}(x,y) and p_Y(y) = Σ_x p_{X,Y}(x,y).

**Conditional PMF of Y given X=x**: p_{Y|X=x}(y) = p_{X,Y}(x,y) / p_X(x).

**Independence of X, Y**: P(X=x, Y=y) = P(X=x)P(Y=y) for all x, y.

#### Theorem 2.21 ★ (Linearity of expectation)
E[aX + bY] = aE[X] + bE[Y] (no independence needed!)

**Proof**: E[aX+bY] = Σ_x Σ_y (ax+by) p_{X,Y}(x,y) = a Σ_x x Σ_y p_{X,Y}(x,y) + b Σ_y y Σ_x p_{X,Y}(x,y) = aE[X] + bE[Y]. □

#### Theorem 2.23 ★ (Independent ⟹ E[XY] = E[X]E[Y])
**Proof**: E[XY] = Σ_x Σ_y xy P(X=x, Y=y) = Σ_x Σ_y xy P(X=x)P(Y=y) = (Σ_x x P(X=x))(Σ_y y P(Y=y)) = E[X]E[Y]. □

**Covariance**: cov(X,Y) = E[(X−E[X])(Y−E[Y])] = E[XY] − E[X]E[Y].

Note cov(X,X) = var(X).

**Key identity**: var(X+Y) = var(X) + var(Y) + 2cov(X,Y).

> **WARNING**: cov(X,Y) = 0 does NOT imply X and Y are independent.

**i.i.d.**: independent and identically distributed random variables.

---

## Chapter 3: Difference Equations and Random Walks

### 3.1 Difference Equations

**k-th order linear recurrence**: Σ_{j=0}^k a_j u_{n+j} = f(n), with a₀ ≠ 0, a_k ≠ 0.

#### Theorem 3.3 (General solution structure)
General solution = particular solution + general solution to homogeneous equation (Σ a_j w_{n+j} = 0).

### 3.2 First Order: u_{n+1} = au_n + b

- **Homogeneous solution**: w_n = A·a^n.
- **Particular solution**: v_n = b/(1−a) if a ≠ 1; try v_n = Cn if a = 1.
- **General solution (a ≠ 1)**: u_n = Aa^n + b/(1−a).

### 3.3 Second Order: u_{n+1} + au_n + bu_{n−1} = f(n)

**Auxiliary equation**: λ² + aλ + b = 0.

| Roots | Homogeneous solution |
|---|---|
| Distinct roots λ₁ ≠ λ₂ | w_n = A₁λ₁^n + A₂λ₂^n |
| Repeated root λ₁ = λ₂ = λ | w_n = (A + Bn)λ^n |
| Complex roots re^{±iθ} | w_n = r^n(A cos nθ + B sin nθ) |

**Particular solutions**: guess same form as f(n); if that solves the homogeneous equation, multiply by n (or n²).

#### Example: Fibonacci numbers
f_{n+2} = f_{n+1} + f_n, f₀ = f₁ = 1. Auxiliary: λ² − λ − 1 = 0, roots λ = (1±√5)/2.
$$f_n = \frac{1}{\sqrt{5}}\left(\frac{1+\sqrt{5}}{2}\right)^{n+1} - \frac{1}{\sqrt{5}}\left(\frac{1-\sqrt{5}}{2}\right)^{n+1}$$

### 3.4 Random Walks (Gambler's Ruin)

**Setup**: Gambler wins £1 with prob p, loses £1 with prob q = 1−p. Starts at £n, stops at 0 or M.

Let u_n = P(bankruptcy). Recurrence: pu_{n+1} − u_n + qu_{n−1} = 0, with u₀=1, u_M=0.

Auxiliary: pλ² − λ + q = 0 ⟹ (pλ−q)(λ−1) = 0, so λ = q/p or λ = 1.

**Solution (p ≠ 1/2)**:
$$u_n = \frac{(q/p)^n - (q/p)^M}{1 - (q/p)^M}$$

**Solution (p = 1/2)**: u_n = 1 − n/M.

**Expected duration** (p ≠ q): e_n = M/(p−q) · (1−(q/p)^n)/(1−(q/p)^M) − n/(p−q).

**Expected duration (p = q = 1/2)**: e_n = n(M−n).

#### Theorem 3.15 (Random walk on ℤ, non-examinable)
Walk started at n > 0 hits 0 with probability:
- (q/p)^n if p > q
- 1 if p ≤ q

---

## Chapter 4: Probability Generating Functions

### 4.1 Definition and Properties

For non-negative integer-valued X:
$$G_X(s) = \mathbb{E}[s^X] = \sum_{k=0}^{\infty} s^k \mathbb{P}(X=k)$$

Defined at least for |s| ≤ 1; G_X(1) = 1.

#### Theorem 4.2 ★ (Uniqueness theorem)
The distribution of X is uniquely determined by G_X.

**Proof**: d^k/ds^k G_X(s)|_{s=0} = k! · P(X=k), so we can recover all probabilities. □

#### PGFs of Common Distributions

| Distribution | PGF |
|---|---|
| Bernoulli Ber(p) | G_X(s) = q + ps |
| Binomial Bin(n,p) | G_X(s) = (q + ps)^n |
| Poisson Po(λ) | G_X(s) = e^{λ(s−1)} |
| Geometric Geom(p) | G_X(s) = ps/(1−(1−p)s) |

#### Moments via PGF
- **E[X]** = G'_X(1)
- **E[X(X−1)]** = G''_X(1) ⟹ **var(X)** = G''_X(1) + G'_X(1) − (G'_X(1))²
- In general: d^k/ds^k G_X(s)|_{s=1} = E[X(X−1)…(X−k+1)] (factorial moments).

### 4.2 Sum of Independent Variables

#### Theorem 4.3 ★
If X and Y are independent: **G_{X+Y}(s) = G_X(s) · G_Y(s)**

**Proof**: G_{X+Y}(s) = E[s^{X+Y}] = E[s^X · s^Y] = E[s^X]E[s^Y] (by independence) = G_X(s)G_Y(s). □

#### Theorem 4.4 ★ (Sum of Bernoullis = Binomial)
If X₁,…,X_n i.i.d. Ber(p), then X₁+…+X_n ~ Bin(n,p).

**Proof**: G_Y(s) = E[s^{X₁+…+X_n}] = Π E[s^{X_i}] = (q+ps)^n = PGF of Bin(n,p). By uniqueness, Y ~ Bin(n,p). □

#### Theorem 4.5 ★ (Sum of Poissons = Poisson)
If X_i ~ Po(λ_i) independent, then Σ X_i ~ Po(Σ λ_i).

**Proof**: G_{ΣX_i}(s) = Π e^{λ_i(s−1)} = exp((s−1)Σλ_i) = PGF of Po(Σλ_i). □

### 4.3 Random Sums

#### Theorem 4.8 ★ (PGF of random sum)
Let X₁, X₂,… be i.i.d. with PGF G_X(s), and N independent of {X_i} with PGF G_N(s). Then:
$$G_{\sum_{i=1}^N X_i}(s) = G_N(G_X(s))$$

**Proof**: Condition on N:
E[s^{X_1+…+X_N}] = Σ_{n=0}^∞ E[s^{X_1+…+X_n}|N=n] P(N=n) = Σ_{n=0}^∞ (G_X(s))^n P(N=n) = G_N(G_X(s)). □

#### Corollary 4.9
If X_i i.i.d. Ber(p) and N ~ Po(λ), then Σ_{i=1}^N X_i ~ Po(λp).

### 4.4 Branching Processes

**Setup**: Population where each individual independently produces offspring with distribution p(i), i ≥ 0 (offspring distribution). G(s) = Σ p(i)sⁱ. X_n = population size in generation n.

#### Theorem 4.11 ★
G_{X_n}(s) = G(G(…G(s)…)) = G composed n+1 times = G(G_{X_{n−1}}(s)).

**Proof**: X_{n+1} = Σ_{i=1}^{X_n} C_i^{(n)} where C_i^{(n)} i.i.d. with PGF G. By Theorem 4.8: G_{X_{n+1}}(s) = G_{X_n}(G(s)). By induction the result follows. □

#### Corollary 4.12
If μ = E[offspring] = G'(1), then **E[X_n] = μ^n**.

**Proof**: E[X_n] = G_n'(1). By chain rule: G_n'(s) = G_{n-1}'(s) · G'(G_{n-1}(s)). At s=1: E[X_n] = E[X_{n−1}] · G'(1) = E[X_{n−1}] · μ. By induction, E[X_n] = μ^n. □

**Extinction probability**: q = P(population eventually dies out).

#### Theorem 4.14 ★ (Extinction probability is smallest fixed point)
q is the **smallest non-negative solution** of x = G(x).

**Proof**: q = lim_{n→∞} G_n(0) (by increasing sequence of events). If r ≥ 0 also satisfies G(r) = r, then G_0(0) = 0 ≤ r, and if G_{n-1}(0) ≤ r then G_n(0) = G(G_{n-1}(0)) ≤ G(r) = r (G is non-decreasing). So G_n(0) ≤ r for all n, hence q ≤ r. □

#### Theorem 4.15 ★ (Extinction criterion)
Assume p(1) ≠ 1. Then:
- **μ ≤ 1** ⟹ q = 1 (certain extinction)
- **μ > 1** ⟹ q < 1 (positive survival probability)

**Proof sketch**: G is convex (G'' ≥ 0), G(0) = p₀ ≥ 0, G(1) = 1, G'(1) = μ.
- μ > 1: slope at 1 is > 1, so G(x) crosses the line y = x at some x* ∈ (0,1). This x* is the smallest fixed point, so q = x* < 1.
- μ ≤ 1: slope at 1 is ≤ 1 and G is convex, so G(x) ≥ x for all x ∈ [0,1). Only fixed point in [0,1] is 1. So q = 1. □

---

## Chapter 5: Continuous Random Variables

### 5.1 CDF and PDF

**CDF**: F_X(x) = P(X ≤ x). Properties:
1. F_X is non-decreasing.
2. P(a < X ≤ b) = F_X(b) − F_X(a).
3. F_X(x) → 0 as x → −∞; F_X(x) → 1 as x → ∞.

**Continuous RV**: X is continuous if F_X(x) = ∫_{-∞}^x f_X(u) du for some **pdf** f_X with:
- f_X(u) ≥ 0 for all u
- ∫_{-∞}^∞ f_X(u) du = 1

Note: dF_X(x)/dx = f_X(x) wherever f_X is continuous.

> **WARNING**: f_X(x) is NOT a probability (it can exceed 1).

#### Theorem 5.12
For a continuous RV: P(X = x) = 0 for all x ∈ ℝ, and P(a ≤ X ≤ b) = ∫_a^b f_X(x) dx.

### 5.2 Classical Continuous Distributions

| Distribution | PDF | Mean | Variance |
|---|---|---|---|
| **Uniform** U[a,b] | 1/(b−a) for a≤x≤b | (a+b)/2 | (b−a)²/12 |
| **Exponential** Exp(λ) | λe^{−λx}, x≥0 | 1/λ | 1/λ² |
| **Gamma** Γ(α,λ) | λ^α x^{α−1} e^{−λx}/Γ(α), x≥0 | α/λ | α/λ² |
| **Normal** N(μ,σ²) | (1/√(2πσ²)) exp(−(x−μ)²/(2σ²)) | μ | σ² |

**Gamma function**: Γ(α) = ∫_0^∞ u^{α−1} e^{-u} du; for n ∈ ℕ: Γ(n) = (n−1)!

**Note**: Exp(λ) = Gamma(1, λ). The parameter λ of Exp is the **reciprocal of the mean**.

#### Example 5.15 ★ (Normal pdf integrates to 1 — key proof technique)
Show ∫_{-∞}^∞ (1/√(2π)) e^{−x²/2} dx = 1.

**Proof**: Let I = ∫_{-∞}^∞ (1/√(2π)) e^{−x²/2} dx. Then:
I² = (1/2π) ∫∫ e^{−(x²+y²)/2} dx dy.
Convert to polar: x = r cosθ, y = r sinθ, Jacobian = r:
I² = (1/2π) ∫_0^{2π} ∫_0^∞ r e^{−r²/2} dr dθ = (1/2π) · 2π · [−e^{−r²/2}]_0^∞ = 1.
Since I ≥ 0, we have I = 1. □

**Standard normal**: Z ~ N(0,1). For X ~ N(μ,σ²): X has same distribution as μ + σZ, and F_X(x) = Φ((x−μ)/σ) where Φ is the standard normal CDF.

**Proof that var(N(0,1)) = 1**:
var(Z) = E[Z²] = ∫_{-∞}^∞ x² (1/√(2π)) e^{-x²/2} dx.
Integrate by parts (u = x, dv = x e^{-x²/2}/√(2π) dx):
= [-x e^{-x²/2}/√(2π)]_{-∞}^∞ + ∫_{-∞}^∞ e^{-x²/2}/√(2π) dx = 0 + 1 = 1. □

### 5.3 Expectation

$$\mathbb{E}[X] = \int_{-\infty}^{\infty} x f_X(x) dx, \quad \mathbb{E}[h(X)] = \int_{-\infty}^{\infty} h(x) f_X(x) dx$$

**Variance**: var(X) = E[X²] − (E[X])²

#### Theorem 5.18
E[aX + b] = aE[X] + b and var(aX + b) = a²var(X).

> **WARNING**: In general, E[1/X] ≠ 1/E[X].

### 5.4 Functions of Continuous RVs

#### Theorem 5.24 ★ (Change of variables formula)
If h: ℝ → ℝ is strictly increasing and differentiable, and Y = h(X), then:
$$f_Y(y) = f_X(h^{-1}(y)) \cdot \left|\frac{d}{dy} h^{-1}(y)\right|$$

**Proof**: Since h is strictly increasing: F_Y(y) = P(h(X) ≤ y) = P(X ≤ h^{-1}(y)) = F_X(h^{-1}(y)). Differentiate using the chain rule. □

**For non-monotone h**: work via the CDF directly on a case-by-case basis.

### 5.5 Joint Distributions

**Joint density**: f_{X,Y}(x,y) ≥ 0 and ∫∫ f_{X,Y}(x,y) dx dy = 1.

F_{X,Y}(x,y) = ∫_{-∞}^y ∫_{-∞}^x f_{X,Y}(u,v) du dv.

**Marginal densities**:
- f_X(x) = ∫_{-∞}^∞ f_{X,Y}(x,y) dy
- f_Y(y) = ∫_{-∞}^∞ f_{X,Y}(x,y) dx

#### Theorem 5.27 ★
For jointly continuous X, Y:
$$\mathbb{P}(a < X \leq b,\; c < Y \leq d) = \int_c^d \int_a^b f_{X,Y}(x,y)\, dx\, dy$$

**Independence**: X and Y jointly continuous are independent iff f_{X,Y}(x,y) = f_X(x) f_Y(y) for all x, y.

**Covariance** (same formula as discrete):
$$\text{cov}(X,Y) = \mathbb{E}[XY] - \mathbb{E}[X]\mathbb{E}[Y]$$
$$\text{var}(X+Y) = \text{var}(X) + \text{var}(Y) + 2\text{cov}(X,Y)$$

#### Example: Standard bivariate normal
Joint density for −1 < ρ < 1:
$$f_{X,Y}(x,y) = \frac{1}{2\pi\sqrt{1-\rho^2}} \exp\!\left(-\frac{x^2 - 2\rho xy + y^2}{2(1-\rho^2)}\right)$$
Marginals: X, Y ~ N(0,1). Covariance: cov(X,Y) = ρ.

**Special property**: For jointly normal X, Y: independent ⟺ cov(X,Y) = 0.

---

## Chapter 6: Random Samples and the Weak Law of Large Numbers

### 6.1 Sample Mean

**Definition**: Random sample of size n = i.i.d. X₁,…,X_n. Sample mean: X̄_n = (1/n) Σ X_i.

#### Theorem 6.3 ★
If X_i i.i.d. with mean μ and variance σ²: **E[X̄_n] = μ** and **var(X̄_n) = σ²/n**.

**Proof**: E[X̄_n] = (1/n) Σ E[X_i] = μ. var(X̄_n) = (1/n²) var(Σ X_i) = (1/n²) · nσ² = σ²/n (using independence → zero covariances). □

### 6.2 Inequalities

#### Theorem 6.6 ★ (Markov's Inequality)
If Y ≥ 0 and E[Y] exists, then for all t > 0:
$$\mathbb{P}(Y \geq t) \leq \frac{\mathbb{E}[Y]}{t}$$

**Proof**: Let A = {Y ≥ t}. By law of total probability for expectations:
E[Y] = E[Y|A]P(A) + E[Y|A^c]P(A^c) ≥ E[Y|A]P(A) ≥ t · P(A).
Rearrange: P(Y ≥ t) ≤ E[Y]/t. □

#### Theorem 6.7 ★ (Chebyshev's Inequality)
For any RV Z with finite variance, and any t > 0:
$$\mathbb{P}(|Z - \mathbb{E}[Z]| \geq t) \leq \frac{\text{var}(Z)}{t^2}$$

**Proof**: P(|Z − E[Z]| ≥ t) = P((Z − E[Z])² ≥ t²). Apply Markov's inequality to the non-negative RV Y = (Z−E[Z])²: P(Y ≥ t²) ≤ E[Y]/t² = var(Z)/t². □

### 6.3 Weak Law of Large Numbers

#### Theorem 6.5 ★ (WLLN)
If X₁, X₂,… i.i.d. with mean μ, then for any ε > 0:
$$\mathbb{P}\!\left(\left|\frac{1}{n}\sum_{i=1}^n X_i - \mu\right| > \varepsilon\right) \to 0 \quad \text{as } n \to \infty$$

**Proof (finite variance case)**: Let Z = X̄_n. By Theorem 6.3, E[Z] = μ and var(Z) = σ²/n. By Chebyshev:
P(|X̄_n − μ| > ε) ≤ var(X̄_n)/ε² = σ²/(nε²) → 0 as n → ∞. □

---

## Quick Reference: Distributions

### Discrete Distributions

| Distribution | PMF | Mean | Variance | PGF |
|---|---|---|---|---|
| Uniform U{1,…,n} | 1/n | (n+1)/2 | (n²−1)/12 | (s−s^{n+1})/(n(1−s)) |
| Bernoulli Ber(p) | P(X=1)=p | p | p(1−p) | q + ps |
| Binomial Bin(n,p) | C(n,k)p^k(1-p)^{n-k} | np | np(1-p) | (q+ps)^n |
| Poisson Po(λ) | λ^k e^{-λ}/k! | λ | λ | e^{λ(s-1)} |
| Geometric Geom(p) | (1-p)^{k-1}p, k≥1 | 1/p | (1-p)/p² | ps/(1-(1-p)s) |

### Continuous Distributions

| Distribution | PDF | CDF | Mean | Variance |
|---|---|---|---|---|
| Uniform U[a,b] | 1/(b-a) | (x-a)/(b-a) | (a+b)/2 | (b-a)²/12 |
| Exponential Exp(λ) | λe^{-λx}, x≥0 | 1-e^{-λx} | 1/λ | 1/λ² |
| Gamma Γ(α,λ) | λ^α x^{α-1} e^{-λx}/Γ(α) | — | α/λ | α/λ² |
| Normal N(μ,σ²) | (1/√(2πσ²))exp(-(x-μ)²/(2σ²)) | Φ((x-μ)/σ) | μ | σ² |

---

## Key Warnings (Common Exam Traps)

1. **Pairwise independent ≠ independent**
2. **cov(X,Y) = 0 ≠ X,Y independent** (except for jointly normal)
3. **f_X(x) is NOT a probability** (can exceed 1)
4. **E[1/X] ≠ 1/E[X]** in general
5. **Geometric distribution**: two conventions — number of trials until first success (starts at 1) vs number of failures (starts at 0). State which you use.
6. **P(X = x) = 0** for continuous RVs (but P(a ≤ X ≤ b) > 0)

---

## Key Proofs Checklist

| Theorem | Key technique |
|---|---|
| Vandermonde's identity | Combinatorial/counting argument |
| P(A^c) = 1 − P(A) | Axioms P₂ and P₃ |
| Law of total probability | Partition Ω, use P₃ on disjoint sets A ∩ B_i |
| Bayes' theorem | Definition of conditional prob + LTP |
| Independence preserved under complement | A = (A∩B) ∪ (A∩B^c), use P₃ |
| LOTUS (E[h(X)] formula) | Group x-values by h(x)=y, swap sums |
| Partition theorem for expectations | Apply LTP to each P(X=x) |
| Linearity of expectation | Double sum, use marginals |
| E[XY]=E[X]E[Y] when independent | Factor joint PMF, split double sum |
| PGF uniqueness | Repeated differentiation at s=0 recovers all p_k |
| PGF of sum = product of PGFs | Independence of s^X and s^Y |
| PGF of random sum | Condition on N, use independence |
| Branching process PGF recurrence | Theorem 4.8 applied to X_{n+1} = Σ C_i |
| E[X_n] = μ^n (branching) | Chain rule for G_n'(1) |
| Extinction prob = smallest fixed point | Induction: G_n(0) ≤ r if G(r)=r |
| Normal integrates to 1 | Polar coordinates trick on I² |
| Change of variables (pdf) | Differentiate CDF, chain rule |
| Markov's inequality | Law of total expectation on {Y≥t} |
| Chebyshev's inequality | Apply Markov to (Z−E[Z])² |
| Weak Law of Large Numbers | Chebyshev applied to X̄_n with var = σ²/n |
