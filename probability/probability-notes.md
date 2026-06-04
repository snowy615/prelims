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

- **Sample space** $\Omega$: set of all possible outcomes.
- **Event**: a subset $A \subseteq \Omega$.
- **Complement**: $A^c = \Omega \setminus A$. Set operations: $A \cup B$, $A \cap B$, $A \setminus B$.
- **Disjoint**: $A \cap B = \emptyset$.

### 1.2 Counting

**Arrangements of $n$ distinguishable objects**: $n!$ orderings.

**Stirling's formula**: $n! \sim \sqrt{2\pi}\, n^{n+\frac{1}{2}} e^{-n}$

**Multinomial coefficient** — arrangements of $n$ objects where $\alpha_i$ appears $m_i$ times ($m_1 + \cdots + m_k = n$):
$$\frac{n!}{m_1!\, m_2!\, \cdots\, m_k!}$$

**Binomial coefficient**: $\binom{n}{m} = \dfrac{n!}{m!(n-m)!}$

**Binomial theorem**: $(x+y)^n = \displaystyle\sum_{k=0}^n \binom{n}{k} x^k y^{n-k}$

**Stars and bars**: number of non-negative integer solutions to $x_1 + \cdots + x_m = n$ is $\binom{n+m-1}{n}$.

#### Lemma 1.5 (Vandermonde's Identity) ★
$$\binom{m+n}{k} = \sum_{j=0}^{k} \binom{m}{j}\binom{n}{k-j}$$
**Proof**: Count committees of $k$ from $m$ men and $n$ women; choose $j$ men in $\binom{m}{j}$ ways then $k-j$ women in $\binom{n}{k-j}$ ways. Sum over $j$. $\square$

### 1.3 The Axiomatic Approach

**Probability space**: triple $(\Omega, \mathcal{F}, \mathbb{P})$ where:
- $\mathcal{F}$ (σ-algebra): $\Omega \in \mathcal{F}$; closed under complements (**F**$_2$); closed under countable unions (**F**$_3$).
- $\mathbb{P}$ (probability measure): $\mathbb{P}(A) \geq 0$ (**P**$_1$); $\mathbb{P}(\Omega) = 1$ (**P**$_2$); countable additivity for disjoint events (**P**$_3$).

#### Theorem 1.8 ★ (Basic consequences of axioms)
1. $\mathbb{P}(A^c) = 1 - \mathbb{P}(A)$
2. $A \subseteq B \implies \mathbb{P}(A) \leq \mathbb{P}(B)$

**Proof**:
1. $A \cup A^c = \Omega$ and $A \cap A^c = \emptyset$, so by **P**$_3$: $\mathbb{P}(\Omega) = \mathbb{P}(A) + \mathbb{P}(A^c)$. By **P**$_2$: $\mathbb{P}(\Omega) = 1$, giving $\mathbb{P}(A^c) = 1 - \mathbb{P}(A)$. $\square$
2. $B = A \cup (B \cap A^c)$. Since $B \cap A^c \subseteq A^c$ it is disjoint from $A$. By **P**$_3$: $\mathbb{P}(B) = \mathbb{P}(A) + \mathbb{P}(B \cap A^c) \geq \mathbb{P}(A)$. $\square$

### 1.4 Conditional Probability

**Definition**: If $\mathbb{P}(B) > 0$, then $\mathbb{P}(A \mid B) = \dfrac{\mathbb{P}(A \cap B)}{\mathbb{P}(B)}$.

#### Theorem 1.11 ★ (Conditional probability is a probability measure)
$\mathbb{Q}(A) = \mathbb{P}(A \mid B)$ defines a valid probability space on $(\Omega, \mathcal{F}, \mathbb{Q})$.

**Proof**: Check axioms **P**$_1$–**P**$_3$:
- **P**$_1$: $\mathbb{Q}(A) = \mathbb{P}(A \cap B)/\mathbb{P}(B) \geq 0$. ✓
- **P**$_2$: $\mathbb{Q}(\Omega) = \mathbb{P}(\Omega \cap B)/\mathbb{P}(B) = \mathbb{P}(B)/\mathbb{P}(B) = 1$. ✓
- **P**$_3$: For disjoint $A_i$, the sets $A_i \cap B$ are also disjoint, so $\mathbb{Q}(\bigcup A_i) = \mathbb{P}\bigl((\bigcup A_i) \cap B\bigr)/\mathbb{P}(B) = \sum \mathbb{P}(A_i \cap B)/\mathbb{P}(B) = \sum \mathbb{Q}(A_i)$. $\square$

**Multiplication rule**: $\mathbb{P}(A \cap B) = \mathbb{P}(A \mid B)\,\mathbb{P}(B)$

**Chain rule**:
$$\mathbb{P}(A_1 \cap \cdots \cap A_n) = \mathbb{P}(A_1)\,\mathbb{P}(A_2 \mid A_1)\,\mathbb{P}(A_3 \mid A_1 \cap A_2)\cdots\mathbb{P}(A_n \mid A_1 \cap \cdots \cap A_{n-1})$$

### 1.5 Law of Total Probability and Bayes' Theorem

**Partition**: $\{B_1, B_2, \ldots\}$ is a partition of $\Omega$ if $\Omega = \bigcup B_i$ and $B_i \cap B_j = \emptyset$ for $i \neq j$.

#### Theorem 1.16 ★ (Law of Total Probability)
If $\{B_i\}$ is a partition with $\mathbb{P}(B_i) > 0$ for all $i$, then for any $A$:
$$\mathbb{P}(A) = \sum_{i \geq 1} \mathbb{P}(A \mid B_i)\,\mathbb{P}(B_i)$$

