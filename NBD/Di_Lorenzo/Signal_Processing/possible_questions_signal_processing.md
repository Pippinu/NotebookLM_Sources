# 1 - Signals

### Question - Projection Theorem

### Answer

#### I. The Fundamental Problem: Signal Approximation

Given a signal $x$ in an N-dimensional space ($\mathbb{C}^{N}$), which can be represented as a linear combination of N basis vectors, if we want to approximate this signal using only K basis vectors (where K < N) to retain as much information as possible and minimize the approximation error, we can exploit the Projection Theorem.

#### II. The Projection Theorem

The Projection Theorem states that the best approximation $\tilde{x}$ of a signal $x$ in a subspace P is the orthogonal projection of $x$ onto P. This approximation $\tilde{x}$ is the closest possible to the original signal $x$. The theorem also states that the approximation error $e = x - \tilde{x}$ is orthogonal to the approximation $\tilde{x}$ and to every vector in the subspace P. Geometrically, this means finding the best approximation is like "dropping a perpendicular" from the tip of the vector $x$ onto the subspace P. The point where it lands is $\tilde{x}$, and the perpendicular line itself is the error vector $e$.

This projection $\tilde{x}$ is constructed using the synthesis formula, representing the signal as a linear combination of the K selected basis functions and their corresponding coefficients:

$$\tilde{x}=\sum_{k=0}^{K-1}X_{k}u_{k}$$

These coefficients are precisely determined by the analysis formula, which leverages the inner product operation to project the signal onto each of the K basis vectors:

$$X_{k}=\frac{(u_{k},x)}{||u_{k}||^{2}}=\frac{u_{k}^{H}x}{u_{k}^{H}u_{k}}$$

Thus, each coefficient $X_k$ quantifies the "amount" or "similarity" of the signal's content aligned with that specific basis vector.

#### III. How to Choose the "Best" Subspace?

The theorem tells us how to project onto a given subspace, but the choice of the K basis vectors that create the "best" subspace for approximation is crucial. There are primarily two approaches:

1.  **Using a Fixed, Standard Basis:** We start with a known, complete basis (e.g., Fourier, Discrete Cosine Transform (DCT), Wavelet) for the N-dimensional space. We then compute all N expansion coefficients $X_k$ and select the K basis vectors that correspond to the K coefficients with the largest magnitudes. This strategy ensures that the subspace spanned by these selected vectors captures the most significant portion of the signal's energy, thereby minimizing the approximation error by discarding the least energetic components.

2.  **Using Signal-Dependent (Data-Adaptive) Bases (e.g., Karhunen-Loève Transform (KLT) or Principal Component Analysis (PCA)):** Techniques like PCA derive a custom basis optimally tailored to the statistical properties of the signal. The first K vectors of this basis are guaranteed to span the K-dimensional subspace that minimizes the mean squared approximation error across all possible K-dimensional subspaces.

#### IV. Consequences and Importance

The Projection Theorem is a cornerstone concept with wide-ranging applications:

* **Data Compression:** Particularly in lossy techniques, the Projection Theorem provides the theoretical foundation. By selecting a subspace spanned by K "important" basis vectors and representing the signal with fewer coefficients, the theorem guarantees that the reconstructed signal has the minimum possible error for that chosen number of dimensions, effectively removing redundancies and irrelevant information. This applies to both 1D signals like sound and 2D signals like images.

* **Noise Reduction:** If the desired signal components are assumed to lie primarily within a certain subspace P, while noise components are spread across other dimensions, projecting the noisy signal onto P can effectively reduce noise. The error vector, being orthogonal to P, would contain the parts of the signal (including noise) that couldn't be represented by the chosen basis vectors.

* **Feature Extraction:** The coefficients $X_k$ used to form the best approximation $\tilde{x}$ (obtained via the analysis formula) are considered the most significant features of the signal with respect to the chosen basis. These features capture the essence of the signal within that subspace.

* **Basis for Advanced Algorithms:** The principle of finding an optimal solution by projection is fundamental to many advanced algorithms in signal processing, statistics, and machine learning, such as least squares estimation and PCA.


## 2 - Spectral Analysis

