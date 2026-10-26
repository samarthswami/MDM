# Experiment 10: Wi-Fi Network Scanning Using Mobile Apps (SSID, RSSI, Channel Mapping)

[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)

---

---

## 📌 Experiment Metadata
- **Experiment No:** 10
- **Title:** Wi-Fi Network Scanning Using Mobile Apps (SSID, RSSI, Channel Mapping)
- **Aim:** To scan and analyze nearby Wi-Fi networks by measuring parameters such as SSID, BSSID, RSSI, channel number, operating frequency, and security standard using the open-source *WiFi Analyzer* smartphone application, and identify sources of channel congestion and interference.
- **Experiment Type:** **Hands-on Smartphone App Practical (No Python code required in syllabus)**

---

## 🛠️ Requirements
- **Hardware:**
  - Android smartphone with Wi-Fi capability
  - Wi-Fi Router or Access Point
- **Software:**
  - *WiFi Analyzer (open-source)* application by VREM Software Development (available free on Google Play Store)

---

## 📖 Core Theory & Principles

### 1. Wi-Fi (IEEE 802.11) Overview
Wi-Fi networks operate in unlicensed radio frequency bands, primarily:
- **$2.4\text{ GHz}$ Band:** Longer transmission range, better wall penetration, but narrow spectrum and heavily congested.
- **$5\text{ GHz}$ Band:** Shorter transmission range, higher attenuation through walls, but wider spectrum with high data rates and low interference.

---

### 2. Fundamental Wi-Fi Parameters
1. **SSID (Service Set Identifier):** The human-readable name of the wireless network (up to 32 characters), identifying a specific wireless LAN.
2. **BSSID (Basic Service Set Identifier):** The 48-bit physical MAC address of the Access Point radio transmitter (e.g., `A4:2B:B0:89:C1:20`).
3. **RSSI (Received Signal Strength Indicator):** The power of the received radio signal, expressed in **negative dBm**. Values closer to 0 indicate stronger signals (e.g., $-40\text{ dBm}$ is strong, $-85\text{ dBm}$ is weak).
4. **Channel Number:** In the $2.4\text{ GHz}$ band, channels 1 to 14 are spaced only $5\text{ MHz}$ apart. Because standard Wi-Fi transmissions require $20\text{ MHz}$ of bandwidth, adjacent channels overlap.
5. **Non-overlapping Channels:** To prevent channel overlap and mutual interference, only three channels in the 2.4 GHz band are non-overlapping:
   $$\mathbf{Channel\ 1},\quad \mathbf{Channel\ 6},\quad \mathbf{Channel\ 11}$$

```
Channel:    1       2       3       4       5       6       7       8       9      10      11
          ┌───┐                                   ┌───┐                                   ┌───┐
         /     \                                 /     \                                 /     \
        /       \                               /       \                               /       \
  ─────┴─────────┴─────────────────────────────┴─────────┴─────────────────────────────┴─────────┴────
       2412 MHz                                 2437 MHz                                 2462 MHz
       [   CH 1: NON-OVERLAPPING   ]            [   CH 6: NON-OVERLAPPING   ]            [  CH 11: NON-OVERLAPPING  ]
```

---

### 3. Working Principle of Wi-Fi Analyzers
Wi-Fi analyzers work by **passively listening** to 802.11 **Beacon Frames** broadcast periodically (every $100\text{ ms}$) by access points. Beacon frames contain the network SSID, BSSID, supported data rates, channel number, and security protocols. The smartphone radio chip measures the signal strength of these frames to calculate RSSI.

---

## 📝 Step-by-Step Procedure

1. Install the open-source **WiFi Analyzer** app by VREM Software Development from the Google Play Store.
2. Open the app and grant the required Location permission (required by Android to perform Wi-Fi scanning).
3. Ensure Wi-Fi is enabled on your smartphone.
4. Stand at a fixed location in your lab or home.
5. Explore the views in the app:
   - **Access Points (List View):** Displays all visible networks with SSID, BSSID, RSSI, channel, and security standard.
   - **Channel Graph View:** Displays parabolic signal curves across channels, visually highlighting overlapping networks.
   - **Time Graph View:** Displays RSSI signal fluctuations over time for a selected network.
6. Record readings for at least 5 visible Wi-Fi networks in **Table 1**.
7. Analyze channel congestion in **Table 2**.

---

## 📊 Observation Tables

### Table 1: Wi-Fi Network Parameters