**Proof**: $\mathbb{P}(A) = \mathbb{P}(A \cap \Omega) = \mathbb{P}\!\left(A \cap \bigcup_i B_i\right) = \mathbb{P}\!\left(\bigcup_i (A \cap B_i)\right) = \sum_i \mathbb{P}(A \cap B_i)$ (disjoint) $= \sum_i \mathbb{P}(A \mid B_i)\,\mathbb{P}(B_i)$. $\square$

#### Theorem 1.18 ★ (Bayes' Theorem)
With the same partition:
$$\mathbb{P}(B_k \mid A) = \frac{\mathbb{P}(A \mid B_k)\,\mathbb{P}(B_k)}{\displaystyle\sum_{i \geq 1} \mathbb{P}(A \mid B_i)\,\mathbb{P}(B_i)}$$

**Proof**: $\mathbb{P}(B_k \mid A) = \dfrac{\mathbb{P}(B_k \cap A)}{\mathbb{P}(A)} = \dfrac{\mathbb{P}(A \mid B_k)\,\mathbb{P}(B_k)}{\mathbb{P}(A)}$. Substitute the law of total probability for $\mathbb{P}(A)$. $\square$

**Odds form of Bayes**:
$$\frac{\mathbb{P}(B \mid A)}{\mathbb{P}(B^c \mid A)} = \frac{\mathbb{P}(A \mid B)}{\mathbb{P}(A \mid B^c)} \cdot \frac{\mathbb{P}(B)}{\mathbb{P}(B^c)}$$

### 1.6 Independence

**Definition 1.22**:
1. $A$ and $B$ are **independent** if $\mathbb{P}(A \cap B) = \mathbb{P}(A)\,\mathbb{P}(B)$.
2. A family $\{A_i\}$ is **independent** if for all finite $J$: $\mathbb{P}\!\left(\bigcap_{i \in J} A_i\right) = \prod_{i \in J} \mathbb{P}(A_i)$.
3. **Pairwise independent** $\neq$ independent (warning!).

#### Theorem 1.24 ★
If $A$ and $B$ are independent, then: (a) $A$ and $B^c$ are independent; (b) $A^c$ and $B^c$ are independent.

**Proof (a)**: $A = (A \cap B) \cup (A \cap B^c)$ with disjoint pieces, so
$$\mathbb{P}(A \cap B^c) = \mathbb{P}(A) - \mathbb{P}(A \cap B) = \mathbb{P}(A) - \mathbb{P}(A)\,\mathbb{P}(B) = \mathbb{P}(A)(1 - \mathbb{P}(B)) = \mathbb{P}(A)\,\mathbb{P}(B^c). \quad\square$$

### 1.7 Useful Calculation Rules

**AND**: $\mathbb{P}(A \cap B) = \mathbb{P}(A \mid B)\,\mathbb{P}(B)$; generalise via chain rule.

**OR (disjoint)**: $\mathbb{P}(A_1 \cup \cdots \cup A_n) = \mathbb{P}(A_1) + \cdots + \mathbb{P}(A_n)$.

**OR (complement trick)**: $\mathbb{P}(A_1 \cup \cdots \cup A_n) = 1 - \mathbb{P}(A_1^c \cap \cdots \cap A_n^c)$.

**Inclusion-exclusion**:
$$\mathbb{P}(A_1 \cup \cdots \cup A_n) = \sum_i \mathbb{P}(A_i) - \sum_{i<j}\mathbb{P}(A_i\cap A_j) + \cdots + (-1)^{n+1}\mathbb{P}(A_1\cap\cdots\cap A_n)$$

---

## Chapter 2: Discrete Random Variables

### 2.1 Classical Distributions

| Distribution | PMF | Mean | Variance |
|---|---|---|---|
| **Bernoulli** $\text{Ber}(p)$ | $\mathbb{P}(X=1)=p,\ \mathbb{P}(X=0)=1-p$ | $p$ | $p(1-p)$ |
| **Binomial** $\text{Bin}(n,p)$ | $\binom{n}{k}p^k(1-p)^{n-k},\ k=0,\ldots,n$ | $np$ | $np(1-p)$ |
| **Geometric** $\text{Geom}(p)$ | $(1-p)^{k-1}p,\ k=1,2,\ldots$ | $\frac{1}{p}$ | $\frac{1-p}{p^2}$ |
| **Poisson** $\text{Po}(\lambda)$ | $\frac{\lambda^k e^{-\lambda}}{k!},\ k=0,1,\ldots$ | $\lambda$ | $\lambda$ |

**Note on Geometric**: alternative definition counts failures before first success: $\mathbb{P}(Y=k) = p(1-p)^k$, $k=0,1,\ldots$ (then $Y = X-1$). State which convention you use.

### 2.2 Expectation

**Definition**: $\mathbb{E}[X] = \displaystyle\sum_{x \in \text{Im}X} x\,\mathbb{P}(X=x)$, provided $\displaystyle\sum_{x}|x|\,\mathbb{P}(X=x) < \infty$.

#### Theorem 2.8 ★ (Law of the Unconscious Statistician)
For $h: \mathbb{R} \to \mathbb{R}$:
$$\mathbb{E}[h(X)] = \sum_{x \in \text{Im}X} h(x)\,\mathbb{P}(X=x)$$

