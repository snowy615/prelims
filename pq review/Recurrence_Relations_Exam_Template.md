# Recurrence Relations Exam Template
## Oxford / Cambridge Mathematics Prelims / Part A

---

## 1. Standard Form

A **linear recurrence relation with constant coefficients** has the form:

$$a_n = c_1 a_{n-1} + c_2 a_{n-2} + \dots + c_k a_{n-k} + f(n)$$

- **Order**: $k$ (the largest lag).
- **Homogeneous** if $f(n) = 0$; **non-homogeneous** otherwise.
- **Initial conditions**: usually $a_0, a_1, \dots, a_{k-1}$ are given (or boundary conditions $a_0, a_N$).

---

## 2. Homogeneous Solution $a_n^{(h)}$

### Step 1: Characteristic Equation

Substitute $a_n = \lambda^n$ (or $a_n = r^n$) to obtain:

$$\lambda^k - c_1 \lambda^{k-1} - c_2 \lambda^{k-2} - \dots - c_k = 0$$

### Step 2: Classify Roots & Write General Solution

| Root Type | Multiplicity | Contribution to $a_n^{(h)}$ |
|-----------|-------------|---------------------------|
| Real, distinct | $m=1$ | $A \lambda^n$ |
| Real, repeated | $m$ | $(A_0 + A_1 n + \dots + A_{m-1} n^{m-1}) \lambda^n$ |
| Complex conjugate $\alpha \pm \beta i = \rho e^{\pm i\theta}$ | $m=1$ | $\rho^n (A \cos n\theta + B \sin n\theta)$ |
| Complex conjugate, repeated | $m$ | $\rho^n \left[ (A_0 + \dots + A_{m-1}n^{m-1})\cos n\theta + (B_0 + \dots + B_{m-1}n^{m-1})\sin n\theta \right]$ |

**Critical distinction:**
- **Real double root** $\lambda$: gives $A\lambda^n + Bn\lambda^n$.
- **Complex conjugate pair** $\lambda = \alpha \pm \beta i$: gives $\rho^n(A\cos n\theta + B\sin n\theta)$.
- **Do NOT** use $A + Bn + Cn^2$ for complex roots—that is only for real repeated roots of multiplicity 3.

---

## 3. Non-Homogeneous Particular Solution $a_n^{(p)}$

### Method of Undetermined Coefficients

Guess $a_n^{(p)}$ based on $f(n)$, then substitute into the recurrence to solve for constants.

| $f(n)$ | Guess $a_n^{(p)}$ | Adjustment if guess overlaps with $a_n^{(h)}$ |
|--------|------------------|-----------------------------------------------|
| Polynomial $P_d(n)$ | $Q_d(n)$ (same degree) | Multiply by $n^m$ where $m$ is multiplicity of root $1$ |
| $C \cdot \alpha^n$ | $K \alpha^n$ | Multiply by $n^m$ if $\alpha$ is a characteristic root of multiplicity $m$ |
| $\alpha^n P_d(n)$ | $\alpha^n Q_d(n)$ | Same rule as above |
| $\cos(\gamma n)$ or $\sin(\gamma n)$ | $A\cos(\gamma n) + B\sin(\gamma n)$ | Multiply by $n^m$ if $e^{\pm i\gamma}$ matches a root |

### Superposition Principle
If $f(n) = f_1(n) + f_2(n)$, find particular solutions for each separately and add them.

---

## 4. Boundary Conditions & Constants

### Standard Initial-Value Problem
Given $a_0, a_1, \dots, a_{k-1}$:
1. Write $a_n = a_n^{(h)} + a_n^{(p)}$.
2. Plug in $n=0, 1, \dots, k-1$.
3. Solve the linear system for constants $A, B, \dots$.

### Boundary-Value Problem (e.g., $a_0 = 0, a_N = 0$)
Common in **probability** (hitting probabilities, expected times):
1. Same general solution.
2. Plug in both boundaries to get two equations.
3. **If only one boundary is given** (e.g., $a_N = 0$), look for:
   - **Symmetry**: $a_{-k} = a_k$ or $a_{N+k} = a_{N-k}$.
   - **Boundedness**: If the solution must remain bounded as $n \to \infty$, discard growing terms (set their coefficients to 0).
   - **Physical/probabilistic constraint**: Probabilities must lie in $[0,1]$; expected times must be finite.

### Non-Homogeneous + Boundary Example (Expected Hitting Time)
Recurrence: $e_{i+1} - 2e_i + e_{i-1} = -2$ with $e_0 = 0, e_N = 0$.

1. **Homogeneous**: $e_i^{(h)} = A + Bi$ (double root $\lambda = 1$).
2. **Particular**: RHS is constant, so guess $e_i^{(p)} = Ci^2$.
   - Substitute: $C(i+1)^2 - 2Ci^2 + C(i-1)^2 = 2C = -2 \Rightarrow C = -1$.
   - Thus $e_i^{(p)} = -i^2$.
3. **General**: $e_i = A + Bi - i^2$.
4. **Boundaries**:
   - $e_0 = A = 0$.
   - $e_N = BN - N^2 = 0 \Rightarrow B = N$.
5. **Solution**: $e_i = Ni - i^2 = i(N-i)$.

**Symmetry trick**: If the walk is symmetric about 0, impose $e_{-N} = e_N$ or $e_{-N} = 0$ to get the second condition.