### Question: Time-Frequency Analysis: Spectrogram vs Scalogram

### Answer

#### I. The Problem: Limitations of Global Fourier Transform

Techniques like the Fourier Transform (FT) and Discrete Fourier Transform (DFT) allow us to represent continuous or discrete-time signals in the frequency domain, enabling the study of frequency magnitudes and phase alignment. However, these methods lose information about *when* those frequencies occur or assume certain magnitudes and phases. This is a major drawback for non-stationary signals like music and voice, where frequency content changes over time.

#### II. The Spectrogram (via Short-Time Fourier Transform - STFT)

To overcome this problem, the Short-Time Fourier Transform (STFT) and its corresponding 2D visualization, the Spectrogram, are adopted. The signal is split into small, possibly overlapping, segments or "chunks" of length N. On each chunk, the STFT is computed to determine its frequency domain representation. The STFT coefficients $S[k,m]$ are given by:

$$S[k,m]=\sum_{n=0}^{N-1}x[mM+n]w[n]e^{-j2\pi nk/N}$$

where $m$ is the time-chunk index, $k$ is the frequency index, $N$ is the chunk length, and $w[n]$ is an optional window function. The Spectrogram then visualizes the squared magnitude of these coefficients, $|S[k,m]|^2$. This allows us to understand where and when the signal's energy concentrates, providing a time-varying view of its frequency content.

The main trade-off of this technique is that the chunk size (N) is fixed, leading to a direct relationship between time and frequency resolution. For small chunks (small N), the STFT is computed over fewer samples, resulting in good time resolution, allowing us to pinpoint rapid changes in time. However, this leads to poor frequency resolution, meaning we are less able to distinguish between closely spaced frequencies, as frequency resolution is inversely proportional to N (given by $F_s/N$ Hz). Conversely, for large chunks (large N), the STFT is computed over more samples, leading to bad time resolution as events are averaged over a longer period. Yet, this yields good frequency resolution, enabling finer details in the frequency spectrum. Thus, we face an inherent time-frequency uncertainty principle: we cannot simultaneously achieve arbitrarily high resolution in both time and frequency.

#### III. The Scalogram (via Continuous Wavelet Transform - CWT)

To overcome this fixed-resolution trade-off, a multi-resolution analysis technique like the Wavelet Transform can be adopted. This approach uses adaptive windows: short windows (compressed wavelets) for high frequencies to achieve good time resolution, and long windows (stretched wavelets) for low frequencies to achieve good frequency resolution. This is achieved by generating a family of wavelets from a single prototype called the mother wavelet, $h(t)$. A specific wavelet $h_{a,\tau}(t)$ is obtained by scaling ($a$) and translating ($\tau$) the mother wavelet:

$$h_{a,\tau}(t)=\frac{1}{\sqrt{a}}h\left(\frac{t-\tau}{a}\right)$$

The Wavelet Transform then computes the similarity (via an inner product) between the signal and these scaled and shifted wavelets. The coefficients resulting from this operation are used in the Scalogram, a 2D visualization of the squared magnitude of the wavelet coefficients, $|CWT_x(\tau,a)|^2$.

#### IV. Comparison: Spectrogram vs. Scalogram

To compare both techniques:

The **Spectrogram** (derived from STFT) provides a time-frequency visual representation based on a fixed window size and leverages Fourier-based transforms. This results in a uniform tiling of the time-frequency plane, leading to an inherent trade-off in resolution that cannot be overcome for all frequencies simultaneously.

The **Scalogram** (derived from CWT) provides a time-scale (still time-frequency) visual representation based on adaptive window sizes. At small scales (high frequencies), wavelets are compressed, yielding good time resolution but coarser frequency resolution. At large scales (low frequencies), wavelets are stretched, yielding good frequency resolution but coarser time resolution. This adaptive tiling of the time-frequency plane is the main strength of wavelet analysis.

In conclusion, the Scalogram's adaptive analysis is better suited for analyzing non-stationary signals that exhibit both transient bursts and long, quasi-stationary components, as it automatically adjusts its "view" to be appropriate for different time-frequency characteristics.

## 3 - Data Compression

### Question