**Proof**: Let $A = \{y : y = h(x) \text{ for some } x \in \text{Im}X\}$. Then:
$$\sum_x h(x)\,\mathbb{P}(X=x) = \sum_y \sum_{\substack{x:\,h(x)=y}} h(x)\,\mathbb{P}(X=x) = \sum_y y \sum_{\substack{x:\,h(x)=y}} \mathbb{P}(X=x) = \sum_y y\,\mathbb{P}(h(X)=y) = \mathbb{E}[h(X)]. \quad\square$$

**Proof that $\mathbb{E}[\text{Po}(\lambda)] = \lambda$**:
$$\mathbb{E}[X] = \sum_{k=0}^{\infty} k \cdot \frac{\lambda^k e^{-\lambda}}{k!} = e^{-\lambda} \sum_{k=1}^{\infty} \frac{\lambda^k}{(k-1)!} = \lambda e^{-\lambda} \sum_{j=0}^{\infty} \frac{\lambda^j}{j!} = \lambda e^{-\lambda} e^{\lambda} = \lambda. \quad\square$$

#### Theorem 2.10 (Properties of Expectation)
(a) $X \geq 0 \implies \mathbb{E}[X] \geq 0$. $\quad$ (b) $\mathbb{E}[aX + b] = a\,\mathbb{E}[X] + b$.

**Variance**:
$$\text{var}(X) = \mathbb{E}\!\left[(X - \mathbb{E}[X])^2\right] = \mathbb{E}[X^2] - (\mathbb{E}[X])^2$$

**Theorem 2.12**: $\text{var}(aX + b) = a^2\,\text{var}(X)$.

### 2.3 Conditional Distributions

**Conditional PMF** of $X$ given $B$: $\mathbb{P}(X=x \mid B) = \dfrac{\mathbb{P}(\{X=x\} \cap B)}{\mathbb{P}(B)}$.

**Conditional expectation**: $\mathbb{E}[X \mid B] = \displaystyle\sum_x x\,\mathbb{P}(X=x \mid B)$.

#### Theorem 2.14 ★ (Partition Theorem for Expectations)
If $\{B_i\}$ is a partition with $\mathbb{P}(B_i) > 0$:
$$\mathbb{E}[X] = \sum_{i \geq 1} \mathbb{E}[X \mid B_i]\,\mathbb{P}(B_i)$$

**Proof**: $\mathbb{E}[X] = \sum_x x\,\mathbb{P}(X=x) = \sum_x x \sum_i \mathbb{P}(X=x \mid B_i)\,\mathbb{P}(B_i) = \sum_i \mathbb{P}(B_i) \sum_x x\,\mathbb{P}(X=x \mid B_i) = \sum_i \mathbb{E}[X \mid B_i]\,\mathbb{P}(B_i)$. $\square$

### 2.4 Joint Distributions

**Joint PMF**: $p_{X,Y}(x,y) = \mathbb{P}(X=x,\, Y=y)$.

**Marginals**: $p_X(x) = \displaystyle\sum_y p_{X,Y}(x,y)$ and $p_Y(y) = \displaystyle\sum_x p_{X,Y}(x,y)$.

**Conditional PMF of $Y$ given $X=x$**: $p_{Y \mid X=x}(y) = \dfrac{p_{X,Y}(x,y)}{p_X(x)}$.

**Independence of $X$, $Y$**: $\mathbb{P}(X=x,\, Y=y) = \mathbb{P}(X=x)\,\mathbb{P}(Y=y)$ for all $x, y$.

#### Theorem 2.21 ★ (Linearity of Expectation)
$$\mathbb{E}[aX + bY] = a\,\mathbb{E}[X] + b\,\mathbb{E}[Y] \qquad \text{(no independence needed!)}$$

**Proof**:
$$\mathbb{E}[aX+bY] = \sum_x \sum_y (ax+by)\,p_{X,Y}(x,y) = a\sum_x x \underbrace{\sum_y p_{X,Y}(x,y)}_{p_X(x)} + b\sum_y y \underbrace{\sum_x p_{X,Y}(x,y)}_{p_Y(y)} = a\,\mathbb{E}[X] + b\,\mathbb{E}[Y]. \quad\square$$

#### Theorem 2.23 ★ ($X \perp Y \implies \mathbb{E}[XY] = \mathbb{E}[X]\,\mathbb{E}[Y]$)
**Proof**:
$$\mathbb{E}[XY] = \sum_x \sum_y xy\,\mathbb{P}(X=x, Y=y) = \sum_x \sum_y xy\,\mathbb{P}(X=x)\,\mathbb{P}(Y=y) = \left(\sum_x x\,\mathbb{P}(X=x)\right)\!\left(\sum_y y\,\mathbb{P}(Y=y)\right) = \mathbb{E}[X]\,\mathbb{E}[Y]. \quad\square$$

**Covariance**: $\text{cov}(X,Y) = \mathbb{E}\!\left[(X - \mathbb{E}[X])(Y - \mathbb{E}[Y])\right] = \mathbb{E}[XY] - \mathbb{E}[X]\,\mathbb{E}[Y]$.

Note $\text{cov}(X,X) = \text{var}(X)$.

**Key identity**: $\text{var}(X+Y) = \text{var}(X) + \text{var}(Y) + 2\,\text{cov}(X,Y)$.

> **WARNING**: $\text{cov}(X,Y) = 0$ does **not** imply $X$ and $Y$ are independent.

**i.i.d.**: independent and identically distributed random variables.

---

## Chapter 3: Difference Equations and Random Walks

### 3.1 Difference Equations