| Sr. No. | SSID (Network Name) | BSSID (MAC Address) | RSSI ($\text{dBm}$) | Channel | Frequency Band | Security Standard |
| :---: | :--- | :--- | :---: | :---: | :---: | :--- |
| 1 | `Campus_WiFi_Host` | `54:AF:97:12:4A:A1` | $-42\text{ dBm}$ | 6 | $2.4\text{ GHz}$ | WPA2-PSK |
| 2 | `Lab_HighSpeed_5G` | `54:AF:97:12:4A:A2` | $-48\text{ dBm}$ | 36 | $5\text{ GHz}$ | WPA3-Personal |
| 3 | `Department_Office`| `A0:04:60:88:E2:31` | $-65\text{ dBm}$ | 1 | $2.4\text{ GHz}$ | WPA2-Enterprise |
| 4 | `Student_Hostel_Net`| `88:D7:F6:3C:99:10` | $-72\text{ dBm}$ | 11 | $2.4\text{ GHz}$ | WPA2-PSK |
| 5 | `Guest_Neighbor_Net`| `CC:32:E5:1B:02:44` | $-81\text{ dBm}$ | 6 | $2.4\text{ GHz}$ | WPA2-PSK |

### Table 2: Channel Utilization Summary

| Channel Number | Number of Visible Networks | Interference Condition | Recommendation |
| :---: | :---: | :--- | :--- |
| **Channel 1** | 1 | Moderate / Low | Good channel selection |
| **Channel 6** | 2 | **Congested** (Co-Channel Interference) | High contention, slower speeds |
| **Channel 11** | 1 | **Clean / Low** | **Best channel for router configuration** |

---

## 🎯 Viva Questions & Answers (Experiment 10)

### Q1. What is the difference between SSID and BSSID?
**Answer:**
- **SSID (Service Set Identifier):** The human-friendly name of the Wi-Fi network (e.g., `Lab_WiFi`).
- **BSSID (Basic Service Set Identifier):** The unique 48-bit hardware MAC address of the Access Point radio transmitting that network (e.g., `54:AF:97:12:4A:A1`).

### Q2. Which channels in the 2.4 GHz band are non-overlapping?
**Answer:** **Channels 1, 6, and 11**. Each Wi-Fi channel requires $20\text{ MHz}$ of bandwidth, but channels are centered $5\text{ MHz}$ apart. Channels 1, 6, and 11 are spaced $25\text{ MHz}$ apart, ensuring zero spectral overlap.

### Q3. Why do 2.4 GHz channels overlap?
**Answer:** The total 2.4 GHz ISM band is only about $83.5\text{ MHz}$ wide. Channels were allocated with $5\text{ MHz}$ spacing, whereas 802.11 transmissions need at least $20\text{ MHz}$ bandwidth. Consequently, each channel overlaps with several neighboring channels.

### Q4. What is the difference between Co-Channel Interference (CCI) and Adjacent-Channel Interference (ACI)?
**Answer:**
- **Co-Channel Interference (CCI):** Networks operate on the exact same channel (e.g., two routers both on Channel 6). They use CSMA/CA to coordinate transmissions politely without corrupting packets, though throughput drops due to airtime sharing.
- **Adjacent-Channel Interference (ACI):** Routers operate on overlapping intermediate channels (e.g., Channel 1 and Channel 3). The transmissions act as unstructured noise to each other, causing continuous packet collisions, CRC errors, and connection drops.

### Q5. What are Beacon Frames in Wi-Fi?
**Answer:** Management frames broadcast periodically (typically every $100\text{ ms}$) by Access Points to announce their presence, SSID, supported data rates, security encryption standards, and channel numbers.

### Q6. Compare 2.4 GHz and 5 GHz Wi-Fi.
**Answer:**
- **2.4 GHz:** Longer range, superior wall penetration, but crowded (only 3 non-overlapping channels) and lower speeds ($50-150\text{ Mbps}$).
- **5 GHz:** Shorter range, higher wall attenuation, but vast spectrum (over 24 non-overlapping channels), channel bonding up to $160\text{ MHz}$, and gigabit speeds ($> 1\text{ Gbps}$).

### Q7. What Wi-Fi security protocols are in use today?
**Answer:**
- **WEP:** Insecure, deprecated (broken RC4 cipher).
- **WPA2:** Industry standard using AES-CCMP encryption.
- **WPA3:** Latest standard using SAE (Simultaneous Authentication of Equals) to protect against dictionary attacks and provide forward secrecy.

### Q8. What is the practical takeaway of this experiment?
**Answer:** By scanning the wireless environment, network administrators can identify congested channels and manually reconfigure access points to the least occupied non-overlapping channel (1, 6, or 11), minimizing packet collisions and maximizing real-world network throughput.


---
[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)