Describe the typical block scheme of an image compression system. Detail the function of each component within both the encoder and the decoder, explaining how they contribute to the overall compression and decompression process.

### Answer

A typical image compression system is structured into an encoder and a decoder, working in tandem to compress and reconstruct the image data, respectively.

**Encoder:** The encoder's primary goal is to transform the input image into a compact representation by systematically removing redundancies[cite: 715]. It comprises three sequential operations:
* **Mapper:** This component transforms the input image, $f(x,y)$ (or $f(x,y,t)$ for video), into a different, usually non-visual, format[cite: 135, 716]. Its design aims to reduce spatial and temporal redundancy within the data[cite: 135, 716]. This stage also "decorrelates data or compacts its energy"[cite: 696], making the data more suitable for subsequent compression steps[cite: 717].
* **Quantizer:** The quantizer reduces the accuracy or precision of the mapper's output[cite: 135, 718]. This reduction is guided by a pre-established fidelity criterion, balancing the amount of compression with the quality of the reconstructed image[cite: 135, 719]. The main purpose of quantization is to discard irrelevant or less critical information[cite: 135, 720], and it is typically where information is lost in lossy compression schemes[cite: 739].
* **Symbol Coder:** This final encoding stage generates a fixed- or variable-length code to represent the quantized output[cite: 135, 722]. It maps the data in accordance with this code, producing the final compressed data stream ready for storage or transmission[cite: 135, 721].

**Decoder:** The decoder's role is to reverse the encoding process to reconstruct an approximation of the original image. It typically consists of two main components:
* **Symbol Decoder:** This component uses the same coding scheme employed by the encoder to convert the compressed codes back into a sequence of quantized values[cite: 137, 725].
* **Inverse Mapper:** This component attempts to reverse the original transformation applied by the mapper in the encoder[cite: 137, 726]. It converts the processed data back into the image domain to produce the reconstructed image, $\hat{f}(x,y)$ (or $\hat{f}(x,y,t)$)[cite: 137, 726]. It's important to note that if lossy compression techniques were used, the reconstructed image will be an approximation of the original, not an exact replica[cite: 727].

### Question

What is the primary goal of signal compression (or source encoding), and why is it necessary in modern data management? Explain the concept of "redundancy" in signals and provide examples of how redundancy manifests.

### Answer

The primary goal of signal compression, also known as source encoding, is to **reduce the amount of data required for effective signal representation**[cite: 700]. This process is crucial in modern data management due to the significant data volumes associated with uncompressed signals. For instance, a two-hour standard definition (SD) television movie, if stored uncompressed, would occupy approximately 224 gigabytes, necessitating a compression factor of around 26.3 to fit onto a standard 8.5 GB DVD[cite: 703].

The feasibility of compression arises from the presence of **redundancy** in data, meaning that the same information can often be conveyed using fewer bits because the data contains irrelevant or repeated information[cite: 128]. Redundancy commonly manifests in several ways:
* **Spatial and Temporal Redundancy:** In images, adjacent pixels often have similar values, leading to spatial correlation where information is unnecessarily replicated[cite: 709]. Similarly, in video sequences, strong correlations exist between pixels across consecutive frames, constituting temporal redundancy[cite: 709].
* **Irrelevant Information:** Datasets often contain information that is imperceptible to human senses or is otherwise ignored. For example, certain audio frequencies might be beyond human hearing, or fine visual details might not be critical for perception[cite: 710]. Removing such information significantly reduces data size with minimal impact on perceived quality[cite: 711].

---

### Block Transform Coding

Many real-world signals, whether 1D like music or 2D like images, are significant in terms of data size due to their quantity and quality. Therefore, data compression, which is the process of reducing data weight, is essential for both storage and, most importantly, transmission. This is possible because these signals often contain a considerable amount of redundancies—data that overlaps on consecutive samples or time instants, or irrelevant information like higher frequencies that are imperceptible to human hearing. These redundancies can be broadly grouped into spatial or temporal redundancies and irrelevant information.

The general process of data compression involves several key steps:

