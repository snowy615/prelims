# Newton's Method Convergence Exam Template
## Oxford / Cambridge Numerical Analysis Prelims / Part A

---

## 1. Single-Variable Newton Iteration

For solving $f(x) = 0$, given $f \in C^2$ and $f'(x) \neq 0$ near the root $x^*$:

$$x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}$$

Define the error $e_n = x_n - x^*$. The goal is to show **quadratic convergence**:

$$\lim_{n \to \infty} \frac{|e_{n+1}|}{|e_n|^2} = C \neq 0$$

---

## 2. Quadratic Convergence Proof (The Standard Template)

### Step 1: Taylor Expand $f(x_n)$ about $x^*$

Since $f(x^*) = 0$:

$$f(x_n) = f(x^* + e_n) = f'(x^*)e_n + \frac{1}{2}f''(\xi_n)e_n^2$$

for some $\xi_n$ between $x_n$ and $x^*$ (Lagrange remainder).

### Step 2: Taylor Expand $f'(x_n)$ about $x^*$

$$f'(x_n) = f'(x^*) + f''(\eta_n)e_n$$

for some $\eta_n$ between $x_n$ and $x^*$.

### Step 3: Substitute into the Iteration

$$x_{n+1} - x^* = e_n - \frac{f'(x^*)e_n + \frac{1}{2}f''(\xi_n)e_n^2}{f'(x^*) + f''(\eta_n)e_n}$$

### Step 4: Simplify to Leading Order

Factor out $e_n$:

$$e_{n+1} = e_n\left[1 - \frac{f'(x^*) + \frac{1}{2}f''(\xi_n)e_n}{f'(x^*) + f''(\eta_n)e_n}\right]$$

For small $e_n$, use $\frac{1}{1+\varepsilon} \approx 1 - \varepsilon$:

$$e_{n+1} = e_n\left[1 - \left(1 + \frac{\frac{1}{2}f''(\xi_n)}{f'(x^*)}e_n - \frac{f''(\eta_n)}{f'(x^*)}e_n + O(e_n^2)\right)\right]$$

