# Experiment 2: Simulation of Basic Communication System (Tx–Channel–Rx) Using Python

[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)

---

---

## 📌 Experiment Metadata
- **Experiment No:** 02
- **Title:** Simulation of Basic Communication System (Tx–Channel–Rx) Using Python
- **Aim:** To simulate the fundamental blocks of a communication system (Transmitter, Physical Channel with AWGN noise, and Receiver) for both analog and digital signals using Python, and analyze the effect of noise on signal reception.

---

## 🛠️ Requirements
- **Hardware:** Desktop / Laptop
- **Software:** Python 3.x (Google Colab / Jupyter Notebook / VS Code)
- **Libraries:** `numpy`, `matplotlib`

---

## 📖 Core Theory & Architecture

A basic communication system transmits information from an information source to a sink (destination) through three primary subsystems:

```
  ┌─────────────────┐       ┌──────────────────────┐       ┌─────────────────┐
  │   Transmitter   │  s(t) │ Communication Channel│  r(t) │    Receiver     │
  │ (Source, Filter,├──────►│    (Free Space /     ├──────►│ (Detector, LPF, ├──► Output
  │   Modulator)    │       │     Cable + AWGN)    │       │   Demodulator)  │
  └─────────────────┘       └──────────┬───────────┘       └─────────────────┘
                                       ▲
                                       │ Noise n(t)
                                 ┌─────┴─────┐
                                 │ AWGN Noise│
                                 └───────────┘
```

### 1. Transmitter (Tx)
- **Role:** Converts the raw message information into an electrical or optical signal suited for transmission over the channel.
- **Key Functions:** Information digitization/sampling, source encoding, channel encoding (adding error-correcting parity bits), and carrier modulation.
- **In our simulation:**
  - *Analog signal:* A pure sinusoidal wave $s(t) = A \sin(2\pi f t)$.
  - *Digital signal:* A random binary sequence $\{0, 1\}$ mapped to bipolar NRZ pulses $\{-1, +1\}$.

---

### 2. Communication Channel
- **Role:** The physical medium connecting Tx and Rx (e.g., twisted pair, coaxial cable, optical fiber, or wireless free-space).
- **Channel Impairments:**
  1. **Attenuation:** Signal power decreases with distance.
  2. **Distortion:** Unequal frequency response of the channel.
  3. **Noise (AWGN):** Unwanted random electrical disturbances.
- **What is AWGN?**
  - **Additive:** The noise adds directly to the signal: $r(t) = s(t) + n(t)$.
  - **White:** Flat power spectral density (contains all frequencies equally, similar to white light).
  - **Gaussian:** Amplitudes follow a normal (Gaussian) probability distribution with mean $\mu = 0$ and variance $\sigma^2$:
    $$P(x) = \frac{1}{\sqrt{2\pi \sigma^2}} \exp\left(-\frac{(x - \mu)^2}{2\sigma^2}\right)$$

---

### 3. Receiver (Rx)
- **Role:** Extracts and reconstructs the transmitted message from the degraded, noisy received waveform.
- **Key Functions:** Low-Noise Amplification (LNA), bandpass filtering, demodulation, symbol synchronization, threshold detection/decision slicing:
  $$\hat{b} = \begin{cases} 1, & \text{if } r(t) > 0 \\ 0, & \text{if } r(t) \le 0 \end{cases}$$
- **Evaluation Metric:** **Bit Error Rate (BER)**:
  $$\text{BER} = \frac{\text{Number of Bits in Error}}{\text{Total Transmitted Bits}}$$

---

## 💻 Google Colab Code (Copy-Paste Ready)

This single script runs both **Part 1 (Analog Communication)** and **Part 2 (Digital Binary Communication with Detection and Bit Error Counting)**. Copy and paste into a single Colab code cell:

```python
# ==============================================================================
# Experiment 2: Basic Communication System Simulation (Tx - Channel - Rx)
# Tested and 100% working in Google Colab / Jupyter
# ==============================================================================

import numpy as np
import matplotlib.pyplot as plt

# ------------------------------------------------------------------------------
# PART 1: Analog Signal Communication System (Sine Wave + AWGN)
# ------------------------------------------------------------------------------
t = np.linspace(0, 1, 1000)      # Time vector: 1 second sampled at 1000 Hz
f_msg = 5                        # Message frequency = 5 Hz
message = np.sin(2 * np.pi * f_msg * t)

# Channel: Introduce Additive White Gaussian Noise
noise_std = 0.35                 # Standard deviation of Gaussian noise
awgn_noise = np.random.normal(0, noise_std, len(t))
received_analog = message + awgn_noise

# Plotting Part 1
fig1, axes = plt.subplots(3, 1, figsize=(10, 7), sharex=True)

axes[0].plot(t, message, color='blue', linewidth=1.8)
axes[0].set_title("Transmitter (Tx): Original Analog Message Signal (5 Hz Sine)")
axes[0].set_ylabel("Amplitude")
axes[0].grid(True)

axes[1].plot(t, awgn_noise, color='red', alpha=0.7, linewidth=0.8)
axes[1].set_title(f"Channel: Additive White Gaussian Noise (AWGN, σ={noise_std})")
axes[1].set_ylabel("Noise Amplitude")
axes[1].grid(True)

axes[2].plot(t, received_analog, color='darkgreen', linewidth=1.2)
axes[2].set_title("Receiver (Rx): Received Noisy Signal [s(t) + n(t)]")
axes[2].set_xlabel("Time (seconds)")
axes[2].set_ylabel("Amplitude")
axes[2].grid(True)

plt.tight_layout()
plt.show()

# ------------------------------------------------------------------------------
# PART 2: Digital Signal Communication System (Binary Bits + Decision Threshold)
# ------------------------------------------------------------------------------
np.random.seed(42)               # Seed for reproducible results
num_bits = 20
tx_bits = np.random.randint(0, 2, num_bits)

# Bipolar NRZ Signaling: Bit 0 -> -1V, Bit 1 -> +1V
bipolar_tx = 2 * tx_bits - 1

# Channel with AWGN Noise
channel_noise = np.random.normal(0, 0.6, num_bits)
received_waveform = bipolar_tx + channel_noise

# Receiver: Decision Threshold at 0V
rx_recovered_bits = (received_waveform > 0).astype(int)

# Calculate Bit Errors
bit_errors = np.sum(tx_bits != rx_recovered_bits)
ber = bit_errors / num_bits

print("="*60)
print(f"Transmitted Bits : {tx_bits}")
print(f"Received Bits    : {rx_recovered_bits}")
print(f"Bit Errors       : {bit_errors} out of {num_bits} bits")
print(f"Bit Error Rate   : {ber:.2%}")
print("="*60)

# Plotting Part 2
fig2, (ax_tx, ax_ch, ax_rx) = plt.subplots(3, 1, figsize=(10, 7))

ax_tx.stem(range(num_bits), tx_bits, linefmt='b-', markerfmt='bo', basefmt='k-')
ax_tx.set_title(f"Transmitter (Tx): {num_bits} Digital Transmitted Bits")
ax_tx.set_ylabel("Bit Value")
ax_tx.set_yticks([0, 1])
ax_tx.grid(True)

ax_ch.plot(range(num_bits), received_waveform, 'ro-', linewidth=1.5, label='Noisy Level')
ax_ch.axhline(0, color='black', linestyle='--', linewidth=1.2, label='Threshold (0V)')
ax_ch.set_title("Channel: Received Signal with Noise vs Decision Threshold")
ax_ch.set_ylabel("Voltage Level")
ax_ch.legend(loc='upper right')
ax_ch.grid(True)

ax_rx.stem(range(num_bits), rx_recovered_bits, linefmt='g-', markerfmt='go', basefmt='k-')
# Highlight any errors with red circle
error_indices = np.where(tx_bits != rx_recovered_bits)[0]
if len(error_indices) > 0:
    ax_rx.scatter(error_indices, rx_recovered_bits[error_indices], color='red', s=120, zorder=5, label='Bit Error!')
    ax_rx.legend(loc='upper right')
ax_rx.set_title(f"Receiver (Rx): Recovered Bits (Bit Error Rate = {ber:.1%})")
ax_rx.set_xlabel("Bit Index")
ax_rx.set_ylabel("Bit Value")
ax_rx.set_yticks([0, 1])
ax_rx.grid(True)

plt.tight_layout()
plt.show()
```

