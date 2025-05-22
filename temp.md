### Relation between FT and DFT

Okay, we've covered the DFT, its matrix form, and the efficiency of the FFT algorithms. Now let's look at slide 59, which discusses the **Relation between the continuous-time Fourier Transform (FT) and the Discrete Fourier Transform (DFT)**.

This is a crucial conceptual bridge. The DFT is what we compute in practice on digital computers, but it's an approximation or a specific representation related to the continuous-time FT of an underlying analog signal.

Slide 59 presents a diagram that illustrates this relationship in four stages:

1.  **Top-Left: Fourier Transform of a continuous function $s(t)$**
    * This shows a continuous-time signal $s(t)$ (not explicitly drawn, but implied) and its continuous-time Fourier Transform $S(f)$. $S(f)$ is a continuous function of frequency, typically shown as a smooth curve (e.g., a Gaussian-like shape in the diagram).
    * This is the ideal, continuous-world scenario: $s(t) \leftrightarrow S(f)$.

2.  **Top-Right: Transform of the periodic summation of $s(t)$ (aka "Fourier Series Coefficients")**
    * This considers a signal formed by taking $s(t)$ and making it periodic with some period $P$. Let's call this periodic signal $s_P(t) = \sum_k s(t-kP)$.
    * As we learned from the **Periodization Property**, the spectrum of such a periodic signal becomes discrete. It consists of impulses (or spectral lines) at frequencies $k/P$ (harmonics of $1/P$).
    * The amplitudes of these impulses are proportional to $S(k/P)$ – samples of the original continuous spectrum $S(f)$ taken at the harmonic frequencies.
    * This is essentially what the **Fourier Series** gives you: a set of discrete coefficients representing a periodic continuous-time signal. The diagram labels this $S(k)$.

3.  **Bottom-Left: Transform of periodically sampled $s(t)$ (aka "Discrete-Time Fourier Transform" - DTFT)**
    * This considers taking the original continuous-time signal $s(t)$ and sampling it in the time domain with a sampling period $T$ (sampling frequency $F_s = 1/T$). Let the sampled signal be $s_{sampled}(t) = \sum_n s(nT)\delta(t-nT)$.
    * As we learned from the **Sampling Property**, the spectrum of this sampled signal, $S_{1/T}(f)$ (the slide uses this notation), becomes a periodic replication of the original spectrum $S(f)$. Replicas of $S(f)$ are centered at multiples of the sampling frequency $F_s = 1/T$.
    * This $S_{1/T}(f)$ is a continuous but periodic function of frequency. This is the **Discrete-Time Fourier Transform (DTFT)** of the sequence of samples $s[n]=s(nT)$.

4.  **Bottom-Right: Transform of both periodic sampling and periodic summation (aka "Discrete Fourier Transform" - DFT)**
    * This is where the DFT comes in. The DFT operates on a **finite number of samples** $s[n]$ (say, $N$ samples).
    * Effectively, the DFT can be understood as the result of performing both operations:
        * **Sampling in time:** This makes the spectrum periodic (as in step 3).
        * **Windowing/Assuming periodicity in time:** When we take $N$ samples for the DFT, we are implicitly dealing with a signal that is either finite in duration or is assumed to be one period of an $N$-point periodic sequence. Making a signal periodic in the time domain (with period $N T_s$, where $T_s$ is the sampling interval) makes its spectrum discrete (sampled in frequency, as in step 2).
    * So, the DFT $S_N(k)$ (as labeled in the diagram) is a **discrete and periodic representation in the frequency domain**. It consists of $N$ discrete frequency samples.
    * The diagram shows these $N$ samples $S_N(k)$ as essentially samples of one period of the periodic spectrum $S_{1/T}(f)$ from step 3.
    * The slide's caption: "The DFT is a discrete version (i.e. samples) of the periodization of the continuous-time signal spectrum, where samples are collected over a single period."
        * This means: Take $S(f)$ (from step 1). Periodize it due to time sampling (step 3, giving $S_{1/T}(f)$). Then, sample this periodic spectrum $S_{1/T}(f)$ to get the DFT coefficients $S_N(k)$.

**In essence:**
The DFT $X[k]$ of a sequence of $N$ samples $x[n]$ can be thought of as $N$ samples of one period of the DTFT $X(e^{j\omega})$ of that sequence. And the DTFT $X(e^{j\omega})$ is, in turn, a scaled and periodic version of the continuous-time Fourier Transform $X_c(f)$ of an underlying analog signal from which $x[n]$ was derived, assuming proper sampling.

This diagram is a very concise way to show how these different Fourier representations relate to each other through operations of sampling and periodization in both domains.

Next, we move on to the properties of the DFT itself. Shall we continue?