**$k$-th order linear recurrence**: $\displaystyle\sum_{j=0}^k a_j u_{n+j} = f(n)$, with $a_0 \neq 0$, $a_k \neq 0$.

#### Theorem 3.3 (General Solution Structure)
General solution $= $ particular solution $+$ general solution to the homogeneous equation $\displaystyle\sum_j a_j w_{n+j} = 0$.

### 3.2 First Order: $u_{n+1} = au_n + b$

- **Homogeneous solution**: $w_n = A \cdot a^n$.
- **Particular solution**: $v_n = \dfrac{b}{1-a}$ if $a \neq 1$; try $v_n = Cn$ if $a = 1$.
- **General solution ($a \neq 1$)**: $u_n = Aa^n + \dfrac{b}{1-a}$.

### 3.3 Second Order: $u_{n+1} + au_n + bu_{n-1} = f(n)$

**Auxiliary equation**: $\lambda^2 + a\lambda + b = 0$.

| Roots | Homogeneous solution |
|---|---|
| Distinct roots $\lambda_1 \neq \lambda_2$ | $w_n = A_1\lambda_1^n + A_2\lambda_2^n$ |
| Repeated root $\lambda_1 = \lambda_2 = \lambda$ | $w_n = (A + Bn)\lambda^n$ |
| Complex roots $re^{\pm i\theta}$ | $w_n = r^n(A\cos n\theta + B\sin n\theta)$ |

**Particular solutions**: guess same form as $f(n)$; if that solves the homogeneous equation, multiply by $n$ (or $n^2$).

#### Example: Fibonacci Numbers
$f_{n+2} = f_{n+1} + f_n$, $f_0 = f_1 = 1$. Auxiliary: $\lambda^2 - \lambda - 1 = 0$, roots $\lambda = \dfrac{1 \pm \sqrt{5}}{2}$.
$$f_n = \frac{1}{\sqrt{5}}\left(\frac{1+\sqrt{5}}{2}\right)^{n+1} - \frac{1}{\sqrt{5}}\left(\frac{1-\sqrt{5}}{2}\right)^{n+1}$$

### 3.4 Random Walks (Gambler's Ruin)

**Setup**: Gambler wins £1 with prob $p$, loses £1 with prob $q = 1-p$. Starts at £$n$, stops at $0$ or $M$.

Let $u_n = \mathbb{P}(\text{bankruptcy})$. Recurrence: $pu_{n+1} - u_n + qu_{n-1} = 0$, with $u_0 = 1$, $u_M = 0$.

Auxiliary: $p\lambda^2 - \lambda + q = 0 \implies (p\lambda - q)(\lambda - 1) = 0$, so $\lambda = q/p$ or $\lambda = 1$.

**Solution ($p \neq \tfrac{1}{2}$)**:
$$u_n = \frac{(q/p)^n - (q/p)^M}{1 - (q/p)^M}$$

**Solution ($p = \tfrac{1}{2}$)**: $u_n = 1 - \dfrac{n}{M}$.

**Expected duration ($p \neq q$)**:
$$e_n = \frac{M}{p-q} \cdot \frac{1-(q/p)^n}{1-(q/p)^M} - \frac{n}{p-q}$$

**Expected duration ($p = q = \tfrac{1}{2}$)**: $e_n = n(M-n)$.

#### Theorem 3.15 (Random walk on $\mathbb{Z}$, non-examinable)
Walk started at $n > 0$ hits $0$ with probability:

$$u_n = \begin{cases} (q/p)^n & \text{if } p > q \\ 1 & \text{if } p \leq q \end{cases}$$

---

## Chapter 4: Probability Generating Functions

### 4.1 Definition and Properties

For non-negative integer-valued $X$:
$$G_X(s) = \mathbb{E}[s^X] = \sum_{k=0}^{\infty} s^k\,\mathbb{P}(X=k)$$

Defined at least for $|s| \leq 1$; $G_X(1) = 1$.

#### Theorem 4.2 ★ (Uniqueness Theorem)
The distribution of $X$ is uniquely determined by $G_X$.

**Proof**: $\dfrac{d^k}{ds^k} G_X(s)\Big|_{s=0} = k!\cdot \mathbb{P}(X=k)$, so we can recover all probabilities. $\square$

#### PGFs of Common Distributions

| Distribution | PGF |
|---|---|
| $\text{Ber}(p)$ | $G_X(s) = q + ps$ |
| $\text{Bin}(n,p)$ | $G_X(s) = (q + ps)^n$ |
| $\text{Po}(\lambda)$ | $G_X(s) = e^{\lambda(s-1)}$ |
| $\text{Geom}(p)$ | $G_X(s) = \dfrac{ps}{1-(1-p)s}$ |

#### Moments via PGF
- $\mathbb{E}[X] = G'_X(1)$
- $G''_X(1) = \mathbb{E}[X(X-1)] = \mathbb{E}[X^2] - \mathbb{E}[X]$, so $\text{var}(X) = G''_X(1) + G'_X(1) - (G'_X(1))^2$
- In general: $\dfrac{d^k}{ds^k} G_X(s)\Big|_{s=1} = \mathbb{E}[X(X-1)\cdots(X-k+1)]$ (factorial moments).

### 4.2 Sum of Independent Variables

#### Theorem 4.3 ★
If $X$ and $Y$ are independent: $G_{X+Y}(s) = G_X(s)\cdot G_Y(s)$

**Proof**: $G_{X+Y}(s) = \mathbb{E}[s^{X+Y}] = \mathbb{E}[s^X \cdot s^Y] = \mathbb{E}[s^X]\,\mathbb{E}[s^Y]$ (by independence) $= G_X(s)\,G_Y(s)$. $\square$

