# Experiment 4: Measurement of Signal Strength vs. Distance for Wi-Fi Router

[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)

---

---

## 📌 Experiment Metadata
- **Experiment No:** 04
- **Title:** Measurement of Signal Strength vs. Distance for Wi-Fi Router
- **Aim:** To measure and analyze the variation of Wi-Fi Received Signal Strength Indicator (RSSI) with increasing distance from a Wi-Fi router, understand the logarithmic path loss model in wireless propagation, and plot RSSI vs. Distance using Python.

---

## 🛠️ Requirements
- **Hardware:** Wi-Fi Router / Mobile Hotspot, Laptop with Wi-Fi adapter or Android Smartphone with Wi-Fi Analyzer app, Measuring tape ($10-20\text{ m}$).
- **Software:** Python 3.x (Google Colab / Jupyter Notebook / VS Code)
- **Libraries:** `numpy`, `matplotlib`

---

## 📖 Core Theory & Principles

### 1. What is RSSI and Why is it in dBm?
- **RSSI (Received Signal Strength Indicator):** An arbitrary or standardized measurement of the RF power level received by a wireless receiver's antenna.
- It is universally expressed in **dBm** (decibels referenced to 1 milliwatt):
  $$\text{Power (dBm)} = 10 \log_{10}\left(\frac{P_{\text{mW}}}{1 \text{ mW}}\right)$$

```
  0 dBm   = 1 mW
-30 dBm   = 0.001 mW (1 µW)      ---> Excellent signal (right next to router)
-60 dBm   = 0.000001 mW (1 nW)   ---> Good, reliable Wi-Fi streaming
-70 dBm   = 0.1 nW               ---> Fair, minimum for voice/video calls
-80 dBm   = 0.01 nW              ---> Weak, high packet loss, slow surfing
-90 dBm   = 0.001 nW (1 pW)      ---> Disconnection boundary / unusable noise floor
```

> **Remember for Viva:** RSSI values are **negative**. A value closer to zero (e.g., $-35\text{ dBm}$) represents a **stronger signal**, whereas a more negative number (e.g., $-75\text{ dBm}$) represents a **weaker signal**.

---

### 2. Log-Distance Path Loss Propagation Model
As radio waves propagate through the atmosphere, signal power radiates outward over an expanding spherical wavefront, causing attenuation according to the **Inverse Square Law** ($P_r \propto 1/d^2$ in free space).

In practical indoor environments (subject to walls, reflections, and shadowing), the received power follows the **Log-Distance Path Loss Model**:
$$\text{RSSI}(d) = \text{RSSI}(d_0) - 10 \cdot n \cdot \log_{10}\left(\frac{d}{d_0}\right)$$

Where:
- $d$ = Distance from the transmitter / router (in meters)
- $d_0$ = Reference distance (typically $1\text{ meter}$)
- $\text{RSSI}(d_0)$ = Received signal strength measured at reference distance $d_0$ (e.g., $-35\text{ dBm}$)
- $n$ = **Path Loss Exponent**, which depends on the physical environment:
  - Free space (ideal LOS): $n = 2.0$
  - Urban outdoor cellular: $n = 2.7 - 3.5$
  - Indoor office with drywall/furniture: $n = 3.0 - 4.5$
  - Dense industrial environment with metal/concrete walls: $n = 4.0 - 6.0$

---

## 📊 Sample Observation Table

| Reading No. | Distance from Router $d$ (meters) | Measured RSSI ($\text{dBm}$) | Signal Quality Level | Observed Throughput / Experience |
| :---: | :---: | :---: | :---: | :---: |
| 1 | $1\text{ m}$ | $-35\text{ dBm}$ | Excellent | Maximum speed, zero latency |
| 2 | $2\text{ m}$ | $-42\text{ dBm}$ | Very Good | Full HD/4K streaming, smooth web |
| 3 | $4\text{ m}$ | $-50\text{ dBm}$ | Good | Stable connection, low jitter |
| 4 | $6\text{ m}$ | $-57\text{ dBm}$ | Fair | Moderate speed, slight packet retransmission |
| 5 | $8\text{ m}$ | $-63\text{ dBm}$ | Fair to Weak | Noticeable drop in speed |
| 6 | $10\text{ m}$ | $-68\text{ dBm}$ | Weak | Frequent buffering, rate drops |

---

## 💻 Google Colab Code (Copy-Paste Ready)

Copy and run the code below in Google Colab to plot RSSI vs. Distance with logarithmic model fitting and quality zones:

```python
# ==============================================================================
# Experiment 4: Wi-Fi Signal Strength (RSSI) vs Distance Measurement
# Run this cell directly in Google Colab or Jupyter Notebook
# ==============================================================================

import numpy as np
import matplotlib.pyplot as plt

# Measured Experimental Data
distance = np.array([1, 2, 4, 6, 8, 10])                       # Meters
rssi = np.array([-35, -42, -50, -57, -63, -68])                # Measured RSSI in dBm

# Fit a Log-Distance Path Loss Model: RSSI(d) = RSSI_0 - 10 * n * log10(d)
# Equivalent to linear regression: y = a * x + b where x = log10(d)
x_log = np.log10(distance)
slope, intercept = np.polyfit(x_log, rssi, 1)
estimated_n = -slope / 10.0   # Path loss exponent

# Generate smooth curve for fitted model
d_smooth = np.linspace(1, 10, 100)
rssi_fitted = intercept + slope * np.log10(d_smooth)

# Create Plot
plt.figure(figsize=(10, 6))

# Highlight Signal Quality Zones with background color bands
plt.axhspan(-40, -30, color='#D4EDDA', alpha=0.6, label='Excellent (-30 to -40 dBm)')
plt.axhspan(-60, -40, color='#D1ECF1', alpha=0.6, label='Good (-40 to -60 dBm)')
plt.axhspan(-70, -60, color='#FFF3CD', alpha=0.6, label='Fair (-60 to -70 dBm)')
plt.axhspan(-90, -70, color='#F8D7DA', alpha=0.6, label='Weak (-70 to -90 dBm)')

# Plot measured points and fitted line
plt.plot(distance, rssi, 'ro', markersize=8, markeredgecolor='black', label='Measured RSSI Readings')
plt.plot(d_smooth, rssi_fitted, 'b--', linewidth=2, label=f'Log Path Loss Fit (n={estimated_n:.2f})')

# Annotate each measured point
for d, r in zip(distance, rssi):
    plt.annotate(f"{r} dBm", (d, r), textcoords="offset points", xytext=(0, 10),
                 ha='center', fontsize=9, fontweight='bold', color='darkred')

plt.title("Wi-Fi Signal Strength (RSSI) vs. Distance from Access Point", fontsize=13, fontweight='bold', pad=12)
plt.xlabel("Distance from Wi-Fi Router (meters)", fontsize=11)
plt.ylabel("Received Signal Strength (RSSI in dBm)", fontsize=11)
plt.xlim(0.5, 10.5)
plt.ylim(-75, -28)
plt.grid(True, linestyle=':', alpha=0.7)
plt.legend(loc='lower left', framealpha=0.9)

plt.tight_layout()
plt.show()

print(f"Calculated Path Loss Exponent (n): {estimated_n:.2f}")
print(f"Reference Signal RSSI at 1 meter: {intercept:.1f} dBm")
```

---

## 🎯 Viva Questions & Answers (Experiment 4)

### Q1. What is RSSI and in which unit is it measured?
**Answer:** RSSI stands for **Received Signal Strength Indicator**. It is measured in **dBm** (decibels relative to 1 milliwatt).

### Q2. Why is RSSI typically a negative number?
**Answer:** Because the received power of a wireless signal is typically in the microwatt ($\mu\text{W}$), nanowatt ($\text{nW}$), or picowatt ($\text{pW}$) range—far less than $1\text{ milliwatt}$ ($1\text{ mW}$). Since $\log_{10}$ of any fraction between 0 and 1 is negative, the resulting $\text{dBm}$ value is negative.

### Q3. Which signal is stronger: $-45\text{ dBm}$ or $-75\text{ dBm}$?
**Answer:** **$-45\text{ dBm}$ is much stronger**. In logarithmic terms, $-45\text{ dBm}$ is $30\text{ dB}$ higher than $-75\text{ dBm}$, which corresponds to $10^{30/10} = \mathbf{1000\text{ times}}$ more received signal power!

### Q4. What is the Log-Distance Path Loss Model formula?
**Answer:**
$$\text{RSSI}(d) = \text{RSSI}(d_0) - 10 \cdot n \cdot \log_{10}\left(\frac{d}{d_0}\right)$$
Where $d$ is distance, $d_0$ is reference distance (usually $1\text{ m}$), and $n$ is the path loss exponent.

### Q5. What is the value of the path loss exponent ($n$) in free space vs. an indoor environment?
**Answer:**
- In **free space (ideal LOS)**: $n = 2.0$.
- In **indoor office / home environments**: $n = 3.0 \text{ to } 5.0$, due to reflection, diffraction, and attenuation from walls, doors, and furniture.

### Q6. What factors contribute to the rapid drop of Wi-Fi signals indoors?
**Answer:**
1. **Free-space path loss** (spreading of the electromagnetic wavefront).
2. **Absorption/penetration loss** through concrete, brick, drywall, and glass.
3. **Multipath fading** caused by destructive wave interference from reflecting surfaces.
4. **Interference** from other 2.4 GHz devices (Bluetooth, microwave ovens).

### Q7. How can you check Wi-Fi signal strength on a Windows laptop without third-party software?
**Answer:** Open Command Prompt (`cmd`) and type:
```cmd
netsh wlan show interfaces
```
This displays the Wi-Fi interface details, including SSID, BSSID, Radio type, Channel, and Signal percentage ($0\% - 100\%$).

### Q8. What is the relationship between Wi-Fi signal strength and data transfer rate (throughput)?
**Answer:** As RSSI degrades, the Signal-to-Noise Ratio (SNR) drops. The Wi-Fi adapter automatically down-negotiates to lower-order modulation schemes (e.g., from 64-QAM / 256-QAM down to 16-QAM or QPSK/BPSK) to prevent packet errors, which reduces the effective physical data rate (throughput).

### Q9. What RSSI threshold is considered the minimum acceptable level for seamless video calls?
**Answer:** Approximately **$-67\text{ dBm}$**. Signals weaker than $-70\text{ dBm}$ will experience jitter, packet drops, video freezing, and audio stuttering.

### Q10. What is Friis Transmission Equation?
**Answer:**
$$P_r = P_t \cdot G_t \cdot G_r \cdot \left(\frac{\lambda}{4\pi d}\right)^2$$
It states that received power $P_r$ is directly proportional to transmitter power $P_t$, antenna gains $G_t$ and $G_r$, and the square of the wavelength $\lambda^2$, while inversely proportional to the square of the distance $d^2$.


---
[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)