* [cite_start]**Mapper:** Its purpose is to transform data into a new format that allows for the concentration of the signal's energy and decorrelation[cite: 3]. [cite_start]This step actively reduces spatial and temporal redundancies, making the data more suitable for subsequent compression stages[cite: 3].
* [cite_start]**Quantizer:** This stage reduces the precision of the mapped data by discarding irrelevant or less critical information[cite: 3]. [cite_start]This reduction follows a predefined fidelity criterion that balances the trade-off between the quantity of data removed and the quality of the data retained[cite: 3].
* [cite_start]**Symbol Coder:** The symbol coder's purpose is to generate a compact code from the quantized data, producing the final compressed data stream ready for storage or transmission[cite: 3]. [cite_start]This code can be fixed-length or, more commonly, variable-length[cite: 3].

[cite_start]On the decoder side, the symbol decoder and inverse mapper reverse this process, transforming the bitstream back into the original or an approximated signal, depending on whether lossless or lossy compression was applied by the quantizer[cite: 3].

This is a general framework for data compression, and **Block Transform Coding** is a specific and widely adopted method that fits within this framework. Taking an image (a 2D signal) as an example, it involves the following steps:

1.  [cite_start]**Image Division:** The image is first divided into smaller, non-overlapping $n \times n$ subimages or blocks (e.g., $8 \times 8$ pixel blocks in JPEG)[cite: 3].
2.  [cite_start]**Forward Transform:** A transform (such as the Discrete Cosine Transform (DCT), Fourier Transform, or a Wavelet Transform) is applied to each $n \times n$ subimage[cite: 3]. [cite_start]This converts the spatial data into a frequency-like domain, where the signal's energy is typically more compacted into fewer coefficients[cite: 3]. This facilitates a more efficient representation.
3.  **Quantizer and Symbol Coder:** These stages are conceptually similar to those in the general framework, but with specific implementations for transform coefficients.

In particular, different approaches can be adopted for quantization and symbol coding:

**For the Quantizer, two main approaches are adopted:**

* [cite_start]**Zonal Coding:** This approach assumes that most of the significant image information (coefficients with maximum variance) is concentrated around low frequencies (e.g., the top-left corner of the transform block for DCT)[cite: 3]. [cite_start]A zonal mask, which is a binary template of the same $n \times n$ size as the transform block, is applied to retain coefficients within this predefined "zone" and discard or coarsely quantize higher frequencies outside it[cite: 3]. [cite_start]This is often combined with bit allocation, where more bits are assigned to important coefficients within the zone for finer precision, and fewer or zero bits are assigned to less important ones[cite: 3].

* [cite_start]**Threshold Coding:** This approach assumes that the most important information is carried by coefficients with the largest magnitudes, regardless of their location[cite: 3]. [cite_start]A threshold is defined, and coefficients with magnitudes above this threshold are retained, while those below are reduced or discarded[cite: 3]. Different variations include:
    * [cite_start]**Fixed Threshold:** A single threshold value is applied uniformly to all subimage blocks[cite: 3].
    * [cite_start]**Adaptive Threshold (per subimage):** A different threshold is determined for each subimage, often with the goal of retaining a fixed number of the largest magnitude coefficients per block[cite: 3].
    * **Location-Dependent Threshold (Normalization Array):** This approach combines location awareness with thresholding. [cite_start]Quantization is performed by dividing each transform coefficient $T(u,v)$ by a corresponding value $Z(u,v)$ from a quantization matrix and rounding to the nearest integer[cite: 3]:
        $$\hat{T}(u,v) = \text{round}\left[\frac{T(u,v)}{Z(u,v)}\right]$$
        [cite_start]The values in $Z(u,v)$ are typically larger for higher frequencies (less perceptual importance) and smaller for lower frequencies, effectively applying a coarser quantization (higher effective threshold) to high frequencies[cite: 3].

**For the Symbol Coder:**

The quantized coefficients that "survived" the quantization are then encoded. Since many coefficients are often reduced to zero after quantization, a reordering step is performed before symbol coding. [cite_start]The coefficients are sorted, typically using a zigzag scan, into a 1D sequence[cite: 3]. [cite_start]This reordering groups similar coefficients and creates long runs of zeros, especially from the discarded high-frequency components[cite: 3]. These runs of zeros are then efficiently compressed using run-length encoding (RLE), and the non-zero values (along with run lengths) are typically encoded using variable-length coding techniques like Huffman coding or arithmetic coding. [cite_start]This assigns shorter codes to more frequent symbols, further reducing data size[cite: 3].