---

## 🎯 Viva Questions & Answers (Experiment 2)

### Q1. What are the three fundamental components of any communication system?
**Answer:**
1. **Transmitter (Tx):** Modulates and shapes the information signal for transmission.
2. **Channel:** The physical transmission medium (wireless free space or wired copper/fiber).
3. **Receiver (Rx):** Detects, filters, and demodulates the signal to recover the original message.

### Q2. What does AWGN stand for, and what does each letter mean?
**Answer:**
- **A (Additive):** The noise adds algebraically to the transmitted signal ($r(t) = s(t) + n(t)$).
- **W (White):** The noise has a constant/flat power spectral density across all frequencies ($S_n(f) = \frac{N_0}{2}$).
- **G (Gaussian):** The instantaneous amplitude of the noise follows a Gaussian (normal) probability density function.
- **N (Noise):** Unwanted, unavoidable random electrical energy.

### Q3. Why is Gaussian distribution commonly used to model physical channel noise?
**Answer:** By the **Central Limit Theorem**, when a large number of independent random microscopic physical phenomena combine (such as thermal vibrations of billions of electrons in conductors and atmospheric interactions), their cumulative sum converges to a Gaussian distribution.

### Q4. What is Signal-to-Noise Ratio (SNR) and how is it expressed mathematically?
**Answer:** SNR measures the ratio of desired signal power to background noise power:
$$\text{SNR} = \frac{P_{\text{signal}}}{P_{\text{noise}}}$$
In decibels (dB):
$$\text{SNR}_{\text{dB}} = 10 \log_{10}\left(\frac{P_{\text{signal}}}{P_{\text{noise}}}\right)$$
Higher SNR means clearer communication with fewer errors.

### Q5. In the digital simulation, how does the receiver decide whether a received symbol was bit 0 or bit 1?
**Answer:** Using a **Threshold Detector (decision slicer)**. In bipolar signaling where bit 1 is $+1\text{V}$ and bit 0 is $-1\text{V}$, the optimal decision boundary is midway at $0\text{V}$. If $r(t) > 0$, the receiver decides bit 1; if $r(t) \le 0$, it decides bit 0.

### Q6. What causes bit errors in a digital communication receiver?
**Answer:** When the magnitude of negative noise exceeds the positive signal amplitude (e.g., transmitted $+1\text{V}$ plus noise $-1.4\text{V}$ equals $-0.4\text{V}$), the received voltage crosses the $0\text{V}$ decision threshold into the negative region, falsely causing the receiver to detect a bit 0.

### Q7. What is Bit Error Rate (BER)?
**Answer:** BER is the ratio of incorrectly received bits to the total number of transmitted bits over a given time interval:
$$\text{BER} = \frac{\text{Bit Errors}}{\text{Total Bits}}$$

### Q8. What is the difference between Baseband transmission and Passband transmission?
**Answer:**
- **Baseband Transmission:** Sending signals at their native low frequencies without carrier modulation (e.g., Ethernet cables, USB).
- **Passband Transmission:** Modulating a high-frequency sinusoidal carrier wave (e.g., ASK, FSK, PSK, QAM) to propagate through wireless media or over long distances.

### Q9. How can we reduce bit errors in a physical communication channel without increasing transmitter power?
**Answer:**
1. Using **Forward Error Correction (FEC)** channel codes (such as Hamming codes, Convolutional codes, Turbo codes, or LDPC).
2. Using **Matched Filters** or equalizers at the receiver.
3. Using higher-order diversity schemes (e.g., spatial diversity antennas / MIMO).

### Q10. What Python libraries were used in this simulation and what are their specific roles?
**Answer:**
- `numpy`: Generates numerical time vectors, mathematical functions (sine), random bit arrays, and Gaussian random distributions (`np.random.normal`).
- `matplotlib.pyplot`: Plots analog waveforms, time series, and discrete stem plots for visualization.


---
[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)
