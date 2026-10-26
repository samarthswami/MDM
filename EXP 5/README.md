# Experiment 5: Throughput and Latency Measurement Using Network Tools

[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)

---

---

## 📌 Experiment Metadata
- **Experiment No:** 05
- **Title:** Throughput and Latency Measurement Using Network Tools (Ping, Speed Test)
- **Aim:** To measure and analyze the latency (Round Trip Time) and throughput (upload and download speeds) of a network connection using standard networking utilities (`ping` and Speed Test), and visualize the performance parameters using Python.

---

## 🛠️ Requirements
- **Hardware:** Wi-Fi Router / Ethernet Broadband Connection, Laptop / PC with Network Interface Card (NIC).
- **Software:** Command Prompt (Windows) / Terminal (Linux/macOS), Web Browser (Speedtest.net / Fast.com), Python 3.x (Google Colab / VS Code).
- **Libraries:** `matplotlib`, `numpy`

---

## 📖 Core Theory & Network Metrics

### 1. Throughput vs. Bandwidth (The Highway Analogy)
- **Bandwidth:** The theoretical maximum data carrying capacity of a transmission medium, measured in bits per second ($\text{bps, Mbps, Gbps}$). *Think of it as the total number of lanes on a highway.*
- **Throughput:** The **actual** rate at which useful data packets are successfully delivered over the network per unit time, accounting for protocol headers, retransmissions, congestion, and server processing. *Think of it as the actual number of cars flowing through toll booths per hour.*
  $$\text{Throughput} \le \text{Bandwidth}$$

---

### 2. Latency (Round Trip Time - RTT)
- **Latency:** The time required for a packet of data to travel from the sender to the receiver, plus the time required for an acknowledgment to return to the sender.
- **Unit:** Milliseconds ($\text{ms}$).
- **Components of Total Network Delay:**
  $$D_{\text{total}} = D_{\text{propagation}} + D_{\text{transmission}} + D_{\text{queuing}} + D_{\text{processing}}$$
  - **Propagation Delay:** Time wave takes to travel through physical medium ($d / v$).
  - **Transmission Delay:** Time to push all packet bits onto wire ($L / R$).
  - **Queuing Delay:** Waiting time in router buffers during network congestion.
  - **Processing Delay:** Time router CPU takes to examine packet headers and determine routing table lookup.

---

### 3. Jitter (Packet Delay Variation)
- **Jitter:** The statistical variance or fluctuation in packet arrival times over a connection.
- If packet 1 arrives in $20\text{ ms}$, packet 2 in $50\text{ ms}$, and packet 3 in $15\text{ ms}$, the connection suffers from high jitter. High jitter causes robotic voices, stuttering audio, and video buffering in real-time streaming (VoIP, Zoom, multiplayer gaming).

---

### 4. Working Principle of Network Tools

#### A. The `ping` Command
- Operates at the **Network Layer (Layer 3)** of the OSI model using the **ICMP (Internet Control Message Protocol)**.
- Sends an **ICMP Type 8 (Echo Request)** message to the destination host and listens for an **ICMP Type 0 (Echo Reply)**.
- Reports minimum, maximum, and average Round Trip Time (RTT), along with percentage packet loss.

```cmd
ping google.com
# Output: Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
# Minimum = 14ms, Maximum = 22ms, Average = 17ms
```

#### B. Speed Test Tools (e.g., Ookla Speedtest, Fast.com)
- Operates at the **Transport / Application Layer (Layer 4/7)** using multi-threaded HTTP/HTTPS over TCP.
- Establishes multiple concurrent TCP sockets to a nearby test server, floods the connection with chunks of data, and divides total bytes transferred by elapsed time to calculate **Download** and **Upload** speeds in $\text{Mbps}$.
- **Why is Download Speed usually higher than Upload Speed?**
  Most consumer internet plans are asymmetric (ADSL, Cable, 4G/5G TDD mode allocates more time/frequency slots to downlink than uplink) because average consumers consume far more data (streaming, downloading) than they upload.

---

## 📊 Sample Observation Table

| Network Parameter | Measured Value | Standard Unit | Quality Assessment |
| :--- | :---: | :---: | :--- |
| **Download Throughput** | $95.4$ | $\text{Mbps}$ | Excellent (supports multiple 4K streams) |
| **Upload Throughput** | $42.1$ | $\text{Mbps}$ | Very Good (ideal for high-res video meetings) |
| **Latency (Ping RTT)** | $18.0$ | $\text{ms}$ | Excellent (< 30 ms is competitive gaming grade) |
| **Jitter** | $2.5$ | $\text{ms}$ | Very Low (< 10 ms is excellent) |
| **Packet Loss** | $0.0\%$ | $\%$ | Perfect (zero retransmissions) |

---

## 💻 Google Colab Code (Copy-Paste Ready)

Copy and run the code below in Google Colab to visualize the measured network performance:

```python
# ==============================================================================
# Experiment 5: Network Performance Visualization (Throughput & Latency)
# Run this cell directly in Google Colab or Jupyter Notebook
# ==============================================================================

import matplotlib.pyplot as plt
import numpy as np

# Experimental Data
metrics = ['Download Speed', 'Upload Speed', 'Latency (RTT)', 'Jitter']
values = [95.4, 42.1, 18.0, 2.5]
units = ['Mbps', 'Mbps', 'ms', 'ms']
colors = ['#28a745', '#17a2b8', '#ffc107', '#fd7e14']

fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 5))

# --- Subplot 1: Throughput Comparison (Download vs Upload) ---
throughput_labels = ['Download', 'Upload']
throughput_vals = [95.4, 42.1]
bars1 = ax1.bar(throughput_labels, throughput_vals, color=['#28a745', '#17a2b8'], width=0.5, edgecolor='black')

for bar in bars1:
    yval = bar.get_height()
    ax1.text(bar.get_x() + bar.get_width()/2, yval + 2, f"{yval} Mbps", 
             ha='center', va='bottom', fontsize=11, fontweight='bold')

ax1.set_title("Network Throughput (Higher is Better)", fontsize=13, fontweight='bold')
ax1.set_ylabel("Speed (Mbps)", fontsize=11)
ax1.set_ylim(0, 120)
ax1.grid(axis='y', linestyle='--', alpha=0.6)

# --- Subplot 2: Delay Comparison (Latency vs Jitter) ---
delay_labels = ['Latency (RTT)', 'Jitter']
delay_vals = [18.0, 2.5]
bars2 = ax2.bar(delay_labels, delay_vals, color=['#ffc107', '#fd7e14'], width=0.5, edgecolor='black')

for bar in bars2:
    yval = bar.get_height()
    ax2.text(bar.get_x() + bar.get_width()/2, yval + 0.5, f"{yval} ms", 
             ha='center', va='bottom', fontsize=11, fontweight='bold')

ax2.set_title("Network Latency & Jitter (Lower is Better)", fontsize=13, fontweight='bold')
ax2.set_ylabel("Time (milliseconds)", fontsize=11)
ax2.set_ylim(0, 25)
ax2.grid(axis='y', linestyle='--', alpha=0.6)

plt.suptitle("Complete Network Performance Analysis (Ping & Speed Test)", fontsize=14, fontweight='bold', y=1.02)
plt.tight_layout()
plt.show()
```

---

## 🎯 Viva Questions & Answers (Experiment 5)

### Q1. What is the fundamental difference between Bandwidth and Throughput?
**Answer:** **Bandwidth** is the theoretical maximum transmission capacity of a network link (e.g., a 100 Mbps fiber line). **Throughput** is the actual amount of useful data successfully transmitted from source to destination per unit time, which is always lower due to packet headers, TCP handshakes, queuing delays, and network congestion.

### Q2. What protocol does the `ping` command use?
**Answer:** It uses **ICMP (Internet Control Message Protocol)**, which operates at the Network Layer (Layer 3) of the OSI model.

### Q3. What is Round Trip Time (RTT)?
**Answer:** RTT is the total elapsed time taken by a data packet to travel from the sender to a remote destination host plus the time taken for the acknowledgment/response packet to return to the original sender.

### Q4. What is Jitter and how does it affect real-time applications?
**Answer:** Jitter is the statistical fluctuation or variance in packet transit delay across a network. In real-time applications such as VoIP calls, video conferences (Zoom/Teams), and online gaming, high jitter causes audio clipping, robotic speech, lag spikes, and video stuttering.

### Q5. What are the four component delays that constitute total network latency?
**Answer:**
$$D_{\text{total}} = D_{\text{propagation}} + D_{\text{transmission}} + D_{\text{queuing}} + D_{\text{processing}}$$

### Q6. Why is consumer residential broadband typically asymmetric (Download > Upload)?
**Answer:** Most internet users download far more data (streaming 4K video, downloading software, browsing web pages) than they upload. Internet Service Providers (ISPs) therefore configure asymmetric spectral allocations (such as in DOCSIS cable or cellular TDD frame structures) to dedicate more frequency and time slots to the downlink than the uplink.

### Q7. What is Packet Loss and what causes it?
**Answer:** Packet loss occurs when one or more data packets travelling across a computer network fail to reach their intended destination. Common causes include network buffer overflows at congested intermediate routers, wireless interference/fading, and faulty network hardware.

### Q8. What happens during a Speed Test when you click "Start"?
**Answer:**
1. The client identifies and pings the lowest-latency test server.
2. The client opens multiple parallel TCP sockets to saturate bandwidth.
3. The server pushes test data chunks to measure **Download Throughput**.
4. The client pushes data chunks to the server to measure **Upload Throughput**.
5. Latency and jitter are calculated during packet exchanges.

### Q9. What does the parameter `TTL` (Time To Live) in ping output represent?
**Answer:** `TTL` is an 8-bit field in the IPv4 header that prevents packets from looping infinitely in a network due to routing errors. Every router hop decrements the TTL by 1; if TTL reaches 0, the packet is discarded, and an ICMP "Time Exceeded" message is returned.

### Q10. What is a "Good" ping latency for online gaming and video conferencing?
**Answer:**
- $< 20\text{ ms}$: Exceptional (ideal for competitive gaming).
- $20 - 50\text{ ms}$: Very good (smooth gaming, instant video conferencing).
- $50 - 100\text{ ms}$: Acceptable for web browsing and streaming.
- $> 150\text{ ms}$: Noticeable lag, audio overlap, and gaming delays.


---
[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)