## 4 - Linear Processing

### Question:

Define a Linear Time-Invariant (LTI) system. What are the two fundamental properties that characterize an LTI system? Explain each property in your own words.

### Answer:

A system is defined as Linear Time-Invariant (LTI) if it satisfies two fundamental properties: **Linearity** and **Time-Invariance**.

* **Linearity:** A system is linear if it adheres to the **superposition principle**. This means that the system's response to a weighted sum of inputs is equal to the weighted sum of the individual outputs. Formally, if inputs $x_1(t)$ and $x_2(t)$ produce outputs $y_1(t)$ and $y_2(t)$ respectively, then for any constants $a_1$ and $a_2$, the input $a_1x_1(t) + a_2x_2(t)$ will produce the output $a_1y_1(t) + a_2y_2(t)$.

* **Time-Invariance:** A system is time-invariant if its behavior does not change over time. If an input $x(t)$ produces output $y(t)$, then a time-shifted input $x(t - t_0)$ will result in a time-shifted output $y(t - t_0)$, for any $t_0$.

---

### Question:

What is the impulse response of an LTI system and how does it relate to the input and output signals?

### Answer:

The impulse response, $h(t)$, is the output of an LTI system when the input is a Dirac delta function $\delta(t)$. It fully characterizes the system in the time domain.

The output $y(t)$ is given by the **convolution** of the input $x(t)$ with the impulse response $h(t)$:

$$
y(t) = \int_{-\infty}^{\infty} h(\tau)x(t - \tau)\, d\tau = \int_{-\infty}^{\infty} x(\tau)h(t - \tau)\, d\tau
$$

The convolution process involves:

* **Flipping** one function (e.g., $h(\tau)$) to obtain $h(-\tau)$
* **Shifting** it by $t$: $h(t - \tau)$
* **Multiplying** it with $x(\tau)$
* **Integrating** the product over all $\tau$

---

### Question:

How is an LTI system characterized in the frequency domain and why is this representation useful?

### Answer:

In the frequency domain, an LTI system is characterized by its **frequency response**, $H(f)$, which is the Fourier Transform of its impulse response $h(t)$.

The output spectrum $Y(f)$ is the product of the input spectrum $X(f)$ and the system frequency response:

$$
Y(f) = X(f) H(f)
$$

This representation simplifies analysis and design, as convolution in the time domain becomes multiplication in the frequency domain.

---

### Question:

Describe the function and characteristics of ideal low-pass, high-pass, and band-pass filters.

### Answer:

**Low-pass filter:**

* Passes low frequencies (below $B$) and attenuates high ones.
* Ideal frequency response: $H(f) = \text{rect}_{2B}(f)$
* Impulse response: $h(t) = 2B \text{sinc}(2\pi B t)$

**High-pass filter:**

* Passes high frequencies (above $B$), attenuates low ones.
* Frequency response: $H(f) = 1 - \text{rect}_{2B}(f)$
* Impulse response: $h(t) = \delta(t) - 2B \text{sinc}(2\pi B t)$

**Band-pass filter:**

* Passes frequencies in a band around $f_0$ of width $2B$
* Frequency response: $H(f) = \text{rect}_{2B}(f - f_0) + \text{rect}_{2B}(f + f_0)$
* Impulse response: $h(t) = 4B \text{sinc}(2\pi B t) \cos(2\pi f_0 t)$

**Why ideal filters are unrealizable:**

* **Infinite duration** of the impulse response
* **Non-causality**: depends on future inputs
* **Implementation limitations**: real filters must trade off ideal properties

---

### Question:

What are the two main aspects of discretizing a continuous-time signal for digital processing?

### Answer:

1. **Sampling (domain discretization):**
   Converts $x(t)$ to $x[n] = x(nT)$. It replicates the frequency spectrum at intervals of $F_s = 1/T$:

   $$
   X_s(f) = F_s \sum_{k=-\infty}^{\infty} X(f - kF_s)
   $$

