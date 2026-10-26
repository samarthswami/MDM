# 📱 Mobile and Digital Communications (MDM) Laboratory

Welcome to the complete reference repository for the **Mobile and Digital Communications (MDM)** laboratory experiments. 

---

## ⚡ Quick Navigation Links
- 📘 **[Master Viva Questions & Answers (All 10 Experiments)](./VIVA_QUESTIONS_AND_ANSWERS.md)**
- 📋 **Individual Experiment Documentation:**
  - **[Experiment 01: Study of EM Spectrum Allocation & Wireless Bands](./EXP%201/README.md)** *(Study / Theory)*
  - **[Experiment 02: Simulation of Basic Communication System (Tx–Channel–Rx)](./EXP%202/README.md)** *(Python Code)*
  - **[Experiment 03: Demonstration of Cellular Frequency Reuse (N=7)](./EXP%203/README.md)** *(Python Code)*
  - **[Experiment 04: Measurement of Signal Strength vs. Distance for Wi-Fi](./EXP%204/README.md)** *(Python Code)*
  - **[Experiment 05: Throughput & Latency Measurement (Ping, Speed Test)](./EXP%205/README.md)** *(Python Code)*
  - **[Experiment 06: Study of Bluetooth Communication (Pairing & Range)](./EXP%206/README.md)** *(Mobile Practical - No Code)*
  - **[Experiment 07: Data Transfer Using Bluetooth & Performance Analysis](./EXP%207/README.md)** *(Python Code)*
  - **[Experiment 08: Analysis of Multipath Effects (Signal Fluctuations)](./EXP%208/README.md)** *(Python Code)*
  - **[Experiment 09: Study of Cellular Network Parameters (2G/3G/4G/5G)](./EXP%209/README.md)** *(Smartphone Practical - No Code)*
  - **[Experiment 10: Wi-Fi Scanning Using Mobile App (WiFi Analyzer)](./EXP%2010/README.md)** *(Smartphone App Practical - No Code)*

---

## 📑 Experiments Categorization (Coding vs. Hands-on Study)

| Exp No. | Experiment Title | Experiment Type | Python Code in Syllabus? |
| :---: | :--- | :--- | :---: |
| **EXP 1** | Study of Electromagnetic Spectrum Allocation | Study / Analytical | ❌ No Code (Study Only) |
| **EXP 2** | Simulation of Basic Communication System | Python Simulation | ✅ **YES (Python Code)** |
| **EXP 3** | Demonstration of Cellular Frequency Reuse | Python Simulation | ✅ **YES (Python Code)** |
| **EXP 4** | Signal Strength vs. Distance for Wi-Fi Router | Measurement & Plotting | ✅ **YES (Python Code)** |
| **EXP 5** | Throughput & Latency Measurement | Network Tools & Plotting | ✅ **YES (Python Code)** |
| **EXP 6** | Study of Bluetooth Communication | Mobile Hands-on | ❌ No Code (Smartphone Pairing/Range) |
| **EXP 7** | Data Transfer Using Bluetooth | Measurement & Plotting | ✅ **YES (Python Code)** |
| **EXP 8** | Analysis of Multipath Effects | Measurement & Plotting | ✅ **YES (Python Code)** |
| **EXP 9** | Study of Cellular Network Parameters | Smartphone Practical | ❌ No Code (Settings / Net Diagnostics) |
| **EXP 10** | Wi-Fi Network Scanning Using Mobile Apps | Mobile App Practical | ❌ No Code (WiFi Analyzer App) |

---

## 🚀 One-Click Google Colab Code Hub (Experiments with Code)

These are the **exact 6 experiments** from your lab manual that require Python code.
Copy and paste each snippet directly into a **Google Colab** code cell and press **Shift + Enter**.

---