---

## 5. Probability Recurrences (The "Big Three")

### 5.1 Absorption / Hitting Probability
Let $r_i = P(\text{hit target } T \text{ before } S \mid X_0 = i)$.

By **Law of Total Probability** (condition on first step):

$$r_i = \sum_{j} P(X_1 = j \mid X_0 = i) \cdot r_j$$

with boundary conditions $r_T = 1, r_S = 0$.

**Template sentence for exam:**
> "Conditioning on the first step, by the Markov property the future depends only on the current state $i$. Let $r_i = P(A \mid X_0 = i)$. By the Law of Total Probability applied to the partition $\{X_1 = i-1, X_1 = i+1\}$ (or whatever states are reachable), we have ..."

### 5.2 Expected Time to Absorption
Let $e_i = E[\text{time to hit } T \mid X_0 = i]$.

By **Law of Total Expectation** (condition on first step):

$$e_i = 1 + \sum_{j} P(X_1 = j \mid X_0 = i) \cdot e_j$$

The **$+1$** accounts for the step just taken. Boundary: $e_T = 0$.

**Template sentence:**
> "Conditioning on the first step, the walker takes one step and then expects $e_j$ further steps from the new state $j$. By the Law of Total Expectation, $e_i = 1 + \sum_j p_{ij} e_j$."

### 5.3 Expected Value Evolution (P 2023 Q7 style)
Let $r_k = E[R_k]$ where $R_k$ is a random variable after $k$ steps.

If you have computed $E[R_{k+1} \mid R_k = r] = \alpha + \beta r$, then by LTE:

$$r_{k+1} = E[E[R_{k+1} \mid R_k]] = E[\alpha + \beta R_k] = \alpha + \beta r_k$$

This is a **first-order linear recurrence** in expectations, solvable by standard methods.

---

## 6. Common Errors & Checks

### Error 1: Confusing Real Repeated vs. Complex Roots
- **Real double root $\lambda$**: $a_n = (A + Bn)\lambda^n$.
- **Complex pair $\rho e^{\pm i\theta}$**: $a_n = \rho^n(A\cos n\theta + B\sin n\theta)$.
- **Never** write $A + Bn + Cn^2$ for a single complex pair.

### Error 2: Forgetting the $+1$ in Expected Time
In $e_i = 1 + \frac{1}{2}e_{i-1} + \frac{1}{2}e_{i+1}$, the **1** is mandatory. Without it, you get the homogeneous equation for probabilities, not times.

### Error 3: Particular Solution Overlap
If $f(n) = 2^n$ and $2$ is a characteristic root, you must guess $Kn\cdot 2^n$ (or $Kn^m 2^n$ for multiplicity $m$), not just $K2^n$.

### Error 4: Boundary Conditions in Non-Homogeneous Problems
For $e_i = A + Bi - i^2$ with $e_0 = 0, e_N = 0$:
- $e_0 = 0$ gives $A = 0$.
- $e_N = 0$ gives $BN - N^2 = 0$, so $B = N$.
- **Do not** plug $i=0$ into the particular solution alone and forget the homogeneous part.

### Error 5: Missing Degrees of Freedom
A second-order recurrence needs **two** independent conditions. If the problem only gives one, actively look for:
- Symmetry ($a_{-k} = \pm a_k$)
- Boundedness (discard $\lambda^n$ with $|\lambda| > 1$)
- Normalization ($\sum p_i = 1$)

---

## 7. Quick Reference: Solving Steps

1. **Identify order** and whether homogeneous.
2. **Write characteristic equation** and find all roots.
3. **Write $a_n^{(h)}$** using the root table.
4. **If non-homogeneous**: guess $a_n^{(p)}$, adjust for overlap, substitute, solve for coefficients.
5. **Combine**: $a_n = a_n^{(h)} + a_n^{(p)}$.
6. **Apply conditions** (initial or boundary) to solve for constants.
7. **Sanity check**:
   - Plug $n=0,1$ back into original recurrence.
   - Check limiting behavior (probabilities in $[0,1]$, times positive).
   - Check symmetry if applicable.

---

## 8. Example Skeleton (P 2024 Q8 style)

**Problem**: Random walk on $\{0,1,\dots,n\}$, absorbing at $0$ and $n$. Find $r_i = P(\text{hit } n \text{ before } 0 \mid X_0 = i)$.

**Step 1 (Condition)**:
$$r_i = \frac{1}{2}r_{i-1} + \frac{1}{2}r_{i+1}, \quad 1 \le i \le n-1$$

**Step 2 (Characteristic)**:
$$\lambda^2 - 2\lambda + 1 = 0 \Rightarrow (\lambda - 1)^2 = 0 \Rightarrow \lambda = 1 \text{ (double root)}$$

**Step 3 (General)**:
$$r_i = A + Bi$$

**Step 4 (Boundaries)**:
$$r_0 = 0 \Rightarrow A = 0, \qquad r_n = 1 \Rightarrow Bn = 1 \Rightarrow B = \frac{1}{n}$$

**Step 5 (Answer)**:
$$r_i = \frac{i}{n}$$

**Check**: $r_0 = 0$, $r_n = 1$, $r_i$ is a valid probability in $(0,1)$. $\square$

---

*Memorise the root table and the three probability templates. In an exam, write the characteristic equation and boundary conditions explicitly—even if you make an arithmetic error, you will earn method marks.*