2. **Quantization (codomain discretization):**
   Maps each sample amplitude to a finite set of discrete values.

---

### Question:

Explain the Nyquist-Shannon Sampling Theorem and aliasing.

### Answer:

The theorem states that a band-limited signal with max frequency $B$ can be perfectly reconstructed if sampled at **at least $2B$** Hz — the **Nyquist rate**.

If sampled below $2B$, **aliasing** occurs: spectral overlap causes irreversible distortion. To prevent it, use a **low-pass anti-aliasing filter** before sampling to remove components above $B$.

---

### Question:

How is the output of a discrete-time LTI system computed, and what changes when using the DFT?

### Answer:

The output $y[n]$ is the **linear convolution** of input $x[n]$ and impulse response $h[n]$:

$$
y[n] = \sum_{k=-\infty}^{\infty} h[k]x[n - k]
$$

With DFTs, multiplication in frequency domain gives:

$$
Y[k] = X[k] H[k]
$$

This corresponds to **circular convolution** in time:

$$
y[n] = \sum_{m=0}^{N-1} h[m] x[(n - m) \bmod N]
$$

To make circular and linear convolution match, **zero-pad** to length $\geq N_x + N_h - 1$.

---

### Question:

How does 2D linear processing apply to images? Describe it in both spatial and frequency domains.

### Answer:

For 2D signals (like images), the output is the **2D convolution** of input $x(t_1, t_2)$ with impulse response $h(t_1, t_2)$:

$$
y(t_1, t_2) = \iint h(\tau_1, \tau_2) x(t_1 - \tau_1, t_2 - \tau_2) d\tau_1 d\tau_2
$$

In the frequency domain:

$$
Y(f_1, f_2) = H(f_1, f_2) X(f_1, f_2)
$$

For discrete signals $x[m,n]$, the same applies with summations. DFT-based multiplication corresponds to **2D circular convolution** unless signals are zero-padded.

This concept underpins many image processing tasks like filtering, blurring, sharpening, and deblurring.

---

### Real Exam Question: Characterization and Analysis of LTI systems

A system transforms an input signal $x(t)$ into an output signal $y(t)$. Linear Time-Invariant (LTI) systems are a particularly important class of systems because they are significantly easier to analyze. For a system to be classified as LTI, it must satisfy two key properties:

* **Linearity:** The system must obey the superposition principle. For any two input signals $x_1(t)$ and $x_2(t)$ and any two constants $a_1$ and $a_2$, the response to a weighted sum of inputs must be the weighted sum of the individual responses:
    $$\mathcal{L}[a_1x_1(t) + a_2x_2(t)] = a_1\mathcal{L}x_1(t) + a_2\mathcal{L}x_2(t) = a_1y_1(t) + a_2y_2(t)$$
* **Time-Invariance:** The system's behavior must not change over time. This means that if an input $x(t)$ produces an output $y(t)$, then a time-shifted version of that input $x(t-t_0)$ will produce the same output, but shifted by the same amount, $y(t-t_0)$:
    $$\text{If } y(t)=\mathcal{L}x(t), \text{ then } \mathcal{L}x(t-t_0)=y(t-t_0)$$

#### Time-Domain Characterization

A key property of an LTI system is that it is completely characterized in the time domain by its **impulse response**, $h(t)$. The impulse response is defined as the output of the system when the input is a Dirac delta function $\delta(t)$ (an impulse centered at $t=0$): $h(t) = \mathcal{L}\delta(t)$. Once the impulse response $h(t)$ of an LTI system is known, its output $y(t)$ for any arbitrary input signal $x(t)$ can be determined using a mathematical operation called **convolution**:
$$y(t) = x(t) * h(t) = \int_{-\infty}^{\infty} x(\tau)h(t-\tau)d\tau$$
Knowing $h(t)$ provides a complete external description of the system's behavior without needing to know its internal structure.

#### Frequency-Domain Characterization

