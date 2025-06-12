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

Let me know if you'd like this saved as a `.md` or `.pdf` file.
