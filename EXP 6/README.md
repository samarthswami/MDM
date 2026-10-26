# Experiment 6: Study of Bluetooth Communication Using Mobile Devices (Pairing & Range Testing)

[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)

---

---

## 📌 Experiment Metadata
- **Experiment No:** 06
- **Title:** Study of Bluetooth Communication Using Mobile Devices (Pairing, Range Testing)
- **Aim:** To study the working of Bluetooth communication between mobile devices by performing Bluetooth discovery, pairing, passkey authentication, and measuring the effective communication range using a measuring tape in an open and obstructed environment.
- **Experiment Type:** **Hands-on Mobile Hardware Experiment (No Python code required in syllabus)**

---

## 🛠️ Requirements
- **Hardware:**
  - Two Bluetooth-enabled smartphones (Android / iOS)
  - Measuring tape ($10 - 20\text{ meters}$)
  - Open laboratory space / corridor
- **Software:**
  - Built-in Bluetooth Settings on both smartphones
  - File manager / gallery app for initiating test file share

---

## 📖 Core Theory & Principles

### 1. What is Bluetooth?
Bluetooth is a short-range wireless communication technology designed to exchange data between electronic devices without physical cables.
- **Standard:** IEEE 802.15.1 (Wireless Personal Area Network - WPAN).
- **Operating Band:** Unlicensed **$2.4\text{ GHz}$ ISM band** ($2402 - 2480\text{ MHz}$).
- **Channels:** 79 channels spaced $1\text{ MHz}$ apart.
- **Spread Spectrum:** Uses **Frequency Hopping Spread Spectrum (FHSS)** at **$1600\text{ hops/second}$** to prevent interference from Wi-Fi networks and microwave ovens.

---

### 2. Basic Bluetooth Communication Architecture
```
       Device A (Smartphone 1)                Device B (Smartphone 2)
    ┌───────────────────────────┐          ┌───────────────────────────┐
    │  Bluetooth Radio Enabled  │◄──2.4GHz─►  Bluetooth Radio Enabled  │
    └───────────────────────────┘          └───────────────────────────┘
                  │                                      │
                  └──────── Pairing & Authentication ────┘
```

A connection sequence follows 4 phases:
1. **Inquiry (Discovery):** Device A broadcasts inquiry packets to discover nearby discoverable Bluetooth devices.
2. **Paging (Connecting):** Device A initiates synchronization to Device B using its 48-bit Bluetooth MAC address (BD_ADDR).
3. **Pairing (Authentication):** Both devices establish a trusted relationship by exchanging a PIN / passkey or confirming a 6-digit number (Numeric Comparison in Secure Simple Pairing - SSP). A shared 128-bit Link Key is generated and cached.
4. **Data Exchange:** Data transfer occurs using Bluetooth profiles (e.g., OPP / OBEX).

---

### 3. Bluetooth Power Classes

| Class | Maximum Output Power | Typical Range | Common Application |
| :---: | :---: | :---: | :--- |
| **Class 1** | $100\text{ mW}$ ($+20\text{ dBm}$) | Up to $100\text{ meters}$ | Industrial wireless equipment, PCs |
| **Class 2** | $2.5\text{ mW}$ ($+4\text{ dBm}$) | Up to $10\text{ meters}$ | Smartphones, headphones, smartwatches |
| **Class 3** | $1\text{ mW}$ ($0\text{ dBm}$) | Up to $1\text{ meter}$ | Ultra-low-power sensors |

---

## 📝 Step-by-Step Procedure

### Part A — Bluetooth Pairing
1. Switch ON Bluetooth on both smartphones.
2. Go to **Settings $\rightarrow$ Bluetooth** on both devices and ensure visibility / discoverability is enabled.
3. From Smartphone A, tap **Search for devices**.
4. Select Smartphone B from the list of available devices.
5. A pairing prompt will appear displaying a 6-digit passkey.
6. Confirm the passkey on both devices.
7. Verify that the device status shows **"Paired"** or **"Connected"**.