At the same time, an LTI system is completely characterized in the frequency domain by its **frequency response**, $H(f)$. The frequency response is defined as the Fourier Transform of the impulse response $h(t)$:
$$H(f) = \mathcal{F}h(t) = \int_{-\infty}^{\infty} h(t)e^{-j2\pi ft}dt$$A key property of LTI system behavior in the frequency domain is that, by the convolution property of the Fourier Transform, the Fourier Transform of the output signal $Y(f)$ is simply given by the product of the input signal's spectrum $X(f)$ and the system's frequency response $H(f)$:$$Y(f) = X(f)H(f)$$
This means that convolution in the time domain becomes multiplication in the frequency domain, which significantly simplifies analysis. The magnitude $|H(f)|$ indicates how much the system amplifies or attenuates each frequency $f$, and the phase $\angle H(f)$ indicates the phase shift introduced at each frequency.

#### Applications: Filters

Filters are a fundamental application of LTI systems, designed to selectively modify the frequency content of a signal by passing certain frequencies and attenuating others. Their behavior is defined by their frequency response $H(f)$. We can distinguish between ideal types:

* **Ideal Low-Pass Filter:** Allows low-frequency components (from $f=0$ up to a cutoff frequency $B$) to pass, while completely blocking frequencies higher than $B$. Its frequency response is rectangular, and its corresponding impulse response is a sinc function. Ideality refers to their perfectly sharp cutoffs in the frequency domain, which implies an impulse response (sinc function) that is infinitely long in the time domain, making ideal filters non-causal and therefore unrealizable in practice.
* **Ideal High-Pass Filter:** Allows high-frequency components (above a cutoff frequency $B$) to pass, while blocking low-frequency components (below $B$). Its frequency response can be thought of as 1 minus the response of an ideal low-pass filter, and its impulse response corresponds to a Dirac delta minus the sinc function of the low-pass filter.
* **Ideal Band-Pass Filter:** Allows frequencies within a specific range (a "band") to pass, while blocking frequencies outside that band. Its impulse response is a sinc function multiplied by a cosine, which shifts the low-pass sinc spectrum to be centered around the passband frequencies.

#### Discrete-Time LTI Systems

The concepts of linearity and time-invariance apply directly to discrete-time systems.

* **Time Domain:** The output $y[n]$ of a discrete-time LTI system is given by the discrete convolution between the input $x[n]$ and the system's discrete impulse response $h[n]$ (the response to a Kronecker delta function $\delta[n]$):
    $$y[n] = x[n] * h[n] = \sum_{k=-\infty}^{\infty} h[k]x[n-k]$$
* **Frequency Domain:** For finite-length discrete-time signals, if $X[k]$ is the DFT of the input $x[n]$ and $H[k]$ is the DFT of the impulse response $h[n]$, the DFT of the output, $Y[k]$, is given by their product:
    $$Y[k] = X[k]H[k]$$
    It is important to note that for finite-length signals, this multiplication in the DFT domain corresponds to **circular convolution** in the time domain, not linear convolution.

Filters play a crucial role in applications like signal reconstruction from samples. Discretization of a continuous-time signal with a sampling period $T$ results in the periodization of the signal's spectrum, with replicas repeated at multiples of the sampling frequency $F_s = 1/T$. To recover the original signal, an ideal low-pass filter can be used to isolate a single replication of the spectrum, followed by an inverse Fourier Transform. If the sampling frequency is lower than twice the maximum frequency (Nyquist rate) for a band-limited signal, the tails of consecutive spectral replications will overlap, leading to corrupted spectrum values and information loss. This phenomenon is called **aliasing**. Aliasing is inevitable for non-band-limited signals. The Nyquist-Shannon Sampling Theorem states that for a band-limited signal, perfect reconstruction is possible if the sampling frequency is at least twice the maximum frequency $B$. To prevent aliasing in non-band-limited signals, an **anti-aliasing filter** (a low-pass filter) is applied before sampling to forcibly band-limit the signal, resulting in controlled information loss but avoiding aliasing distortion.

### The Sampling Theorem

#### I. Motivation: From Continuous to Discrete

Physical signals encountered in the real world, such as audio or images, are often continuous-time (analog) signals. To process, store, or transmit these signals using digital computers, they must be converted into discrete-time signals. This conversion is primarily achieved through **sampling**, where the continuous signal $x(t)$ is observed and recorded at specific, uniformly spaced time instants, typically $nT$, creating a discrete sequence $x[n] = x(nT)$. The central question addressed by the Sampling Theorem is: Is it possible to perfectly recover the original analog signal $x(t)$ from its discrete samples $x[n]$?

