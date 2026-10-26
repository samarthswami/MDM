# Experiment 7: Data Transfer Using Bluetooth and Analysis of Performance

[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)

---

---

## 📌 Experiment Metadata
- **Experiment No:** 07
- **Title:** Data Transfer Using Bluetooth and Analysis of Performance
- **Aim:** To transfer files of varying sizes between two Bluetooth-enabled mobile devices, measure the transfer elapsed time, calculate the effective data transfer rate (throughput) in both $\text{MB/s}$ and $\text{Mbps}$, and analyze performance trends using Python.

---

## 🛠️ Requirements
- **Hardware:** Two Bluetooth-enabled smartphones / laptops, Digital stopwatch (or built-in phone timer), Test files of defined sizes ($1\text{ MB}, 5\text{ MB}, 10\text{ MB}, 25\text{ MB}, 50\text{ MB}$).
- **Software:** Bluetooth File Transfer / Share via Bluetooth (OBEX Profile), Python 3.x (Google Colab / VS Code).
- **Libraries:** `matplotlib`, `numpy`

---

## 📖 Core Theory & Mathematical Formulations

### 1. Bluetooth File Transfer Protocol Stack (OBEX & OPP)
Bluetooth file transfers between mobile devices rely on:
- **OBEX (Object Exchange):** A high-level binary communications protocol designed to exchange arbitrary data objects (files, contact vCards, photos).
- **OPP (Object Push Profile):** The specialized Bluetooth profile built on top of OBEX and RFCOMM that enables a mobile device to "push" a file directly to another device without requiring a complex file-system browsing session.

```
  ┌───────────────────────────────────────────────┐
  │         Application Layer (Share File)        │
  ├───────────────────────────────────────────────┤
  │    OPP Profile (Object Push) / OBEX Protocol  │
  ├───────────────────────────────────────────────┤
  │         RFCOMM (Serial Port Emulation)        │
  ├───────────────────────────────────────────────┤
  │      L2CAP (Logical Link Control/Adaptation)  │
  ├───────────────────────────────────────────────┤
  │     Baseband & Controller (ACL Packets)       │
  ├───────────────────────────────────────────────┤
  │       Physical Layer (2.4 GHz ISM Radio)      │
  └───────────────────────────────────────────────┘
```

---

### 2. Mathematical Performance Formulas

#### A. Transfer Rate in Megabytes per Second ($\text{MB/s}$)
$$\text{Transfer Rate (MB/s)} = \frac{\text{File Size (MB)}}{\text{Transfer Time (seconds)}}$$

#### B. Transfer Rate in Megabits per Second ($\text{Mbps}$)
Since $1\text{ Byte} = 8\text{ bits}$:
$$\text{Transfer Rate (Mbps)} = \frac{\text{File Size (MB)} \times 8}{\text{Transfer Time (seconds)}} = \text{Rate (MB/s)} \times 8$$

*Example:*
If a $25\text{ MB}$ video takes $18.0\text{ seconds}$ to transfer:
$$\text{Rate (MB/s)} = \frac{25}{18.0} = 1.389\text{ MB/s}$$
$$\text{Rate (Mbps)} = 1.389 \times 8 = 11.11\text{ Mbps}$$

---

### 3. Theoretical vs. Practical Bluetooth Throughput
Why is practical throughput lower than the advertised physical link rate?
1. **Protocol Overhead:** Data must be packaged into L2CAP frames, RFCOMM frames, and baseband ACL (Asynchronous Connection-Less) packets, each with preamble, headers, CRC checksums, and trailer bytes.
2. **Half-Duplex TDD (Time Division Duplexing):** Master and slave take turns transmitting in alternating $625\text{ \mu s}$ time slots.
3. **Packet Retransmissions (ARQ):** Any packet corrupted by 2.4 GHz interference or fading must be acknowledged negatively and re-transmitted, consuming channel time.
4. **Initial Handshake Latency:** Opening the OBEX session, user confirmation, and filesystem storage flush introduce fixed delays that disproportionately lower the measured rate for small files.

---

## 📊 Observation Table

