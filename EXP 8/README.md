# Experiment 8: Analysis of Multipath Effects (Signal Fluctuation Observation)

[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)

---

---

## 📌 Experiment Metadata
- **Experiment No:** 08
- **Title:** Analysis of Multipath Effects (Signal Fluctuation Observation)
- **Aim:** To observe, measure, and analyze the effect of multipath propagation and small-scale fading on received wireless signal strength (RSSI) across multiple spatial locations in an indoor environment, and demonstrate constructive vs. destructive interference using Python.

---

## 🛠️ Requirements
- **Hardware:** Wi-Fi Access Point / Router, Laptop with Wi-Fi adapter or Android smartphone with Wi-Fi Analyzer, Measuring tape ($10\text{ m}$), Indoor room with obstacles (walls, desks, metal cabinets).
- **Software:** Python 3.x (Google Colab / VS Code)
- **Libraries:** `matplotlib`, `numpy`

---

## 📖 Core Theory & Physics of Multipath

### 1. What is Multipath Propagation?
In a wireless channel, an electromagnetic wave emitted from a transmitter rarely travels strictly along a single unobstructed line-of-sight (LOS) path. Instead, the radio wave encounters environmental objects and undergoes three fundamental electromagnetic phenomena:
1. **Reflection:** Occurs when a radio wave impinges upon an object whose dimensions are large compared to the wavelength (e.g., walls, floors, ceilings, metal doors).
2. **Diffraction:** Occurs when the radio path is obstructed by a sharp edge or dense body, causing waves to bend around the obstacle (Huygens' Principle).
3. **Scattering:** Occurs when waves impinge on objects with dimensions comparable to or smaller than the wavelength, or rough surfaces (e.g., foliage, lampposts, irregular furniture).

```
                             [Ceiling / Wall]
                               /            \ (Reflected Path)
                              /              ▼
     ┌────────────┐          /             ┌────────────┐
     │            ├─────────/─────────────►│            │
     │ Transmitter├───────────────────────►│  Receiver  │
     │            ├───┐   (Direct LOS)     │            │
     └────────────┘   │                    └─────▲──────┘
                       \                         │
                        \──► [Desk / Metal Cabinet]
                             (Scattered Path)
```

As a direct consequence, the receiver's antenna receives **multiple delayed, attenuated, and phase-shifted copies** of the original transmitted signal.

---

### 2. Mathematical Formulation
The composite received signal $r(t)$ is the vector superposition of all $n$ incoming multipath components:
$$r(t) = \sum_{i=1}^{n} A_i(t) \cdot s(t - \tau_i) \cdot \cos(2\pi f_c t + \phi_i)$$

Where:
- $A_i(t)$ = Amplitude of the $i$-th multipath component
- $\tau_i$ = Propagation delay of the $i$-th path ($\tau_i = d_i / c$)
- $\phi_i$ = Phase shift of the $i$-th component ($\phi_i = 2\pi f_c \tau_i$)

### 3. Constructive vs. Destructive Interference (Multipath Fading)
Because each path has a slightly different travel distance ($\Delta d$), the phase difference between arriving waves is:
$$\Delta \phi = \frac{2\pi \cdot \Delta d}{\lambda}$$
For $2.4\text{ GHz}$ Wi-Fi, the wavelength is only $\lambda = \frac{3 \times 10^8}{2.4 \times 10^9} \approx \mathbf{12.5\text{ cm}}$ ($5\text{ inches}$)!
- **Constructive Interference:** When path lengths differ by an integer number of wavelengths ($\Delta \phi = 0, 2\pi, 4\pi$), the waves reinforce each other, producing a **strong signal boost (local peak)**.
- **Destructive Interference:** When path lengths differ by an odd multiple of half-wavelengths ($\Delta \phi = \pi, 3\pi$), the peaks of one wave align with the troughs of another, cancelling each other out and creating a **severe signal null (deep fade)**.

> **Key Discovery:** Moving a laptop or smartphone by merely **a few centimeters** can cause a signal to swing dramatically by **$10\text{ to }20\text{ dBm}$**! This is why the RSSI curve does **not** decrease monotonically with distance in indoor environments.

---

## 📊 Observation Table

Measurements taken in a typical indoor laboratory with desks, walls, and reflecting surfaces:

| Position No. | Distance from Router ($d$) | Measured Average RSSI ($\text{dBm}$) | Interference Behavior | Observation / Explanation |
| :---: | :---: | :---: | :---: | :--- |
| **1** | $1\text{ m}$ | $-45\text{ dBm}$ | Reference | Strong direct LOS path dominates |
| **2** | $2\text{ m}$ | $-49\text{ dBm}$ | Minor destruct | Expected slight distance attenuation |
| **3** | $3\text{ m}$ | **$-43\text{ dBm}$** | **Constructive Peak** | **Stronger than Position 2 despite being farther away!** Reflections reinforce. |
| **4** | $4\text{ m}$ | **$-55\text{ dBm}$** | **Destructive Null** | Severe deep fade caused by out-of-phase wall reflection. |
| **5** | $5\text{ m}$ | $-47\text{ dBm}$ | Constructive | Signal recovers as receiver moves out of the cancellation null. |
| **6** | $6\text{ m}$ | **$-62\text{ dBm}$** | **Deep Fade** | Metal cabinet shadow + multipath cancellation. |
| **7** | $7\text{ m}$ | $-51\text{ dBm}$ | Constructive | Multi-bounce wave reinforces signal. |
| **8** | $8\text{ m}$ | $-46\text{ dBm}$ | Constructive Peak | Significant constructive combination near far wall. |

---

## 💻 Google Colab Code (Copy-Paste Ready)

Copy and run the code below in Google Colab to plot the non-monotonic multipath fluctuations vs. the ideal monotonic path loss curve:

```python
# ==============================================================================
# Experiment 8: Analysis of Multipath Effects (Signal Fluctuation Observation)
# Run this cell directly in Google Colab or Jupyter Notebook
# ==============================================================================

import numpy as np
import matplotlib.pyplot as plt

# Experimental distances (meters) and Measured Average RSSI (dBm)
distance = np.array([1, 2, 3, 4, 5, 6, 7, 8])
rssi_measured = np.array([-45, -49, -43, -55, -47, -62, -51, -46])

# Ideal Theoretical Monotonic Path Loss Model (Free space without multipath)
rssi_ideal = -45 - 20 * np.log10(distance / 1.0)

plt.figure(figsize=(10, 6))

# Plot Measured Multipath Curve (Showing Fading Fluctuations)
plt.plot(distance, rssi_measured, 'ro-', linewidth=2.5, markersize=8, label='Measured RSSI (Multipath Fading Observed)')

# Plot Ideal Monotonic Curve for comparison
plt.plot(distance, rssi_ideal, 'b--', linewidth=1.8, label='Ideal Monotonic Free-Space Loss (No Multipath)')

# Highlight Constructive Peaks and Destructive Fades
plt.annotate('Constructive Peak\n(-43 dBm > Pos 2)', xy=(3, -43), xytext=(3, -36),
             arrowprops=dict(facecolor='green', shrink=0.08, width=1.5, headwidth=7),
             ha='center', fontsize=9, fontweight='bold', color='darkgreen')

plt.annotate('Destructive Deep Fade\n(-62 dBm)', xy=(6, -62), xytext=(6, -69),
             arrowprops=dict(facecolor='red', shrink=0.08, width=1.5, headwidth=7),
             ha='center', fontsize=9, fontweight='bold', color='darkred')

plt.title("Multipath Propagation Effects: Constructive vs. Destructive Fluctuations", fontsize=13, fontweight='bold', pad=12)
plt.xlabel("Distance from Wi-Fi Router (meters)", fontsize=11)
plt.ylabel("Signal Strength (RSSI in dBm)", fontsize=11)

# Note: In wireless diagrams, closer to 0 is at the top (stronger)
plt.ylim(-72, -32)
plt.grid(True, linestyle='--', alpha=0.6)
plt.legend(loc='lower left', framealpha=0.95)

plt.tight_layout()
plt.show()

print("Key Conclusion:")
print("Notice how RSSI at 3m (-43 dBm) is stronger than at 2m (-49 dBm),")
print("and RSSI at 8m (-46 dBm) is far stronger than at 6m (-62 dBm).")
print("This non-monotonic behavior proves constructive & destructive multipath fading!")
```

---

## 🎯 Viva Questions & Answers (Experiment 8)

### Q1. What is multipath propagation?
**Answer:** Multipath propagation is the wireless phenomenon where a transmitted radio signal reaches the receiver antenna by two or more paths simultaneously, caused by reflection, diffraction, and scattering off environmental obstacles.

### Q2. What causes constructive and destructive interference in wireless signals?
**Answer:** When multiple paths arrive at the receiver with different propagation delays, they experience phase shifts. If the phase difference is a multiple of $360^\circ$ ($2\pi$), the wave crests add together to produce **constructive interference** (increased signal). If the phase difference is $180^\circ$ ($\pi$), the crest of one wave cancels the trough of another, causing **destructive interference** (deep fade).

### Q3. Why does the measured signal strength fluctuate instead of smoothly decreasing with distance?
**Answer:** In free space, signal strength decreases smoothly according to the inverse square law ($1/d^2$). But in an enclosed indoor space, the received signal is the vector sum of direct and multiple reflected waves. At certain positions, reflections reinforce the direct wave (higher RSSI), while at other positions centimeters away, reflections cancel it (sharp drop in RSSI).

### Q4. What is the wavelength of a 2.4 GHz Wi-Fi signal, and why is this dimension significant in multipath?
**Answer:**
$$\lambda = \frac{c}{f} = \frac{3 \times 10^8\text{ m/s}}{2.4 \times 10^9\text{ Hz}} = 0.125\text{ m} = \mathbf{12.5\text{ cm}}$$
A phase shift of $180^\circ$ (destructive cancellation) requires a physical path difference of only **half a wavelength ($\lambda/2 \approx 6.25\text{ cm}$ or $2.5\text{ inches}$)**. Moving a phone just a few centimeters can transform a deep fade into a strong signal.

### Q5. What is the difference between Rayleigh Fading and Rician Fading?
**Answer:**
- **Rayleigh Fading:** Occurs when there is **no direct Line-of-Sight (LOS)** path between transmitter and receiver (e.g., deep indoors or dense urban canyons). The signal consists purely of scattered/reflected components.
- **Rician Fading:** Occurs when there is **one dominant Line-of-Sight (LOS)** path plus multiple weaker reflected/scattered components. Characterized by the Rician $K$-factor.

### Q6. What is Small-Scale Fading vs. Large-Scale Fading?
**Answer:**
- **Large-Scale Fading:** Signal attenuation over large distances (hundreds of meters) caused by path loss and shadowing from hills or large buildings.
- **Small-Scale Fading:** Rapid, dramatic fluctuations in signal amplitude and phase over very short distances (fractions of a wavelength) or short time spans (milliseconds) due to multipath interference.

### Q7. How do modern wireless systems (Wi-Fi 6, 4G, 5G) combat multipath fading?
**Answer:**
1. **MIMO (Multiple-Input Multiple-Output):** Multiple antennas exploit spatial diversity.
2. **OFDM (Orthogonal Frequency Division Multiplexing):** Splits wideband signals into many narrow subcarriers, turning frequency-selective fading into flat fading.
3. **Cyclic Prefix (CP):** Absorbs delay spread and eliminates Inter-Symbol Interference (ISI).
4. **Adaptive Equalization and Rake Receivers.**

### Q8. What is Intersymbol Interference (ISI) in multipath propagation?
**Answer:** ISI occurs when delayed multipath copies of a previously transmitted symbol arrive late at the receiver and overlap with the current symbol, corrupting symbol decoding and raising the bit error rate.

### Q9. What is Doppler Shift in mobile multipath channels?
**Answer:** When the mobile user or surrounding reflectors are in motion, the frequency of each multipath component shifts proportionally to velocity:
$$f_d = \frac{v}{\lambda} \cos(\theta)$$
Leading to time-varying channel characteristics (coherence time).

### Q10. What is the key conclusion from this experiment?
**Answer:** Multipath propagation causes spatial signal fluctuations (peaks and deep fades). Signal strength in an indoor environment does not follow a strict monotonic decay with distance due to the vector superposition of reflected, scattered, and diffracted electromagnetic waves.


---
[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)
