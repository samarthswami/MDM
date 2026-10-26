# Experiment 9: Study of Cellular Network Parameters Using Smartphones (Signal Strength, Network Type 2G/3G/4G/5G)

[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)

---

---

## 📌 Experiment Metadata
- **Experiment No:** 09
- **Title:** Study of Cellular Network Parameters Using Smartphones (Signal Strength, Network Type 2G/3G/4G/5G)
- **Aim:** To study and measure key cellular network parameters (Network Technology, Signal Strength, RSRP, RSRQ, SINR) using an Android/iOS smartphone, identify the active network generation (2G, 3G, 4G, 5G), and understand cellular engineering parameters.
- **Experiment Type:** **Hands-on Smartphone Practical Experiment (No Python code required in syllabus)**

---

## 🛠️ Requirements
- **Hardware:** Desktop / Laptop (for documentation), Android/iOS Smartphone with active SIM card.
- **Software:** Built-in Android Settings / Phone Status or diagnostic dialer code (`*#*#4636#*#*`), or free cellular monitoring app (e.g., *Network Cell Info Lite*, *NetVelocity*).

---

## 📖 Core Theory & Network Parameters

### 1. Cellular Network Architecture
A cellular network divides a geographical service area into small zones called **cells**, each served by a Base Transceiver Station (BTS / NodeB / eNodeB / gNodeB). As a mobile user moves between cells, the network manages seamless **Handoff (Handover)** without dropping active calls or data connections.

---

### 2. Cellular Network Generations

| Generation | Standard / Technology | Primary Capabilities | Typical Speed |
| :---: | :---: | :--- | :---: |
| **2G** | GSM, GPRS, EDGE | Voice calls, SMS, basic web data | $64 - 384\text{ kbps}$ |
| **3G** | UMTS, WCDMA, HSPA+ | Mobile internet, video calls | $2 - 42\text{ Mbps}$ |
| **4G** | LTE, LTE-Advanced (4G+) | High-speed mobile data, VoLTE (HD voice) | $100 - 300\text{ Mbps}$ |
| **5G** | 5G NR (New Radio) | Ultra-high speeds, massive IoT, ultra-low latency (<1ms) | $1 - 10\text{ Gbps}$ |

---

### 3. Key Cellular Radio Parameters

#### A. Network Type
Indicates the active cellular technology to which the smartphone is currently attached:
- `GSM` / `EDGE` $\rightarrow$ 2G
- `UMTS` / `HSPA` / `HSPA+` $\rightarrow$ 3G
- `LTE` / `LTE_CA` $\rightarrow$ 4G / 4G+
- `NR` (New Radio) $\rightarrow$ 5G

#### B. Signal Strength (in dBm)
Indicates how strongly the smartphone antenna receives the cellular signal from the nearest base station. It is expressed in **negative dBm**:
- **$-60\text{ to } -80\text{ dBm}$:** Strong signal (near base station).
- **$-85\text{ to } -100\text{ dBm}$:** Medium / Good signal.
- **$-105\text{ to } -115\text{ dBm}$:** Weak signal / Cell edge (slow data, risk of call drop).

#### C. Signal Quality Metrics (4G LTE & 5G)
- **RSRP (Reference Signal Received Power):** Average received power of cell-specific reference signals in LTE/5G. It is the primary metric for coverage and cell selection.
- **RSRQ (Reference Signal Received Quality):** Measures received quality and cell load:
  $$\text{RSRQ} = N \times \frac{\text{RSRP}}{\text{RSSI}}$$
  Values above $-10\text{ dB}$ represent excellent quality; values below $-15\text{ dB}$ indicate severe congestion or co-channel interference.
- **SINR (Signal-to-Interference-plus-Noise Ratio):** The ratio of desired signal power to interference plus noise. Higher SINR enables higher-order modulation (256-QAM) and maximum download speed.

---

## 📝 Step-by-Step Procedure

1. Take an Android smartphone with an active SIM card.
2. Open **Settings $\rightarrow$ About Phone $\rightarrow$ Status Information $\rightarrow$ SIM Card Status**.
3. Observe and record:
   - **Network Type:** (e.g., LTE, NR)
   - **Signal Strength:** (e.g., $-85\text{ dBm}\ 55\text{ asu}$)
   - **Mobile Network State:** (Connected)
   - **Service State:** (In service)
