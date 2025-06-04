<script src="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/katex.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/contrib/auto-render.min.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.0/dist/katex.min.css">
<script>
  document.addEventListener("DOMContentLoaded", function () {
    renderMathInElement(document.body, {
      delimiters: [
        {left: "$$", right: "$$", display: true},
        {left: "$", right: "$", display: false}
      ]
    });
  });
</script>

## Important Formulas from `1_signals.pdf`

### 1. Continuous-time vs. Discrete-time Signals
* **Sampling (Conversion from Continuous to Discrete):**
    $$x[n] = x(nT)$$
    (where $x(t)$ is continuous, $x[n]$ is discrete, $T$ is sampling period, $n$ is integer index)
* **Euler's Formula (Fundamental for complex exponentials):**
    $$e^{j\theta} = \cos\theta + j\sin\theta$$

### 2. Basic Discrete-time Signals
* **Unit Impulse $\delta[n]$:**
    $$\delta[n] = \begin{cases} 1, & n=0 \\ 0, & n \ne 0 \end{cases}$$
* **Unit Step $u[n]$:**
    $$u[n] = \begin{cases} 1, & n \ge 0 \\ 0, & n < 0 \end{cases}$$
* **Exponential Signal (e.g., decay/growth):**
    $$x[n] = a^n u[n]$$
* **Discrete-time Sinusoid (example form):**
    $$x[n] = A \cos(\omega n + \phi) \quad \text{or} \quad x[n] = A \cos(2\pi k n + \phi)$$

### 3. Elementary Operations on Discrete-time Signals
* **Time Shift (Delay if $k>0$, Advance if $k<0$):**
    $$y[n] = x[n-k]$$
* **Scaling:**
    $$y[n] = \alpha x[n]$$
* **Sum:**
    $$y[n] = x[n] + w[n]$$
* **Product:**
    $$y[n] = x[n]w[n]$$
* **Integration (Discrete-time Running Sum):**
    $$y[n] = \sum_{m=-\infty}^{n} x[m]$$
* **Differentiation (Discrete-time First-Order Difference):**
    $$y[n] = x[n] - x[n-1]$$

### 4. Energy and Power of Discrete-time Signals
* **Energy of $x[n]$:**
    $$E_x = \sum_{n=-\infty}^{\infty} |x[n]|^2$$
* **Average Power of $x[n]$:**
    $$P_x = \lim_{N\to\infty} \frac{1}{2N+1} \sum_{n=-N}^{N} |x[n]|^2$$

### 5. Classes of Discrete-time Signals
* **Periodic Signal (with period N):**
    $$\tilde{x}[n] = \tilde{x}[n+kN] \quad (\text{for any integer } k)$$
* **Periodization of a finite-support signal $\overline{x}[n]$ (to create $\tilde{x}[n]$ with period N):**
    $$\tilde{x}[n] = \sum_{k=-\infty}^{\infty} \overline{x}[n-kN]$$

### 6. Linear Algebra for Signals
* **Inner Product of $x, y \in \mathbb{C}^N$:**
    $$(x,y) = x^H y = \sum_{n=0}^{N-1} x^*[n]y[n]$$
* **Norm (Squared) and Energy from Inner Product:**
    $$||x||^2 = (x,x) = x^H x = \sum_{n=0}^{N-1} |x[n]|^2 = E_x$$
* **Squared Euclidean Distance:**
    $$||x-y||^2 = ||x||^2 + ||y||^2 - 2\text{Re}\{(x,y)\}$$
* **Orthogonality Condition:**
    $$(x,y) = 0$$
* **Cauchy-Schwarz Inequality:**
    $$|(x,y)| \le ||x|| \cdot ||y||$$

### 7. Signal Expansion over a Basis
* **General Signal Expansion ($x$ in terms of basis $\{u_k\}$):**
    $$x = \sum_{k=0}^{N-1} X_k u_k$$
* **Coefficients for Orthogonal Basis $\{u_k\}$:**
    $$X_k = \frac{(u_k, x)}{||u_k||^2} = \frac{u_k^H x}{u_k^H u_k}$$
* **Orthonormal Basis Condition:**
    $$(u_k, u_l) = \delta[k-l]$$
* **Coefficients for Orthonormal Basis $\{u_k\}$:**
    $$X_k = (u_k, x) = u_k^H x$$
* **Parseval's Relation (for Orthonormal Basis):**
    $$E_x = ||x||^2 = \sum_{k=0}^{N-1} |x[n]|^2 = \sum_{k=0}^{N-1} |X_k|^2$$
* **Unitary Matrix Transform (U with orthonormal columns):**
    Analysis: $X = U^H x$
    Synthesis: $x = U X$

### 8. Best Approximations (Projection Theorem)
* **Best $K$-term Approximation $\tilde{x}$ (using orthogonal basis $\{u_k\}$):**
    $$\tilde{x} = \sum_{k=0}^{K-1} X_k u_k = \sum_{k=0}^{K-1} \frac{(u_k, x)}{||u_k||^2} u_k$$
* **Energy Decomposition (Error $e = x - \tilde{x}$):**
    $$||x||^2 = ||\tilde{x}||^2 + ||e||^2 \quad (\text{since } e \perp \tilde{x})$$

### 9. Analysis and Synthesis Formulas (General)
* **Analysis (Coefficients $X_k$ from signal $x$ using analysis vectors $a_k$):**
    $$X_k = a_k^H x \quad (\text{or matrix form } X = A^H x)$$
* **Synthesis (Signal $x$ from coefficients $X_k$ using synthesis vectors $s_k$):**
    $$x = \sum_k X_k s_k \quad (\text{or matrix form } x = S X)$$
* **Biorthogonality Condition (for perfect reconstruction with $a_k, s_k$):**
    $$a_k^H s_l = \delta[k-l]$$