#### II. The Nyquist-Shannon Sampling Theorem

The **Nyquist-Shannon Sampling Theorem** provides the theoretical foundation for this analog-to-digital conversion and subsequent reconstruction.

**Statement of the Theorem:** If a continuous-time signal $x(t)$ contains no frequencies higher than $B$ Hz (i.e., it is **band-limited** to $B$), it can be completely determined and perfectly reconstructed from its samples if the sampling rate $F_s$ is at least $2B$ samples per second.

* **Nyquist Rate:** The minimum required sampling rate, $2B$, is known as the Nyquist rate.
* **Nyquist Interval:** The maximum time interval between samples, $T = 1/(2B)$, is called the Nyquist interval.

#### III. Frequency Domain Perspective: The Effect of Sampling

The process of sampling a signal in the time domain has a profound effect on its spectrum in the frequency domain. Sampling a continuous signal $x(t)$ with a sampling period $T$ (or sampling frequency $F_s = 1/T$) results in the periodization of its original continuous spectrum $X(f)$. The spectrum of the sampled signal, $X_s(f)$, consists of replicas of the original spectrum $X(f)$ repeated at integer multiples of the sampling frequency $F_s$:
$$X_s(f) = F_s \sum_{k=-\infty}^{\infty} X(f - kF_s)$$
The possibility of perfect reconstruction depends critically on the relationship between the signal's bandwidth $B$ and the sampling frequency $F_s$. There are three primary scenarios:

1.  **Correct Sampling ($F_s \ge 2B$):** When the sampling frequency is greater than or equal to twice the signal's maximum frequency, the spectral replicas do not overlap. There is a "gap" between the end of one replica and the beginning of the next. In this case, the original spectrum $X(f)$ can be perfectly recovered from $X_s(f)$ by applying an ideal low-pass filter to isolate the central replica (around $f=0$).
2.  **Undersampling ($F_s < 2B$):** When the sampling frequency is less than twice the signal's maximum frequency, the spectral replicas overlap. This phenomenon is called **aliasing**. High frequencies from the original signal "fold over" and "disguise" themselves as lower frequencies, causing distortion. Information is irretrievably lost, and perfect reconstruction of the original signal is impossible.
3.  **Non-Band-limited Signal:** If the signal's spectrum extends infinitely, aliasing is theoretically inevitable regardless of the sampling rate chosen. In practice, to prevent severe aliasing distortion, an **anti-aliasing filter** (an analog low-pass filter) is applied to the continuous-time signal *before* sampling. This forcibly band-limits the signal, introducing a controlled amount of information loss but preventing the much more detrimental effects of aliasing.

#### IV. Time Domain Perspective: Signal Reconstruction

The theorem also provides a formula for the perfect reconstruction of the original continuous-time signal $x(t)$ from its samples $x[n] = x(n/(2B))$. This process is known as **interpolation**. The reconstruction is achieved using a sinc interpolation formula:
$$x(t) = \sum_{n=-\infty}^{\infty} x\left(\frac{n}{2B}\right) \text{sinc}\left(2\pi B\left(t - \frac{n}{2B}\right)\right)$$
This formula indicates that the original signal is reconstructed by summing an infinite series of shifted and scaled sinc functions. Each sample value $x[n]$ scales a sinc function, which is centered at the corresponding sample time $nT$. The sinc function acts as the ideal interpolation kernel, which is equivalent to the impulse response of an ideal low-pass filter.

#### V. Conclusion

The Nyquist-Shannon Sampling Theorem is a cornerstone of digital signal processing. It provides the essential theoretical justification for analog-to-digital conversion, precisely defining the conditions under which a continuous signal can be converted into a discrete sequence of samples without any loss of information, and detailing how the original signal can be perfectly reconstructed from these samples. Understanding and adhering to the principles of the Sampling Theorem is crucial for avoiding aliasing and ensuring the integrity of digital signal processing systems.
