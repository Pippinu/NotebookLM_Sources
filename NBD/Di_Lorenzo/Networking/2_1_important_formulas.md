## 1. Digital Modulation Fundamentals

* **Number of Unique Symbols (M)**:
    If each symbol groups $k$ bits, then:
    $$\Large M = 2^k$$

* **Symbol Rate ($R_s$)**:
    If $T_s$ is the symbol period (time to transmit one symbol):
    $$\Large R_s = \frac{1}{T_s} \quad \text{symbols/sec}$$

* **Bit Rate (R)**:
    If $k$ bits are transmitted per symbol:
    $$\Large R = k \cdot R_s \quad \text{bits/sec}$$

* **General Average Energy ($E_{avg}$)**:
    If $E_m$ is the energy of the $m$-th symbol and $p_m$ is its transmission probability:
    $$\Large E_{avg} = \sum_{m=1}^{M} p_m E_m \quad \text{Joules}$$

* **Average Energy for Specific Modulation Schemes (assuming equally likely symbols, $p_m = 1/M$)**:
    * **Pulse Amplitude Modulation (PAM)**:
        $$\Large E_{avg} = \frac{(M^2 - 1)E_g}{6} \quad \text{Joules}$$
        (where $E_g$ is the energy of the pulse shaper $g(t)$)
    * **Phase Shift Keying (PSK)**:
        $$\Large E_{avg} = \frac{E_g}{2} \quad \text{Joules}$$
        (Note: $E_m = E_g/2$ for all $m$, so individual symbol energy is constant)
    * **Quadrature Amplitude Modulation (QAM)**:
        $$\Large E_{avg} = \frac{(M - 1)E_g}{3} \quad \text{Joules}$$

## 2. Digital Demodulation: Maximum A Posteriori (MAP) Decision Rule (Logical Flow)

The primary objective of the digital demodulator is to select the symbol $\hat{m}$ that was most likely transmitted, given the received noisy signal $r$. 
* This decision rule aims to minimize the probability of symbol error $P_e = P[\hat{m} \ne m]$.

### 2.1. Fundamental MAP Decision Rule (A Posteriori Probability)

The optimal decision rule is to choose the symbol $\hat{m}$ that maximizes the a posteriori probability $P[s_m|r]$ (the probability that symbol $s_m$ was sent, given that we received signal $r$):
$$\Large \hat{m} = \arg \max_{1 \le m \le M} P[s_m|r]$$

### 2.2. Rewriting with Bayes' Theorem (A Priori and Likelihood)

Directly calculating $P[s_m|r]$ is often impractical. Bayes' Theorem allows us to express this using probabilities that are more easily known or modeled: the **a priori probability** $P[s_m]$ and the **likelihood function** $P[r|s_m]$.

$$P[s_m|r] = \frac{P[r|s_m]P[s_m]}{P[r]}$$

Since $P[r]$ (the probability of receiving $r$) is constant for all possible symbols $s_m$, it can be disregarded in the maximization process. Thus, the MAP rule simplifies to finding the symbol $s_m$ that maximizes the product of its likelihood and prior probability:
$$\hat{m} = \arg \max_{1 \le m \le M} P[r|s_m]P[s_m]$$
* $P[s_m]$: The prior probability of transmitting symbol $s_m$.
* $P[r|s_m]$: The likelihood of receiving signal $r$ given that symbol $s_m$ was transmitted.

### 2.3. Adapting for Additive White Gaussian Noise (AWGN) Channel

In an AWGN channel, the received signal $r(t)$ is the sum of the transmitted signal $s_m(t)$ and Gaussian noise $n(t)$, i.e., $r(t) = s_m(t) + n(t)$. In equivalent vector form, $r = s_m + n$.