#### Theorem 4.4 ★ (Sum of Bernoullis = Binomial)
If $X_1, \ldots, X_n$ i.i.d. $\text{Ber}(p)$, then $X_1 + \cdots + X_n \sim \text{Bin}(n,p)$.

**Proof**: $G_Y(s) = \mathbb{E}[s^{X_1 + \cdots + X_n}] = \prod_{i=1}^n \mathbb{E}[s^{X_i}] = (q+ps)^n$, which is the PGF of $\text{Bin}(n,p)$. By uniqueness, $Y \sim \text{Bin}(n,p)$. $\square$

#### Theorem 4.5 ★ (Sum of Poissons = Poisson)
If $X_i \sim \text{Po}(\lambda_i)$ independent, then $\displaystyle\sum_i X_i \sim \text{Po}\!\left(\displaystyle\sum_i \lambda_i\right)$.

**Proof**: $G_{\sum X_i}(s) = \displaystyle\prod_i e^{\lambda_i(s-1)} = \exp\!\left((s-1)\displaystyle\sum_i \lambda_i\right)$, which is the PGF of $\text{Po}(\sum\lambda_i)$. $\square$

### 4.3 Random Sums

#### Theorem 4.8 ★ (PGF of Random Sum)
Let $X_1, X_2, \ldots$ be i.i.d. with PGF $G_X(s)$, and $N$ independent of $\{X_i\}$ with PGF $G_N(s)$. Then:
$$G_{\sum_{i=1}^N X_i}(s) = G_N(G_X(s))$$

**Proof**: Condition on $N$:
$$\mathbb{E}\!\left[s^{X_1+\cdots+X_N}\right] = \sum_{n=0}^{\infty} \mathbb{E}\!\left[s^{X_1+\cdots+X_n}\right]\mathbb{P}(N=n) = \sum_{n=0}^{\infty} (G_X(s))^n\,\mathbb{P}(N=n) = G_N(G_X(s)). \quad\square$$

#### Corollary 4.9
If $X_i$ i.i.d. $\text{Ber}(p)$ and $N \sim \text{Po}(\lambda)$, then $\displaystyle\sum_{i=1}^N X_i \sim \text{Po}(\lambda p)$.

### 4.4 Branching Processes

**Setup**: Each individual independently produces offspring with distribution $p(i)$, $i \geq 0$. Let $G(s) = \displaystyle\sum_{i=0}^\infty p(i)s^i$ and $X_n$ = population size in generation $n$.

#### Theorem 4.11 ★
$$G_{X_n}(s) = \underbrace{G(G(\cdots G}_{n+1\text{ times}}(s)\cdots)) = G(G_{X_{n-1}}(s))$$

**Proof**: $X_{n+1} = \displaystyle\sum_{i=1}^{X_n} C_i^{(n)}$ where $C_i^{(n)}$ i.i.d. with PGF $G$. By Theorem 4.8: $G_{X_{n+1}}(s) = G_{X_n}(G(s))$. By induction the result follows. $\square$

#### Corollary 4.12
If $\mu = \mathbb{E}[\text{offspring}] = G'(1)$, then $\mathbb{E}[X_n] = \mu^n$.

**Proof**: $\mathbb{E}[X_n] = G_n'(1)$. By the chain rule: $G_n'(s) = G_{n-1}'(s)\cdot G'(G_{n-1}(s))$. At $s=1$: $\mathbb{E}[X_n] = \mathbb{E}[X_{n-1}]\cdot G'(1) = \mathbb{E}[X_{n-1}]\cdot\mu$. By induction, $\mathbb{E}[X_n] = \mu^n$. $\square$

**Extinction probability**: $q = \mathbb{P}(\text{population eventually dies out})$.

#### Theorem 4.14 ★ (Extinction Probability is Smallest Fixed Point)
$q$ is the **smallest non-negative solution** of $x = G(x)$.

**Proof**: $q = \lim_{n\to\infty} G_n(0)$ (by increasing sequence of events). If $r \geq 0$ also satisfies $G(r)=r$, then $G_0(0) = 0 \leq r$, and if $G_{n-1}(0) \leq r$ then $G_n(0) = G(G_{n-1}(0)) \leq G(r) = r$ (since $G$ is non-decreasing). So $G_n(0) \leq r$ for all $n$, hence $q \leq r$. $\square$

#### Theorem 4.15 ★ (Extinction Criterion)
Assume $p(1) \neq 1$. Then:
- $\mu \leq 1 \implies q = 1$ (certain extinction)
- $\mu > 1 \implies q < 1$ (positive survival probability)

**Proof sketch**: $G$ is convex ($G'' \geq 0$), $G(0) = p_0 \geq 0$, $G(1)=1$, $G'(1)=\mu$.
- $\mu > 1$: slope at $1$ exceeds $1$, so $y = G(x)$ crosses $y = x$ at some $x^* \in (0,1)$. This $x^*$ is the smallest fixed point, so $q = x^* < 1$.
- $\mu \leq 1$: slope at $1$ is $\leq 1$ and $G$ is convex, so $G(x) \geq x$ for all $x \in [0,1)$. Only fixed point in $[0,1]$ is $1$. So $q = 1$. $\square$

---

## Chapter 5: Continuous Random Variables

### 5.1 CDF and PDF