$$e_{n+1} = e_n^2 \cdot \frac{f''(\eta_n) - \frac{1}{2}f''(\xi_n)}{f'(x^*)} + O(e_n^3)$$

As $n \to \infty$, $x_n \to x^*$, so $\xi_n, \eta_n \to x^*$:

$$\boxed{\lim_{n\to\infty} \frac{|e_{n+1}|}{|e_n|^2} = \frac{|f''(x^*)|}{2|f'(x^*)|}}$$

**Exam sentence:**
> "By Taylor's theorem with Lagrange remainder, expanding $f(x_n)$ and $f'(x_n)$ about the root $x^*$, and substituting into the Newton update, the linear terms in $e_n$ cancel, leaving $e_{n+1} = \frac{f''(x^*)}{2f'(x^*)}e_n^2 + o(e_n^2)$. Hence the convergence is quadratic with constant $C = \frac{|f''(x^*)|}{2|f'(x^*)|}$."

---

## 3. Alternative: Direct Algebraic Manipulation (When $f$ is a polynomial)

Sometimes the Taylor expansion is messy. If $f$ is a polynomial (or rational), compute $x_{n+1} - x^*$ directly and factor.

**Example** (CM 2022 Q6): $f(x,y) = x^4 - 4xy + 2y^2 + 4$, minimum at $(1,1)$.

For the $x$-update after simplifying using $y_n = x_n$ (which holds for this problem):

$$x_{n+1} = \frac{2x_n^3}{3x_n^2 - 1}$$

Set $x^* = 1$, $e_n = x_n - 1$:

$$e_{n+1} = \frac{2x_n^3}{3x_n^2 - 1} - 1 = \frac{2x_n^3 - 3x_n^2 + 1}{3x_n^2 - 1} = \frac{(x_n - 1)^2(2x_n + 1)}{3x_n^2 - 1}$$

Thus:

$$\frac{|e_{n+1}|}{|e_n|^2} = \frac{|2x_n + 1|}{|3x_n^2 - 1|} \xrightarrow[n\to\infty]{} \frac{3}{2}$$

**Key exam move:** Factor $e_{n+1}$ as $e_n^2 \times (\text{something that converges to a constant})$.

---

## 4. Error Bound & Step Count Estimation

### 4.1 The Error Recurrence

Once quadratic convergence is established, we have:

$$|e_{n+1}| \le K |e_n|^2$$

where $K = \frac{\max |f''|}{2\min |f'|}$ over the interval (or $K \approx \frac{|f''(x^*)|}{2|f'(x^*)|}$).

Iterating:

$$|e_n| \le K^{-1} (K|e_0|)^{2^n}$$

### 4.2 Step Count to Reach Machine Epsilon

Given tolerance $\varepsilon$ (e.g., $\varepsilon = 10^{-16}$) and initial error $|e_0|$:

$$|e_n| \le \frac{1}{K} (K|e_0|)^{2^n} < \varepsilon$$

Taking logs:

$$2^n \ln(K|e_0|) < \ln(K\varepsilon) \quad \Rightarrow \quad 2^n > \frac{\ln(K\varepsilon)}{\ln(K|e_0|)}$$

For small $K|e_0| < 1$, the number of steps is **doubly logarithmic** in $\varepsilon$:

$$n \approx \log_2\left(\log_{1/(K|e_0|)}\frac{1}{K\varepsilon}\right)$$

**Practical rule of thumb:** Each Newton iteration roughly **doubles** the number of correct digits. So to go from $10^{-1}$ to $10^{-16}$ requires about $\lceil \log_2(16) \rceil = 4$ steps.

### 4.3 Explicit Calculation (CM 2019 Q5 style)

Given $|e_0| \le 0.1$ and $K = \frac{15}{14}$:

| Step | Bound on $\lvert e_n \rvert$ |
|------|------------------|
| $n=0$ | $0.1$ |
| $n=1$ | $\frac{15}{14}(0.1)^2 \approx 0.0107$ |
| $n=2$ | $\frac{15}{14}(0.0107)^2 \approx 1.23 \times 10^{-4}$ |
| $n=3$ | $\approx 1.6 \times 10^{-8}$ |
| $n=4$ | $\approx 2.7 \times 10^{-16}$ |

**Conclusion:** 4 iterations suffice for $10^{-16}$ accuracy.

**Exam template:**
> "Since $|e_{n+1}| \le \frac{15}{14}|e_n|^2$ and $|e_0| \le 0.1$, we compute iteratively: $|e_1| \le \frac{15}{14}(0.1)^2 < 0.011$, $|e_2| \le \frac{15}{14}(0.011)^2 < 1.3 \times 10^{-4}$, $|e_3| < 1.8 \times 10^{-8}$, $|e_4| < 3.5 \times 10^{-16}$. Thus 4 steps guarantee the desired precision."

---

## 5. Multivariate Newton's Method

For $\mathbf{F}: \mathbb{R}^m \to \mathbb{R}^m$, solving $\mathbf{F}(\mathbf{x}) = \mathbf{0}$:

$$\mathbf{x}_{n+1} = \mathbf{x}_n - [J_{\mathbf{F}}(\mathbf{x}_n)]^{-1} \mathbf{F}(\mathbf{x}_n)$$

where $J_{\mathbf{F}}$ is the **Jacobian matrix** with entries $(J_{\mathbf{F}})_{ij} = \frac{\partial F_i}{\partial x_j}$.

### 5.1 Practical Form (Avoiding Matrix Inversion)

Solve the linear system for $\mathbf{d}_n$:

$$J_{\mathbf{F}}(\mathbf{x}_n) \mathbf{d}_n = -\mathbf{F}(\mathbf{x}_n)$$

Then update:

$$\mathbf{x}_{n+1} = \mathbf{x}_n + \mathbf{d}_n$$

**Exam tip:** Never write $J^{-1}$ explicitly unless $m=2$ and the inverse is trivial. Write "solve $J\mathbf{d} = -\mathbf{F}$ for $\mathbf{d}$".

### 5.2 Quadratic Convergence (Multivariate)

If $J_{\mathbf{F}}(\mathbf{x}^*)$ is invertible and $\mathbf{F} \in C^2$:

$$\|\mathbf{e}_{n+1}\| \le C \|\mathbf{e}_n\|^2$$

The constant $C$ depends on $\|J^{-1}\|$ and the Lipschitz constant of $J$.

**Exam sentence:**
> "Provided $J_{\mathbf{F}}(\mathbf{x}^*)$ is non-singular and $\mathbf{F}$ is twice continuously differentiable in a neighbourhood of $\mathbf{x}^*$, the multivariate Newton iteration converges quadratically for sufficiently close initial guesses."

---

## 6. Local Convergence Theorem (The Full Statement)

**Theorem (Local Quadratic Convergence).** Let $f: [a,b] \to \mathbb{R}$ be $C^2$ and $x^* \in (a,b)$ with $f(x^*) = 0$ and $f'(x^*) \neq 0$. Then there exists $\delta > 0$ such that for any $x_0 \in (x^* - \delta, x^* + \delta)$, the Newton sequence converges to $x^*$, and:

$$\lim_{n\to\infty} \frac{|x_{n+1} - x^*|}{|x_n - x^*|^2} = \frac{|f''(x^*)|}{2|f'(x^*)|}$$

**What to check in an exam:**
1. $f(x^*) = 0$ (verify root).
2. $f'(x^*) \neq 0$ (simple root).
3. $f''$ continuous near $x^*$ (so Taylor remainder is controlled).
4. Initial guess sufficiently close (interval trapping argument).

---

## 7. Interval Trapping / Staying in Bounds (CM 2019 Q5 style)

To prove the sequence does not escape an interval $[a,b]$:

1. **Bracket the root**: Show $f(c) > 0$ and $f(d) < 0$ with $c < d$, so $x^* \in (c,d)$.
2. **Bound the initial error**: $|e_0| = |x_0 - x^*| \le d - c$.
3. **Show contraction**: If $|e_n| \le \delta$, then $|e_{n+1}| \le K\delta^2 < \delta$ provided $K\delta < 1$.
4. **Induction**: The sequence remains in $[x^* - \delta, x^* + \delta] \subset [a,b]$.

**Template:**
> "Since $f(1.1) > 0$ and $f(1.2) < 0$, the root $x^* \in (1.1, 1.2)$. With $x_0 = 1.2$, we have $|e_0| \le 0.1$. The error bound $|e_{n+1}| \le \frac{15}{14}e_n^2$ gives $|e_1| \le \frac{15}{14}(0.1)^2 < 0.1$, so by induction the sequence never leaves $(1.1, 1.2)$."

---

## 8. Common Errors & Checks

### Error 1: Forgetting the $O(e_n^2)$ in Taylor Expansion
You must expand to **second order** for $f(x_n)$ and **first order** for $f'(x_n)$. Stopping at first order for $f$ gives $e_{n+1} \approx 0$, which is wrong.

### Error 2: Wrong Jacobian Entry
In multivariate problems, double-check partial derivatives. For $f(x,y)$:
- $\frac{\partial f}{\partial x}$ is the first component of the gradient.
- The Hessian $H$ has $H_{11} = \frac{\partial^2 f}{\partial x^2}$, $H_{12} = H_{21} = \frac{\partial^2 f}{\partial x \partial y}$.

### Error 3: Inverting the Jacobian Incorrectly
For $2 \times 2$ matrix $J = \begin{pmatrix} a & b \\ c & d \end{pmatrix}$:

$$J^{-1} = \frac{1}{ad-bc}\begin{pmatrix} d & -b \\ -c & a \end{pmatrix}$$

**Check**: $\det J = ad - bc$. For Newton on optimization, $J = H$ (Hessian), so $\det H = H_{11}H_{22} - H_{12}^2$.

### Error 4: Confusing Root-Finding with Optimization
- **Root-finding**: $x_{n+1} = x_n - f(x_n)/f'(x_n)$.
- **Optimization**: $x_{n+1} = x_n - f'(x_n)/f''(x_n)$ (Newton for finding critical points).
- **Multivariate optimization**: $\mathbf{x}_{n+1} = \mathbf{x}_n - H^{-1}\nabla f$.

### Error 5: Step Count Off-by-One
When computing "how many steps to reach $\varepsilon$", count carefully:
- $e_0$ is the initial error (before any iteration).
- $e_1$ is after 1 iteration.
- If $e_4 < 10^{-16}$, that means **4 iterations** (not 5).

---

## 9. Quick Reference: Exam Answer Skeleton

**Question**: Prove that Newton's method for $f(x) = 0$ converges quadratically, and estimate the number of steps needed to achieve error $\varepsilon$.

**Skeleton:**

1. **State iteration**: $x_{n+1} = x_n - f(x_n)/f'(x_n)$, define $e_n = x_n - x^*$.
2. **Taylor expand**: $f(x_n) = f'(x^*)e_n + \frac{1}{2}f''(\xi_n)e_n^2$, $f'(x_n) = f'(x^*) + f''(\eta_n)e_n$.
3. **Substitute and simplify**: Show $e_{n+1} = \frac{f''(x^*)}{2f'(x^*)}e_n^2 + o(e_n^2)$.
4. **Conclude quadratic**: $\lim |e_{n+1}|/|e_n|^2 = |f''(x^*)|/(2|f'(x^*)|) = C$.
5. **Bound $C$**: Compute $K = \max|f''| / (2\min|f'|)$ over the relevant interval.
6. **Iterate error bounds**: $|e_0| \le \delta$, $|e_1| \le K\delta^2$, $|e_2| \le K(K\delta^2)^2 = K^3\delta^4$, etc.
7. **Count steps**: Find smallest $n$ with $|e_n| < \varepsilon$.
8. **Verify trapping**: Show $K|e_n| < 1$ ensures the sequence stays in the interval.

---

*Master the Taylor expansion proof. In an exam, even if you cannot compute the exact constant, writing the Taylor steps correctly earns the majority of the marks.*