4. *(Optional Engineering Mode):* Open Phone Dialer and enter `*#*#4636#*#*` $\rightarrow$ select **Phone Information** to view advanced parameters (RSRP, RSRQ, Cell ID, Band).
5. Move from an indoor room to an outdoor balcony or open street and observe how the signal strength changes.

---

## 📊 Sample Observation Table

| Parameter Name | Observed Value | Standard Unit | Quality Rating |
| :--- | :---: | :---: | :--- |
| **Network Type** | `LTE` (4G) | - | High-Speed Data |
| **Signal Strength (RSRP)** | $-84$ | $\text{dBm}$ | Very Good |
| **Signal Quality (RSRQ)** | $-9$ | $\text{dB}$ | Excellent |
| **SINR** | $+17.5$ | $\text{dB}$ | Excellent |
| **ASU (Arbitrary Strength Unit)** | $56$ | - | Good |
| **Operating Frequency Band** | Band 3 ($1800\text{ MHz}$) | $\text{MHz}$ | Urban LTE Downlink |

---

## 🎯 Viva Questions & Answers (Experiment 9)

### Q1. What does RSRP stand for, and why is it used in 4G LTE/5G?
**Answer:** RSRP stands for **Reference Signal Received Power**. It is the average power (in dBm) received from cell-specific reference signals across the channel bandwidth. It is used as the primary parameter for cell reselection and handover decisions.

### Q2. How is RSRP different from RSSI?
**Answer:**
- **RSSI:** Measures total wideband power across the entire channel, including serving cell signal, co-channel interference from neighbor towers, and background noise.
- **RSRP:** Measures strictly the power of the serving cell's reference signals, making it an accurate indicator of coverage independent of cell traffic or interference.

### Q3. What is RSRQ and what does it indicate?
**Answer:** RSRQ stands for **Reference Signal Received Quality**, defined as:
$$\text{RSRQ} = N \times \frac{\text{RSRP}}{\text{RSSI}}$$
It indicates radio channel quality and cell congestion. A low RSRQ indicates either high traffic congestion on the cell tower or severe co-channel interference from adjacent towers.

### Q4. What is SINR?
**Answer:** **Signal-to-Interference-plus-Noise Ratio**. It measures the ratio of the desired signal power to the combined power of interfering signals from neighbor cells plus background thermal noise. High SINR allows the base station to use high-order modulation (e.g., 256-QAM), directly maximizing user download throughput.

### Q5. What is the secret Android dialer code used to inspect cellular engineering parameters?
**Answer:** `*#*#4636#*#*` (opens the Android Testing / Phone Information menu).

### Q6. What does VoLTE mean and how does it differ from 2G/3G calling?
**Answer:** VoLTE stands for **Voice over LTE**. In 2G/3G, voice calls were routed over legacy circuit-switched networks. In VoLTE, voice calls are digitized into packet data and transmitted as high-priority IP packets directly over the 4G LTE data network, providing instant call setup and HD audio quality.

### Q7. What is Carrier Aggregation (4G+ / LTE-Advanced)?
**Answer:** The simultaneous combination of multiple separate carrier frequency bands (e.g., 1800 MHz + 2300 MHz) into a single logical channel to multiply peak user data speeds.

### Q8. What are the three primary 5G usage scenarios?
**Answer:**
1. **eMBB (Enhanced Mobile Broadband):** Multi-gigabit speeds for video streaming and mobile devices.
2. **URLLC (Ultra-Reliable Low-Latency Communication):** Sub-millisecond latency for autonomous driving, industrial robotics, and remote surgery.
3. **mMTC (Massive Machine-Type Communications):** Supporting up to 1 million IoT devices per $\text{km}^2$.

### Q9. What is a Handover (Handoff) in cellular networks?
**Answer:** Handover is the automatic process of transferring an ongoing call or active data session from one cell base station to another as the subscriber moves through the cellular coverage area without dropping the connection.

### Q10. Why is signal strength alone insufficient to determine internet speed?
**Answer:** Even with a strong signal (e.g., $-70\text{ dBm}$ RSRP), if the cell tower is congested with thousands of users or suffers heavy interference (low RSRQ / low SINR), the available throughput will be severely reduced.


---
[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)