**CDF**: $F_X(x) = \mathbb{P}(X \leq x)$. Properties:
1. $F_X$ is non-decreasing.
2. $\mathbb{P}(a < X \leq b) = F_X(b) - F_X(a)$.
3. $F_X(x) \to 0$ as $x \to -\infty$; $\quad F_X(x) \to 1$ as $x \to \infty$.

**Continuous RV**: $X$ is continuous if $F_X(x) = \displaystyle\int_{-\infty}^x f_X(u)\,du$ for some **pdf** $f_X$ with:
- $f_X(u) \geq 0$ for all $u$
- $\displaystyle\int_{-\infty}^{\infty} f_X(u)\,du = 1$

Note: $\dfrac{dF_X(x)}{dx} = f_X(x)$ wherever $f_X$ is continuous.

> **WARNING**: $f_X(x)$ is **not** a probability — it can exceed 1.

#### Theorem 5.12
For a continuous RV: $\mathbb{P}(X = x) = 0$ for all $x \in \mathbb{R}$, and $\mathbb{P}(a \leq X \leq b) = \displaystyle\int_a^b f_X(x)\,dx$.

### 5.2 Classical Continuous Distributions

| Distribution | PDF | Mean | Variance |
|---|---|---|---|
| **Uniform** $\text{U}[a,b]$ | $\dfrac{1}{b-a}$, $a \leq x \leq b$ | $\dfrac{a+b}{2}$ | $\dfrac{(b-a)^2}{12}$ |
| **Exponential** $\text{Exp}(\lambda)$ | $\lambda e^{-\lambda x}$, $x \geq 0$ | $\dfrac{1}{\lambda}$ | $\dfrac{1}{\lambda^2}$ |
| **Gamma** $\Gamma(\alpha,\lambda)$ | $\dfrac{\lambda^\alpha}{\Gamma(\alpha)} x^{\alpha-1} e^{-\lambda x}$, $x \geq 0$ | $\dfrac{\alpha}{\lambda}$ | $\dfrac{\alpha}{\lambda^2}$ |
| **Normal** $\text{N}(\mu,\sigma^2)$ | $\dfrac{1}{\sqrt{2\pi\sigma^2}}\exp\!\left(-\dfrac{(x-\mu)^2}{2\sigma^2}\right)$ | $\mu$ | $\sigma^2$ |

**Gamma function**: $\Gamma(\alpha) = \displaystyle\int_0^{\infty} u^{\alpha-1} e^{-u}\,du$; for $n \in \mathbb{N}$: $\Gamma(n) = (n-1)!$

**Note**: $\text{Exp}(\lambda) = \Gamma(1, \lambda)$. The parameter $\lambda$ of $\text{Exp}$ is the **reciprocal of the mean**.

#### Example 5.15 ★ (Normal pdf integrates to 1 — polar coordinates trick)
Show $I := \displaystyle\int_{-\infty}^{\infty} \frac{1}{\sqrt{2\pi}} e^{-x^2/2}\,dx = 1$.

**Proof**: 
$$I^2 = \frac{1}{2\pi}\int_{-\infty}^{\infty}\int_{-\infty}^{\infty} e^{-(x^2+y^2)/2}\,dx\,dy$$
Convert to polar coordinates $x = r\cos\theta$, $y = r\sin\theta$, Jacobian $= r$:
$$I^2 = \frac{1}{2\pi}\int_0^{2\pi}\int_0^{\infty} r\,e^{-r^2/2}\,dr\,d\theta = \frac{1}{2\pi}\cdot 2\pi\cdot\left[-e^{-r^2/2}\right]_0^{\infty} = 1.$$
Since $I \geq 0$, we have $I = 1$. $\square$

**Standard normal**: $Z \sim \text{N}(0,1)$. For $X \sim \text{N}(\mu, \sigma^2)$: $X \overset{d}{=} \mu + \sigma Z$, and $F_X(x) = \Phi\!\left(\dfrac{x-\mu}{\sigma}\right)$ where $\Phi$ is the standard normal CDF.

**Proof that $\text{var}(\text{N}(0,1)) = 1$**:
$$\text{var}(Z) = \mathbb{E}[Z^2] = \int_{-\infty}^{\infty} \frac{x^2}{\sqrt{2\pi}} e^{-x^2/2}\,dx$$
Integrate by parts with $u = x$, $dv = \dfrac{x e^{-x^2/2}}{\sqrt{2\pi}}\,dx$:
$$= \left[-\frac{x\, e^{-x^2/2}}{\sqrt{2\pi}}\right]_{-\infty}^{\infty} + \int_{-\infty}^{\infty} \frac{e^{-x^2/2}}{\sqrt{2\pi}}\,dx = 0 + 1 = 1. \quad\square$$

### 5.3 Expectation

$$\mathbb{E}[X] = \int_{-\infty}^{\infty} x\,f_X(x)\,dx, \qquad \mathbb{E}[h(X)] = \int_{-\infty}^{\infty} h(x)\,f_X(x)\,dx$$

**Variance**: $\text{var}(X) = \mathbb{E}[X^2] - (\mathbb{E}[X])^2$

#### Theorem 5.18
$\mathbb{E}[aX + b] = a\,\mathbb{E}[X] + b$ and $\text{var}(aX + b) = a^2\,\text{var}(X)$.

> **WARNING**: In general, $\mathbb{E}\!\left[\dfrac{1}{X}\right] \neq \dfrac{1}{\mathbb{E}[X]}$.

### 5.4 Functions of Continuous RVs