### Part B — Communication & Range Testing
1. Keep Smartphone A stationary at a fixed location.
2. Place Smartphone B at a distance of **$1\text{ meter}$** (measured using the measuring tape).
3. Select a small test file (e.g., photo or document) and share via Bluetooth.
4. Record whether the transfer succeeds.
5. Move Smartphone B to **$2\text{ m}, 5\text{ m}, 10\text{ m}, 15\text{ m},$ and $20\text{ m}$**.
6. Repeat the transfer test at each distance.
7. Record the maximum distance at which communication remains reliable.

---

## 📊 Observation Table (Sample Laboratory Data)

| Sr. No. | Distance between Devices (m) | Path Condition | Connection Status | File Transfer Result |
| :---: | :---: | :---: | :---: | :---: |
| 1 | $1\text{ m}$ | Direct Line of Sight (LOS) | Connected | Successful |
| 2 | $2\text{ m}$ | Direct Line of Sight (LOS) | Connected | Successful |
| 3 | $5\text{ m}$ | Direct Line of Sight (LOS) | Connected | Successful |
| 4 | $10\text{ m}$ | Direct Line of Sight (LOS) | Connected (Marginal) | Successful |
| 5 | $12\text{ m}$ | Obstacle (Door / Wall) | Intermittent | Failed / Retrying |
| 6 | $15\text{ m}$ | Obstacle (Concrete Wall) | Disconnected | Failed |

> **Conclusion:** For standard Class 2 smartphone Bluetooth, communication is reliable up to approximately **$10\text{ meters}$** in clear line of sight. Beyond 10 meters or behind concrete walls, signal attenuation causes connection dropouts.

---

## 🎯 Viva Questions & Answers (Experiment 6)

### Q1. What is the IEEE standard for Bluetooth?
**Answer:** **IEEE 802.15.1**.

### Q2. What frequency band does Bluetooth operate in?
**Answer:** The unlicensed **$2.4\text{ GHz}$ ISM band** ($2402 - 2480\text{ MHz}$).

### Q3. What is Frequency Hopping Spread Spectrum (FHSS) and why does Bluetooth use it?
**Answer:** FHSS is a spread-spectrum modulation technique where the carrier frequency hops pseudo-randomly across 79 channels of 1 MHz each at **1600 hops per second**. It prevents interference from other devices sharing the 2.4 GHz band (like Wi-Fi) and protects against multipath fading.

### Q4. What is a Piconet and what are Master and Slave roles?
**Answer:** A Piconet is an ad-hoc Bluetooth network consisting of **1 Master** and up to **7 active Slaves**. The Master unit controls the frequency hopping channel, clock timing, and allocates time slots to each slave.

### Q5. What is a Scatternet?
**Answer:** A Scatternet is formed when two or more Piconets are interconnected by sharing common bridge nodes that act as a slave in one piconet and a master or slave in another.

### Q6. What is the typical range of a smartphone's Bluetooth?
**Answer:** Approximately **$10\text{ meters}$ ($33\text{ feet}$)**, as smartphones use **Class 2** transmitters with a maximum transmit power of $2.5\text{ mW}$ ($+4\text{ dBm}$).

### Q7. What are the steps to connect two Bluetooth devices?
**Answer:**
1. **Inquiry:** Scanning and finding nearby discoverable devices.
2. **Paging:** Synchronizing radio clocks using the target device's 48-bit MAC address (BD_ADDR).
3. **Pairing:** Exchanging security credentials (passkey / PIN) to generate a shared Link Key.
4. **Connection:** Establishing a profile connection (e.g., OPP for file sharing, A2DP for audio).

### Q8. What is the difference between Bluetooth Classic and Bluetooth Low Energy (BLE)?
**Answer:**
- **Bluetooth Classic:** Designed for continuous data transmission (streaming audio, hands-free phone calls, file transfer) with higher power consumption.
- **BLE (Bluetooth 4.0+):** Designed for low-power IoT sensors, smartwatches, and fitness trackers that transmit small packets periodically and run for months on coin-cell batteries.

### Q9. Why does Bluetooth signal drop when blocked by a human body?
**Answer:** The human body is composed of over $60\%$ water. Water molecules strongly absorb electromagnetic radiation in the $2.4\text{ GHz}$ frequency band, causing severe signal attenuation.

### Q10. What is BD_ADDR?
**Answer:** **Bluetooth Device Address**, a unique 48-bit physical MAC address burned into every Bluetooth hardware chip by its manufacturer.


---
[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)
