## 📘 Introduction

This notebook is part of my thesis work in which I study the frequency response of quantum signals generated from the expectation value of the operator $J_{\alpha\beta}$. I use Fast Fourier Transform (FFT) to analyze these signals in the frequency domain and investigate how quantum squeezing affects both the signal-to-noise ratio (SNR) and the accuracy of frequency estimation.

To compare the trade-off between SNR and relative frequency error for various squeezing strengths, I apply **Pareto optimality** analysis. The goal is to identify parameter regions that produce **high SNR** and **low relative error**, helping to determine optimal squeezing configurations in quantum metrology.

---

## 🧭 Steps to Reproduce the Analysis

Follow these steps to understand, reproduce, or extend the results in this notebook:

### **1. Configure the Environment**

Set up the working environment by:

* Creating output directories for storing plots.
* Updating `matplotlib` aesthetics for clean visuals.
* Importing all necessary libraries including `numpy`, `scipy`, `joblib`, and `matplotlib`.

### **2. Define Core Parameters**

Set physical constants and computational parameters:

* Base frequency $\omega_0 = 1.0$
* Spin sizes $J = 10, 100, 1000$
* Squeezing strengths $\chi \in \{0, 0.0999, \ldots, 0.3999\}$
* Sampling and noise parameters:

  * $\sigma = 0.03 \times \omega_0$ (3% frequency noise)
  * $\text{dsn} = 10000$ (noisy samples)

### **3. Define the Expectation Function**

The time-domain signal is generated using:

```python
jalphabeta(alpha, beta, chi, omega, t, J)
```

This models the expectation value of $J_{\alpha\beta}$, incorporating the squeezing factor $\chi$ and phase terms $\alpha$, $\beta$.

### **4. Generate FFT for a Signal**

Use the function `gfft_sig()` to:

* Simulate time-domain data.
* Apply zero-padding for better frequency resolution.
* Compute the FFT and return the power spectrum and frequency range.

### **5. Simulate FFT with Noise**

With `data_fft()`:

* Add Gaussian noise to the frequency.
* Compute FFT for each noisy realization.
* Collect all FFT results for statistical analysis.

### **6. Process Parameter Combinations**

Using `process_combination()`:

* Loop over all combinations of $J, \chi, \alpha, \beta$.
* Compute:

  * Average FFT
  * Standard deviation across trials
  * Interpolated peak frequency
  * SNR at peak
  * Relative error in frequency

### **7. Run in Parallel**

To speed up execution:

* Use `joblib.Parallel` with multiprocessing.
* Collect results from all parameter combinations in parallel using all CPU cores.

### **8. Identify Pareto Optimal Points**

From the collected results:

* For each $J, \alpha, \beta$, find baseline performance at $\chi = 0$.
* Select points with $\chi > 0$ where:

  * SNR > baseline SNR
  * Relative error < baseline error
* These points form the Pareto optimal set.

### **9. Visualize the Results**

Plot the Pareto front:

* Relative Error (x-axis) vs SNR (y-axis)
* Color-coded by squeezing strength $\chi$
* Save as PDF for inclusion in publications or reports

### **10. Output Summary Statistics**

Print:

* Number of Pareto optimal points
* Min, max, and average SNR values
* Min, max, and average relative frequency errors

This helps understand the effectiveness of squeezing under different regimes and parameter choices.

---

## 📂 Output

All generated plots are saved in the `pareto_plots` directory.

---

## 📌 Notes

* This simulation is computationally intensive. On most machines, full execution may take several minutes to an hour depending on available cores.
* You can modify the resolution of `alpha_values` and `beta_values` or reduce `dsn` to speed up preliminary tests.

