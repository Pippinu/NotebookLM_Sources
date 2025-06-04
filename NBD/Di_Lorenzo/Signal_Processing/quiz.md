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

## Key Formulas Quiz `1_signals.pdf`

### 1. Continuous-time vs. Discrete-time Signals
* Sampling (Conversion from Continuous to Discrete)
* Euler's Formula

### 2. Basic Discrete-time Signals
* Unit Impulse $\delta[n]$ (Definition)
* Unit Step $u[n]$ (Definition)
* Exponential Signal (Formula involving $u[n]$)
* Discrete-time Sinusoid (General cosine form)

### 3. Elementary Operations on Discrete-time Signals
* Time Shift
* Scaling
* Sum
* Product
* Integration (Discrete-time Running Sum)
* Differentiation (Discrete-time First-Order Difference)

### 4. Energy and Power of Discrete-time Signals
* Energy of a discrete-time signal $x[n]$
* Average Power of a discrete-time signal $x[n]$

### 5. Classes of Discrete-time Signals
* Periodic Signal (Condition for periodicity with period N)
* Periodization of a finite-support signal $\overline{x}[n]$ (to create $\tilde{x}[n]$)

### 6. Linear Algebra for Signals
* Inner Product of two complex vectors $x, y$
* Norm (Squared) and Energy (from inner product of $x$ with itself)
* Squared Euclidean Distance between $x$ and $y$ (in terms of norms and inner product)
* Orthogonality Condition for two vectors $x, y$
* Cauchy-Schwarz Inequality

### 7. Signal Expansion over a Basis
* General Signal Expansion of $x$ in terms of basis $\{u_k\}$ and coefficients $X_k$
* Formula for Coefficients $X_k$ (for an Orthogonal Basis $\{u_k\}$)
* Orthonormal Basis Condition (inner product of basis vectors $u_k, u_l$)
* Formula for Coefficients $X_k$ (for an Orthonormal Basis $\{u_k\}$)
* Parseval's Relation (Energy in terms of coefficients $X_k$ for Orthonormal Basis)
* Unitary Matrix Transform (Analysis and Synthesis formulas using $U$)

### 8. Best Approximations (Projection Theorem)
* Best $K$-term Approximation $\tilde{x}$ (using $K$ orthogonal basis vectors)
* Energy Decomposition (Original signal energy in terms of approximation and error energies)

### 9. Analysis and Synthesis Formulas (General)
* Analysis Formula (Coefficients $X_k$ from signal $x$ using analysis vectors $a_k$)
* Synthesis Formula (Signal $x$ from coefficients $X_k$ using synthesis vectors $s_k$)
* Biorthogonality Condition (between analysis $a_k$ and synthesis $s_k$ vectors)