### 💻 Experiment 02: Simulation of Basic Communication System (Tx–Channel–Rx)
```python
# ==============================================================================
# Experiment 2: Simulation of Basic Communication System (Tx-Channel-Rx)
# ==============================================================================

import numpy as np
import matplotlib.pyplot as plt

# ----------------------------------------------------
# Part A: Analog Signal Communication System
# ----------------------------------------------------
# 1. Transmitter (Tx)
t = np.linspace(0, 1, 1000)
frequency = 5  # Hz
message = np.sin(2 * np.pi * frequency * t)

# 2. Communication Channel (AWGN Gaussian Noise)
noise = np.random.normal(0, 0.3, len(t))
received_signal = message + noise

# 3. Receiver (Rx)
recovered_signal = received_signal

# Plotting Analog Waveforms
plt.figure(figsize=(10, 7))

plt.subplot(3, 1, 1)
plt.plot(t, message, color='blue')
plt.title("Transmitter: Original Message Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

plt.subplot(3, 1, 2)
plt.plot(t, noise, color='red')
plt.title("Channel: Noise Added by Channel")
plt.xlabel("Time (s)")
plt.ylabel("Noise")
plt.grid(True)

plt.subplot(3, 1, 3)
plt.plot(t, recovered_signal, color='green')
plt.title("Receiver: Received Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)

plt.tight_layout()
plt.show()

# ----------------------------------------------------
# Part B: Digital Signal Communication System
# ----------------------------------------------------
# Transmitter: Generate random binary bits
tx = np.random.randint(0, 2, 20)
signal = 2 * tx - 1  # BPSK symbols: 0 -> -1V, 1 -> +1V

# Channel: Add Gaussian Noise
noise_digital = np.random.normal(0, 0.5, len(signal))
received = signal + noise_digital

# Receiver: Detection threshold at 0V
rx = (received > 0).astype(int)

print("Transmitted Bits :", tx)
print("Received Bits    :", rx)
print("Bit Errors       :", np.sum(tx != rx), "out of 20")

# Plotting Digital Signals
plt.figure(figsize=(10, 7))

plt.subplot(3, 1, 1)
plt.stem(tx)
plt.title("Transmitted Bits")
plt.ylabel("Bit Value")
plt.grid(True)

plt.subplot(3, 1, 2)
plt.plot(received, 'o-')
plt.axhline(0, color='black', linestyle='--')
plt.title("Signal after Channel with Decision Threshold (0V)")
plt.ylabel("Voltage")
plt.grid(True)

plt.subplot(3, 1, 3)
plt.stem(rx)
plt.title("Recovered Bits at Receiver")
plt.xlabel("Bit Index")
plt.ylabel("Bit Value")
plt.grid(True)

plt.tight_layout()
plt.show()
```

---

### 💻 Experiment 03: Demonstration of Cellular Frequency Reuse (N=7 Cluster)
```python
# ==============================================================================
# Experiment 3: Frequency Reuse in Cellular Systems (N=7 Cluster)
# ==============================================================================

import matplotlib.pyplot as plt
from matplotlib.patches import RegularPolygon
import numpy as np

# Hexagon geometry parameters
R = 1.0
dx = 1.5 * R
dy = np.sqrt(3) * R

# Distinct colors for the 7 cells in each cluster
cell_colors = {
    'A': 'red',
    'B': 'blue',
    'C': 'green',
    'D': 'orange',
    'E': 'purple',
    'F': 'cyan',
    'G': 'yellow'
}

# Coordinates of the 7 cells in a single cluster
cluster = [
    (0,       dy,    'B'),   # Top
    (-dx,     dy/2,  'G'),   # Upper-left
    ( dx,     dy/2,  'C'),   # Upper-right
    (0,       0,     'A'),   # Centre
    (-dx,    -dy/2,  'F'),   # Lower-left
    ( dx,    -dy/2,  'D'),   # Lower-right
    (0,      -dy,    'E')    # Bottom
]

# Offsets to position three adjacent 7-cell clusters
cluster_offsets = [
    (0, 0),                  # Upper cluster
    (-3 * R, -2 * dy),       # Lower-left cluster
    (1.5 * R, -2.5 * dy)     # Lower-right cluster
]

# Generate all cells
cells = []
for ox, oy in cluster_offsets:
    for x, y, label in cluster:
        cells.append((x + ox, y + oy, label))

# Draw hexagons
fig, ax = plt.subplots(figsize=(8, 6))

for x, y, label in cells:
    hexagon = RegularPolygon(
        (x, y),
        numVertices=6,
        radius=R,
        orientation=np.radians(30),
        facecolor=cell_colors[label],
        edgecolor='black',
        linewidth=2.0,
        alpha=0.7
    )
    ax.add_patch(hexagon)
    ax.text(x, y, label, ha='center', va='center', fontsize=11, color='black', fontweight='bold')

ax.set_aspect('equal')
ax.set_xlim(-6, 6)
ax.set_ylim(-7.5, 3)
ax.axis('off')

ax.text(-5.1, 2.65, 'Frequency Reuse (Cluster Size N=7)', ha='left', va='top', fontsize=12, fontweight='bold')
plt.tight_layout(pad=0.2)
plt.show()
```

---

### 💻 Experiment 04: Wi-Fi Signal Strength (RSSI) vs. Distance
```python
# ==============================================================================
# Experiment 4: Wi-Fi Signal Strength vs Distance
# ==============================================================================

import matplotlib.pyplot as plt

# Distance from Wi-Fi router (meters)
distance = [1, 2, 4, 6, 8, 10]

# Sample RSSI values (dBm)
rssi = [-35, -42, -50, -57, -63, -68]

plt.figure(figsize=(8, 5))

plt.plot(
    distance,
    rssi,
    marker='o',
    color='blue',
    linewidth=2
)

plt.title("Wi-Fi Signal Strength vs Distance")
plt.xlabel("Distance from Router (m)")
plt.ylabel("Signal Strength (RSSI in dBm)")
plt.grid(True)

# Set y-axis from higher negative values to lower negative values
# Top    -> -30 dBm (stronger)
# Bottom -> -70 dBm (weaker)
plt.ylim(-70, -30)

plt.show()
```