| Reading No. | File Type | File Size ($S$) [MB] | Transfer Time ($T$) [s] | Transfer Rate [MB/s] | Effective Rate [Mbps] |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 1 | Text Document | $1\text{ MB}$ | $1.20\text{ s}$ | $0.83\text{ MB/s}$ | $\mathbf{6.67\text{ Mbps}}$ |
| 2 | High-Res Photo | $5\text{ MB}$ | $3.50\text{ s}$ | $1.43\text{ MB/s}$ | $\mathbf{11.43\text{ Mbps}}$ |
| 3 | MP3 Audio | $10\text{ MB}$ | $7.00\text{ s}$ | $1.43\text{ MB/s}$ | $\mathbf{11.43\text{ Mbps}}$ |
| 4 | Short Video | $25\text{ MB}$ | $18.00\text{ s}$ | $1.39\text{ MB/s}$ | $\mathbf{11.11\text{ Mbps}}$ |
| 5 | Full HD Video | $50\text{ MB}$ | $37.00\text{ s}$ | $1.35\text{ MB/s}$ | $\mathbf{10.81\text{ Mbps}}$ |

> **Key Observation:** For very small files ($1\text{ MB}$), the transfer rate appears lower ($6.67\text{ Mbps}$) due to connection setup and user confirmation overhead. For files $\ge 5\text{ MB}$, throughput stabilizes around **$11\text{ Mbps}$**, representing the steady-state bottleneck of the device's Bluetooth EDR/HS link.

---

## 💻 Google Colab Code (Copy-Paste Ready)

Copy and run the code below in Google Colab to compute throughput and plot performance:

```python
# ==============================================================================
# Experiment 7: Bluetooth Data Transfer Performance Analysis
# Run this cell directly in Google Colab or Jupyter Notebook
# ==============================================================================

import numpy as np
import matplotlib.pyplot as plt

# Measured Experimental Data
file_size_MB = np.array([1, 5, 10, 25, 50])                # In Megabytes (MB)
transfer_time_sec = np.array([1.2, 3.5, 7.0, 18.0, 37.0])  # In Seconds

# Compute Rates
rate_MBps = file_size_MB / transfer_time_sec
rate_Mbps = rate_MBps * 8.0

# Print Tabular Summary to Console
print("="*65)
print(f"{'File Size (MB)':<16} {'Time (s)':<12} {'Rate (MB/s)':<16} {'Rate (Mbps)':<12}")
print("="*65)
for s, t, r_mb, r_mbps in zip(file_size_MB, transfer_time_sec, rate_MBps, rate_Mbps):
    print(f"{s:<16} {t:<12.2f} {r_mb:<16.2f} {r_mbps:<12.2f}")
print("="*65)

# Plotting Performance Curves
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))

# Subplot 1: File Size vs Transfer Time (Linear growth)
ax1.plot(file_size_MB, transfer_time_sec, 'bo-', linewidth=2, markersize=7)
for s, t in zip(file_size_MB, transfer_time_sec):
    ax1.annotate(f"{t}s", (s, t), textcoords="offset points", xytext=(0, 8), ha='center', fontsize=9, fontweight='bold')
ax1.set_title("File Size vs. Transfer Time (Direct Relationship)", fontsize=12, fontweight='bold')
ax1.set_xlabel("File Size (MB)", fontsize=11)
ax1.set_ylabel("Transfer Time (seconds)", fontsize=11)
ax1.grid(True, linestyle='--', alpha=0.6)

# Subplot 2: File Size vs Throughput (Saturation curve)
ax2.plot(file_size_MB, rate_Mbps, 'ro-', linewidth=2, markersize=7, color='#D9534F')
for s, r in zip(file_size_MB, rate_Mbps):
    ax2.annotate(f"{r:.1f} Mbps", (s, r), textcoords="offset points", xytext=(0, 8), ha='center', fontsize=9, fontweight='bold')
ax2.axhline(np.mean(rate_Mbps[1:]), color='darkblue', linestyle=':', label=f'Steady State Avg (~{np.mean(rate_Mbps[1:]):.1f} Mbps)')
ax2.set_title("Bluetooth Effective Transfer Rate vs. File Size", fontsize=12, fontweight='bold')
ax2.set_xlabel("File Size (MB)", fontsize=11)
ax2.set_ylabel("Transfer Rate (Mbps)", fontsize=11)
ax2.set_ylim(0, 16)
ax2.grid(True, linestyle='--', alpha=0.6)
ax2.legend(loc='lower right')

plt.suptitle("Bluetooth File Transfer Performance Analysis", fontsize=14, fontweight='bold', y=1.02)
plt.tight_layout()
plt.show()
```