#### Theorem 5.24 ★ (Change of Variables)
If $h: \mathbb{R} \to \mathbb{R}$ is strictly increasing and differentiable, and $Y = h(X)$, then:
$$f_Y(y) = f_X\!\left(h^{-1}(y)\right) \cdot \left|\frac{d}{dy}\,h^{-1}(y)\right|$$

**Proof**: $F_Y(y) = \mathbb{P}(h(X) \leq y) = \mathbb{P}(X \leq h^{-1}(y)) = F_X(h^{-1}(y))$. Differentiate by the chain rule. $\square$

**For non-monotone $h$**: work via the CDF directly on a case-by-case basis.

### 5.5 Joint Distributions

**Joint density**: $f_{X,Y}(x,y) \geq 0$ and $\displaystyle\int_{-\infty}^{\infty}\int_{-\infty}^{\infty} f_{X,Y}(x,y)\,dx\,dy = 1$.

$$F_{X,Y}(x,y) = \int_{-\infty}^{y}\int_{-\infty}^{x} f_{X,Y}(u,v)\,du\,dv$$

**Marginal densities**:
$$f_X(x) = \int_{-\infty}^{\infty} f_{X,Y}(x,y)\,dy, \qquad f_Y(y) = \int_{-\infty}^{\infty} f_{X,Y}(x,y)\,dx$$

#### Theorem 5.27 ★
For jointly continuous $X$, $Y$:
$$\mathbb{P}(a < X \leq b,\; c < Y \leq d) = \int_c^d \int_a^b f_{X,Y}(x,y)\, dx\, dy$$

**Independence**: $X$ and $Y$ are independent iff $f_{X,Y}(x,y) = f_X(x)\,f_Y(y)$ for all $x, y$.

**Covariance** (same formula as discrete):
$$\text{cov}(X,Y) = \mathbb{E}[XY] - \mathbb{E}[X]\,\mathbb{E}[Y], \qquad \text{var}(X+Y) = \text{var}(X) + \text{var}(Y) + 2\,\text{cov}(X,Y)$$

#### Example: Standard Bivariate Normal
Joint density for $-1 < \rho < 1$:
$$f_{X,Y}(x,y) = \frac{1}{2\pi\sqrt{1-\rho^2}} \exp\!\left(-\frac{x^2 - 2\rho xy + y^2}{2(1-\rho^2)}\right)$$
Marginals: $X, Y \sim \text{N}(0,1)$. Covariance: $\text{cov}(X,Y) = \rho$.

**Special property**: For jointly normal $X$, $Y$: $X \perp Y \iff \text{cov}(X,Y) = 0$.

---

## Chapter 6: Random Samples and the Weak Law of Large Numbers

### 6.1 Sample Mean

**Definition**: Random sample of size $n$ = i.i.d. $X_1, \ldots, X_n$. Sample mean: $\bar{X}_n = \dfrac{1}{n}\displaystyle\sum_{i=1}^n X_i$.

#### Theorem 6.3 ★
If $X_i$ i.i.d. with mean $\mu$ and variance $\sigma^2$:
$$\mathbb{E}[\bar{X}_n] = \mu \qquad \text{and} \qquad \text{var}(\bar{X}_n) = \frac{\sigma^2}{n}$$

**Proof**: $\mathbb{E}[\bar{X}_n] = \dfrac{1}{n}\displaystyle\sum_{i=1}^n \mathbb{E}[X_i] = \mu$. For the variance, independence gives $\text{cov}(X_i, X_j) = 0$ for $i \neq j$, so $\text{var}(\bar{X}_n) = \dfrac{1}{n^2}\cdot n\sigma^2 = \dfrac{\sigma^2}{n}$. $\square$

### 6.2 Inequalities

#### Theorem 6.6 ★ (Markov's Inequality)
If $Y \geq 0$ and $\mathbb{E}[Y]$ exists, then for all $t > 0$:
$$\mathbb{P}(Y \geq t) \leq \frac{\mathbb{E}[Y]}{t}$$

**Proof**: Let $A = \{Y \geq t\}$. By the partition theorem for expectations:
$$\mathbb{E}[Y] = \mathbb{E}[Y \mid A]\,\mathbb{P}(A) + \mathbb{E}[Y \mid A^c]\,\mathbb{P}(A^c) \geq \mathbb{E}[Y \mid A]\,\mathbb{P}(A) \geq t\cdot\mathbb{P}(A).$$
Rearranging gives $\mathbb{P}(Y \geq t) \leq \mathbb{E}[Y]/t$. $\square$

#### Theorem 6.7 ★ (Chebyshev's Inequality)
For any RV $Z$ with finite variance, and any $t > 0$:
$$\mathbb{P}(|Z - \mathbb{E}[Z]| \geq t) \leq \frac{\text{var}(Z)}{t^2}$$

**Proof**: $\mathbb{P}(|Z - \mathbb{E}[Z]| \geq t) = \mathbb{P}\!\left((Z - \mathbb{E}[Z])^2 \geq t^2\right)$. Apply Markov's inequality to $Y = (Z - \mathbb{E}[Z])^2$:
$$\mathbb{P}(Y \geq t^2) \leq \frac{\mathbb{E}[Y]}{t^2} = \frac{\text{var}(Z)}{t^2}. \quad\square$$

### 6.3 Weak Law of Large Numbers

#### Theorem 6.5 ★ (WLLN)
If $X_1, X_2, \ldots$ i.i.d. with mean $\mu$, then for any $\varepsilon > 0$:
$$\mathbb{P}\!\left(\left|\frac{1}{n}\sum_{i=1}^n X_i - \mu\right| > \varepsilon\right) \to 0 \quad \text{as } n \to \infty$$

