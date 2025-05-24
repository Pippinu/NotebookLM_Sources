### Zonal Coding

Zonal coding is one of the two main approaches mentioned for choosing which transform coefficients to keep or how to quantize them, following the transformation stage in block-based image compression.

* **Core Principle:** The fundamental idea behind zonal coding is based on information theory concepts, which suggest that transform coefficients with the **maximum variance** carry the most significant portion of the image information within a block. Therefore, these are the coefficients that should be prioritized and retained during the compression process.
* **Location of High-Variance Coefficients:** For many common types of images and widely used transforms (like the Discrete Cosine Transform - DCT, which is central to JPEG compression), these high-variance coefficients are typically concentrated in a specific region of the transform coefficient block $T(u,v)$. This region is usually around the origin $(u=0, v=0)$, corresponding to the **low-frequency components** of the image block. The $T(0,0)$ coefficient is the DC coefficient, representing the average intensity of the block, and it usually has the highest energy/variance.
* **Zonal Mask:**
    * To implement zonal coding, a **zonal mask** is constructed. This mask is essentially a binary template of the same size as the transform coefficient block (e.g., $\large n \times n$).
    * The mask has a '1' (or some indicator to keep/finely quantize) in locations corresponding to coefficients with high variance (i.e., within the predefined "zone").
    * It has a '0' (or an indicator to discard/coarsely quantize) in all other locations outside this zone.
    * The example image on the slide shows a zonal mask on the left, where the '1's are concentrated in the top-left corner, indicating that these low-frequency coefficients are selected.
* **Bit Allocation:**
    * Often, zonal coding is combined with a **bit allocation** strategy. Coefficients within the selected zone (deemed more important) are allocated more bits, allowing for finer quantization and thus higher precision.
    * Coefficients outside the zone might be allocated fewer bits (coarser quantization) or discarded entirely (allocated zero bits).
    * The example image on the right (labeled "bit allocation") illustrates this, showing more bits assigned to the low-frequency coefficients (top-left) and progressively fewer or zero bits to higher-frequency coefficients.

In essence, zonal coding predefines a "zone" of what are assumed to be the most important coefficients (typically low-frequency) and focuses on preserving them, while reducing or eliminating information from coefficients outside this zone.

---
Next is "Threshold Coding." Shall we continue?