---

## 🎯 Viva Questions & Answers (Experiment 7)

### Q1. What is the formula to convert data transfer rate from $\text{MB/s}$ to $\text{Mbps}$?
**Answer:**
$$\text{Rate in Mbps} = \text{Rate in MB/s} \times 8$$
Because 1 byte consists of 8 bits.

### Q2. What Bluetooth Profile is specifically responsible for file transfers between mobile devices?
**Answer:** The **OPP (Object Push Profile)**, built on top of the **OBEX (Object Exchange)** protocol.

### Q3. Why is the observed transfer rate for a small 1 MB file lower than for a 10 MB file?
**Answer:** Because fixed initial setup latencies (OBEX session initialization, connection handshakes, notification display, and user acceptance) represent a large percentage of total elapsed time for small files. For larger files, this fixed overhead becomes negligible compared to the total steady-state data streaming time.

### Q4. What is the theoretical maximum speed of Bluetooth 2.0 with EDR (Enhanced Data Rate)?
**Answer:** Bluetooth 2.0 + EDR has a theoretical physical link rate of **$3.0\text{ Mbps}$**, with a practical usable payload throughput of approximately **$2.1\text{ Mbps}$**.

### Q5. What is the theoretical maximum data rate of Bluetooth 5.0?
**Answer:** Bluetooth 5.0 supports up to **$2\text{ Mbps}$** in low-energy (BLE) mode and up to **$24 - 50\text{ Mbps}$** when utilizing Bluetooth High Speed (over an 802.11 Wi-Fi radio link).

### Q6. What causes fluctuations or drops in Bluetooth throughput during file transfer?
**Answer:**
1. **Physical distance:** As distance increases, signal attenuation lowers SNR, triggering lower modulation orders.
2. **Obstacles:** Walls or human bodies in the line of sight.
3. **2.4 GHz RF Interference:** Nearby Wi-Fi traffic or microwave ovens causing packet collisions and retransmissions.
4. **Device Flash I/O:** Smartphone storage write speed limitations when receiving large chunks.

### Q7. What is the difference between Half-Duplex and Full-Duplex communication in Bluetooth?
**Answer:** Bluetooth uses **Time Division Duplexing (TDD)**, which is an alternating half-duplex mechanism where master and slave transmit in alternating $625\text{ \mu s}$ time slots. It creates the illusion of full-duplex communication through rapid time-slot switching.

### Q8. What is ACL and SCO in Bluetooth baseband?
**Answer:**
- **ACL (Asynchronous Connection-Less):** Used for **data transfer** (like files), where packet delivery must be $100\%$ error-free through ARQ retransmission, but latency is flexible.
- **SCO (Synchronous Connection-Oriented):** Used for **real-time voice/audio**, where latency must be fixed and deterministic, but dropped audio packets are not retransmitted.

### Q9. Why is Wi-Fi Direct or AirDrop/Nearby Share much faster than pure Bluetooth file transfer?
**Answer:** Bluetooth is primarily designed for low power consumption, not high-speed data transfers. Technologies like Wi-Fi Direct and Quick Share/Nearby Share use Bluetooth only for device discovery and initial handshakes, and then switch the actual high-speed file transfer to a high-bandwidth $5\text{ GHz}$ Wi-Fi connection ($> 300\text{ Mbps}$).

### Q10. What is the conclusion drawn from this experiment?
**Answer:** Practical Bluetooth throughput saturates around a maximum steady-state rate ($11 - 12\text{ Mbps}$ on modern mobile chipsets). While excellent for small photos, documents, and low-energy peripherals, Bluetooth is bandwidth-limited for transferring multi-gigabyte video files compared to Wi-Fi.


---
[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)