**Proof (finite variance case)**: Let $Z = \bar{X}_n$. By Theorem 6.3, $\mathbb{E}[Z] = \mu$ and $\text{var}(Z) = \sigma^2/n$. By Chebyshev's inequality:
$$\mathbb{P}(|\bar{X}_n - \mu| > \varepsilon) \leq \frac{\text{var}(\bar{X}_n)}{\varepsilon^2} = \frac{\sigma^2}{n\varepsilon^2} \to 0 \quad \text{as } n \to \infty. \quad\square$$

---

## Quick Reference: Distributions

### Discrete Distributions

| Distribution | PMF | Mean | Variance | PGF |
|---|---|---|---|---|
| $\text{U}\{1,\ldots,n\}$ | $\frac{1}{n}$ | $\frac{n+1}{2}$ | $\frac{n^2-1}{12}$ | $\frac{s - s^{n+1}}{n(1-s)}$ |
| $\text{Ber}(p)$ | $\mathbb{P}(X=1)=p$ | $p$ | $p(1-p)$ | $q + ps$ |
| $\text{Bin}(n,p)$ | $\binom{n}{k}p^k(1-p)^{n-k}$ | $np$ | $np(1-p)$ | $(q+ps)^n$ |
| $\text{Po}(\lambda)$ | $\frac{\lambda^k e^{-\lambda}}{k!}$ | $\lambda$ | $\lambda$ | $e^{\lambda(s-1)}$ |
| $\text{Geom}(p)$ | $(1-p)^{k-1}p,\ k \geq 1$ | $\frac{1}{p}$ | $\frac{1-p}{p^2}$ | $\frac{ps}{1-(1-p)s}$ |

### Continuous Distributions

| Distribution | PDF | CDF | Mean | Variance |
|---|---|---|---|---|
| $\text{U}[a,b]$ | $\frac{1}{b-a}$ | $\frac{x-a}{b-a}$ | $\frac{a+b}{2}$ | $\frac{(b-a)^2}{12}$ |
| $\text{Exp}(\lambda)$ | $\lambda e^{-\lambda x}$, $x\geq 0$ | $1-e^{-\lambda x}$ | $\frac{1}{\lambda}$ | $\frac{1}{\lambda^2}$ |
| $\Gamma(\alpha,\lambda)$ | $\frac{\lambda^\alpha}{\Gamma(\alpha)}x^{\alpha-1}e^{-\lambda x}$ | — | $\frac{\alpha}{\lambda}$ | $\frac{\alpha}{\lambda^2}$ |
| $\text{N}(\mu,\sigma^2)$ | $\frac{1}{\sqrt{2\pi\sigma^2}}\exp\!\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)$ | $\Phi\!\left(\frac{x-\mu}{\sigma}\right)$ | $\mu$ | $\sigma^2$ |

---

## Key Warnings (Common Exam Traps)

1. **Pairwise independent $\neq$ independent**
2. $\text{cov}(X,Y) = 0$ does **not** imply $X, Y$ independent (except for jointly normal)
3. $f_X(x)$ is **not** a probability — it can exceed 1
4. $\mathbb{E}\!\left[\dfrac{1}{X}\right] \neq \dfrac{1}{\mathbb{E}[X]}$ in general
5. **Geometric distribution**: two conventions — number of trials until first success (starts at $1$) vs. number of failures (starts at $0$). State which you use.
6. $\mathbb{P}(X = x) = 0$ for continuous RVs (but $\mathbb{P}(a \leq X \leq b) > 0$)

---

## Key Proofs Checklist

| Theorem | Key technique |
|---|---|
| Vandermonde's identity | Combinatorial counting argument |
| $\mathbb{P}(A^c) = 1 - \mathbb{P}(A)$ | Axioms **P**$_2$ and **P**$_3$ |
| Law of total probability | Partition $\Omega$, use **P**$_3$ on disjoint sets $A \cap B_i$ |
| Bayes' theorem | Definition of conditional prob + LTP |
| Independence preserved under complement | $A = (A\cap B) \cup (A\cap B^c)$, use **P**$_3$ |
| LOTUS | Group $x$-values by $h(x)=y$, swap sums |
| Partition theorem for expectations | Apply LTP to each $\mathbb{P}(X=x)$ |
| Linearity of expectation | Double sum, recognise marginals |
| $\mathbb{E}[XY]=\mathbb{E}[X]\mathbb{E}[Y]$ when independent | Factor joint PMF, split double sum |
| PGF uniqueness | Repeated differentiation at $s=0$ recovers all $p_k$ |
| PGF of sum = product of PGFs | Independence of $s^X$ and $s^Y$ |
| PGF of random sum | Condition on $N$, use independence |
| Branching process PGF recurrence | Theorem 4.8 applied to $X_{n+1} = \sum C_i$ |
| $\mathbb{E}[X_n] = \mu^n$ (branching) | Chain rule for $G_n'(1)$, induction |
| Extinction prob = smallest fixed point | Induction: $G_n(0) \leq r$ whenever $G(r)=r$ |
| Normal integrates to 1 | Polar coordinates trick on $I^2$ |
| Change of variables (pdf) | Differentiate CDF via chain rule |
| Markov's inequality | Partition theorem for expectations on $\{Y \geq t\}$ |
| Chebyshev's inequality | Apply Markov to $(Z - \mathbb{E}[Z])^2$ |
| Weak Law of Large Numbers | Chebyshev applied to $\bar{X}_n$ with $\text{var} = \sigma^2/n$ |