The likelihood function $P[r|s_m]$ for an $N$-dimensional signal space in AWGN is a Gaussian probability density function. After substituting this into the MAP rule and taking the logarithm (which is a monotonic function and doesn't change the argmax), then removing constant terms, the decision rule becomes:
$$\hat{m} = \arg \max_{1 \le m \le M} \left( \frac{N_0}{2} \log P[s_m] - \frac{1}{2}||r-s_m||^2 \right)$$
where $N_0$ is the noise power spectral density and $||r-s_m||^2$ is the squared Euclidean distance between the received vector $r$ and the ideal transmitted symbol vector $s_m$.

### 2.4. Final Simplification using Signal Energy and Inner Product

Expanding the squared Euclidean distance term $||r-s_m||^2 = ||r||^2 - 2(r,s_m) + ||s_m||^2$:
* $||r||^2$ (the energy of the received signal) is common to all choices of $m$, so it can be discarded for maximization.
* $||s_m||^2$ is the energy of the ideal transmitted signal $E_m$.
* $(r,s_m)$ is the inner product (correlation) between the received signal $r$ and the ideal symbol $s_m$.

The MAP decision rule can be finally expressed as:
$$\Large \hat{m} = \arg \max_{1 \le m \le M} \left( \eta_m + (r,s_m) \right)$$
where $\large \eta_m = \frac{N_0}{2}\log P[s_m] - \frac{1}{2}E_m$.

This form implies that the demodulator correlates the received signal with each possible transmitted symbol and chooses the symbol that yields the maximum result, adjusted by its prior probability and energy. The inner product $\large (r,s_m)$ can be computed as an integral: $(r,s_m) = \int r(t)s_m(t)dt$.

### 2.5. Maximum Likelihood (ML) Simplification

If all symbols are **equally likely** (i.e., $P[s_m] = 1/M$ for all $m$), then the $\log P[s_m]$ term is a constant and can be disregarded. The MAP rule simplifies to the **Maximum Likelihood (ML) decision rule**:
$$\Large \hat{m}_{ML} = \arg \max_{1 \le m \le M} \left( -\frac{1}{2}E_m + (r,s_m) \right)$$
If, additionally, all symbols have the **same energy** ($E_m$ is constant for all $m$, as is the case in PSK), then the $-\frac{1}{2}E_m$ term is also constant and can be ignored. The ML rule then further simplifies to choosing the symbol that maximizes the correlation $(r,s_m)$. This is equivalent to finding the symbol $s_m$ that has the **minimum Euclidean distance** $||r-s_m||^2$ to the received vector $r$ in the constellation diagram.

## 3. Performance Analysis Formulas

* **Signal-to-Noise Ratio per bit ($\gamma_b$)**:
    $$\Large \gamma_b = \frac{E_b}{N_0}$$
    (where $E_b$ is energy per bit, $N_0$ is noise power spectral density) 

* **Error Probability for Antipodal Signals (Special Case: $p=1/2$, $r_{th}=0$)**:
    $$\Large P_e = Q\left(\sqrt{\frac{2E_b}{N_0}}\right)$$
    (where $Q(x)$ is the Q-function, the tail probability of a standard Gaussian distribution) 

* **Overall Symbol Error Probability for PAM (assuming equally likely symbols)**:
    $$\Large P_e = 2\left(1-\frac{1}{M}\right)Q\left(\sqrt{\frac{6 \log_2 M}{M^2-1}\frac{E_{bavg}}{N_0}}\right)$$ 
    * **Approximation for large $M$**:
        $$\large P_e \approx 2Q\left(\sqrt{\frac{6 \log_2 M}{M^2-1}\frac{E_{bavg}}{N_0}}\right)$$ 

* **Shannon Capacity Formula (for AWGN Channel)**:
    The theoretical maximum rate ($C$) at which data can be reliably transmitted:
    $$\Large C = B \log_2(1+SNR)$$
    * $C$: ***Channel Capacity*** (bits per second, bps) 
    * $B$: ***Bandwidth*** (Hertz, Hz) 
    * $SNR$: ***Signal-to-Noise Ratio*** (dimensionless ratio of Signal Power to Noise Power) 