---

### 💻 Experiment 05: Throughput & Latency Measurement
```python
# ==============================================================================
# Experiment 5: Network Performance (Throughput & Latency Plot)
# ==============================================================================

import matplotlib.pyplot as plt

# Sample network data obtained from Ping and Speed Test
parameters = ['Download', 'Upload', 'Latency']
values = [95, 42, 18]

plt.figure(figsize=(7, 5))
plt.bar(parameters, values, color=['#28a745', '#17a2b8', '#ffc107'], edgecolor='black', width=0.5)

# Label values on top of bars
plt.text(0, 97, "95 Mbps", ha='center', fontweight='bold')
plt.text(1, 44, "42 Mbps", ha='center', fontweight='bold')
plt.text(2, 20, "18 ms", ha='center', fontweight='bold')

plt.title("Network Performance")
plt.ylabel("Value (Mbps / ms)")
plt.ylim(0, 115)
plt.grid(axis='y', linestyle='--', alpha=0.7)

plt.show()
```

---

### 💻 Experiment 07: Data Transfer Using Bluetooth & Performance Analysis
```python
# ==============================================================================
# Experiment 7: Bluetooth Data Transfer Performance
# ==============================================================================

import matplotlib.pyplot as plt

# Experimental data
file_size = [1, 5, 10, 25, 50]              # MB
transfer_time = [1.2, 3.5, 7.0, 18.0, 37.0] # seconds

# Calculate transfer rate in Mbps
transfer_rate = []
for size, time in zip(file_size, transfer_time):
    rate = (size * 8) / time
    transfer_rate.append(rate)

# Display tabular results
print("File Size (MB)   Time (s)   Rate (Mbps)")
for size, time, rate in zip(file_size, transfer_time, transfer_rate):
    print(f"{size:10}      {time:6.2f}      {rate:8.2f}")

# Plot Transfer Rate vs File Size
plt.figure(figsize=(8, 5))
plt.plot(
    file_size,
    transfer_rate,
    marker='o',
    color='red',
    linewidth=2
)

plt.xlabel("File Size (MB)")
plt.ylabel("Transfer Rate (Mbps)")
plt.title("Bluetooth Data Transfer Performance")
plt.ylim(0, 15)
plt.grid(True)

plt.show()
```

---

### 💻 Experiment 08: Analysis of Multipath Effects (Signal Fluctuations)
```python
# ==============================================================================
# Experiment 8: Analysis of Multipath Effects
# ==============================================================================

import matplotlib.pyplot as plt
import numpy as np

# Distance from router in metres
distance = np.array([1, 2, 3, 4, 5, 6, 7, 8])

# Average RSSI obtained from experiment
rssi = np.array([-45, -49, -43, -55, -47, -62, -51, -46])

plt.figure(figsize=(9, 5))

plt.plot(
    distance,
    rssi,
    marker='o',
    color='blue',
    linewidth=2
)

plt.xlabel("Distance from Router (m)")
plt.ylabel("Average RSSI (dBm)")
plt.title("Analysis of Multipath Effects")

plt.grid(True)

# Invert y-axis: more negative RSSI = weaker signal (closer to 0 is at top)
plt.gca().invert_yaxis()

plt.show()
```

---

## 🎯 High-Yield Viva Preparation Tips

1. **Which experiments have Python Code?**
   - **Exp 2, 3, 4, 5, 7, and 8** use Python (Matplotlib & NumPy).
   - **Exp 1, 6, 9, and 10** are purely conceptual or mobile hands-on tests (using Android phone, WiFi Analyzer app, and Settings).
2. **Key Concepts to Remember:**
   - **Why are RSSI values negative?** Because received power is in the micro/nanowatt range ($< 1\text{ mW}$), so $\log_{10}(P < 1)$ is negative.
   - **What is Frequency Reuse ($N=7$)?** Reusing channel sets across cells separated by distance $D = R\sqrt{3N}$ to overcome spectrum scarcity.
   - **What causes Multipath fluctuations?** Constructive addition (peaks) and destructive cancellation (nulls) due to reflection, diffraction, and phase differences ($\Delta \phi = 2\pi \Delta d / \lambda$).
   - **Non-overlapping Wi-Fi channels in 2.4 GHz:** Channels **1, 6, and 11**.
   - **Cellular Parameters:** **RSRP** is coverage power of reference signals; **SINR** dictates data rate and modulation.

👉 **For the complete list of 100+ viva questions with answers, open [VIVA_QUESTIONS_AND_ANSWERS.md](./VIVA_QUESTIONS_AND_ANSWERS.md).**
