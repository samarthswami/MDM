# Experiment 1: Study of Electromagnetic Spectrum Allocation and Wireless Bands Used in Communication

[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)

---

---

## 📌 Experiment Metadata
- **Experiment No:** 01
- **Title:** Study of Electromagnetic Spectrum Allocation and Wireless Bands Used in Communication
- **Aim:** To study the electromagnetic spectrum, frequency-wavelength relationships, spectrum allocation principles, licensed vs. unlicensed bands, and wireless frequency bands used in modern communications (Cellular, Wi-Fi, Bluetooth, Satellite, GPS).
- **Experiment Type:** **Study & Analytical Experiment (No Python programming required in syllabus)**

---

## 🛠️ Requirements
- **Hardware:** Desktop / Laptop / Smartphone
- **Software:** Internet Connection / Web Browser (Reference search)

---

## 📖 Core Theory & Concepts

### 1. The Electromagnetic (EM) Spectrum
The electromagnetic spectrum is the continuous range of all electromagnetic radiation frequencies and wavelengths. Electromagnetic waves travel at the speed of light through free space and do not require a physical medium.

The fundamental relationship between speed, frequency, and wavelength is:
$$c = f \times \lambda$$

Where:
- $c$ = Speed of light ($3 \times 10^8 \text{ m/s}$)
- $f$ = Frequency in Hertz ($\text{Hz}$)
- $\lambda$ = Wavelength in meters ($\text{m}$)

> **Important Facts:**
> - **High frequency $\implies$ Short wavelength:** High data transmission capacity, but covers shorter distances and suffers higher obstacle attenuation.
> - **Low frequency $\implies$ Long wavelength:** Lower data transmission capacity, but travels farther and penetrates walls/obstacles effectively.

---

### 2. Regions of the Electromagnetic Spectrum

| Region | Frequency Range | Applications |
| :--- | :--- | :--- |
| **Radio Waves** | $3 \text{ kHz} - 300 \text{ MHz}$ | AM/FM Radio, TV broadcasting |
| **Microwaves** | $300 \text{ MHz} - 300 \text{ GHz}$ | Mobile phones, Wi-Fi, Bluetooth, Satellite, Radar |
| **Infrared** | $300 \text{ GHz} - 430 \text{ THz}$ | Remote controls, night vision, thermal imaging |
| **Visible Light** | $430 \text{ THz} - 770 \text{ THz}$ | Optical fiber communication, Li-Fi, human sight |
| **Ultraviolet** | Above Visible Light | Sterilization, medical treatments |
| **X-Rays** | High Frequency | Medical imaging (radiography), security scanners |
| **Gamma Rays** | Highest Frequency | Cancer radiation therapy, sterilization |

---

### 3. Radio Frequency (RF) Bands (ITU Standard)

| Band Designation | Full Name | Frequency Range | Common Applications |
| :--- | :--- | :--- | :--- |
| **VLF** | Very Low Frequency | $3 - 30 \text{ kHz}$ | Submarine communication, Navigation |
| **LF** | Low Frequency | $30 - 300 \text{ kHz}$ | Long-distance communication, RFID |
| **MF** | Medium Frequency | $300 \text{ kHz} - 3 \text{ MHz}$ | AM Radio broadcasting |
| **HF** | High Frequency | $3 - 30 \text{ MHz}$ | Shortwave radio, Aviation communication |
| **VHF** | Very High Frequency | $30 - 300 \text{ MHz}$ | FM Radio ($88-108\text{ MHz}$), Television |
| **UHF** | Ultra High Frequency | $300 \text{ MHz} - 3 \text{ GHz}$ | Mobile communication (GSM, 3G, 4G), Wi-Fi, Bluetooth, GPS |
| **SHF** | Super High Frequency | $3 - 30 \text{ GHz}$ | Wi-Fi ($5\text{ GHz}$), Satellite communication, Radar |
| **EHF** | Extremely High Frequency | $30 - 300 \text{ GHz}$ | 5G mmWave ($26-28\text{ GHz}$), Automotive radar |

---

### 4. Spectrum Allocation & Regulatory Organizations
Spectrum allocation is the administrative process of assigning specific frequency bands to designated communication services to prevent mutual radio interference and guarantee quality of service.

- **International:** **ITU** (International Telecommunication Union)
- **India:** **DoT** (Department of Telecommunications) and **WPC** (Wireless Planning & Coordination Wing)
- **USA:** **FCC** (Federal Communications Commission)

---

### 5. Licensed vs. Unlicensed Spectrum

| Feature | Licensed Spectrum | Unlicensed Spectrum (ISM Bands) |
| :--- | :--- | :--- |
| **Definition** | Bands auctioned exclusively to operators. | Bands open for free public/private use. |
| **Cost** | Very expensive (billions of dollars in spectrum auctions). | Free of cost. |
| **Interference** | Legally protected from interference; strict QoS. | Prone to interference from nearby consumer devices. |
| **Examples** | Cellular networks (2G, 3G, 4G, 5G), TV broadcasting. | Wi-Fi ($2.4, 5, 6\text{ GHz}$), Bluetooth ($2.4\text{ GHz}$), Zigbee. |

---

### 6. Common Wireless Communication Bands

| Technology | Operating Frequency | Primary Application |
| :--- | :--- | :--- |
| **AM Radio** | $535 - 1705 \text{ kHz}$ | Amplitude modulation audio broadcast |
| **FM Radio** | $88 - 108 \text{ MHz}$ | High-fidelity music broadcast |
| **Television** | $54 - 806 \text{ MHz}$ | Terrestrial TV broadcast |
| **GSM (2G)** | $900 \text{ MHz}, 1800 \text{ MHz}$ | Voice calls and SMS |
| **3G** | $2100 \text{ MHz}$ | Mobile internet and voice |
| **4G LTE** | $700 \text{ MHz}, 1800 \text{ MHz}, 2300 \text{ MHz}$ | High-speed mobile broadband |
| **5G NR** | $3.5 \text{ GHz}$ (Sub-6), $26 \text{ GHz}$ (mmWave) | Ultra-high-speed low-latency mobile communication |
| **Wi-Fi** | $2.4 \text{ GHz}, 5 \text{ GHz}, 6 \text{ GHz}$ | Wireless Local Area Network (WLAN) |
| **Bluetooth** | $2.4 \text{ GHz}$ ($2402 - 2480\text{ MHz}$) | Personal Area Network (PAN) |
| **GPS** | $1575.42 \text{ MHz}$ (L1) | Satellite navigation and positioning |
| **RFID / NFC** | $13.56 \text{ MHz}$ | Contactless payment, smart tags |

---

## 🎯 Viva Questions & Answers (Experiment 1)

### Q1. What is the electromagnetic spectrum?
**Answer:** The electromagnetic spectrum is the complete range of all electromagnetic waves arranged according to their frequency or wavelength, travelling at the speed of light in free space.

### Q2. What is the formula relating frequency and wavelength?
**Answer:**
$$c = f \times \lambda$$
Where $c = 3 \times 10^8 \text{ m/s}$ (speed of light), $f$ is frequency in $\text{Hz}$, and $\lambda$ is wavelength in meters.

### Q3. Why do lower frequencies propagate farther than higher frequencies?
**Answer:** Lower frequencies have longer wavelengths, allowing them to diffract around physical obstacles, penetrate building walls and foliage, and experience much lower atmospheric absorption and free-space path loss compared to high frequencies.

### Q4. What is the difference between Licensed and Unlicensed spectrum?
**Answer:**
- **Licensed Spectrum:** Frequency bands bought by telecom operators through government auctions, giving them exclusive rights to transmit without interference (e.g., 4G/5G mobile bands).
- **Unlicensed Spectrum (ISM):** Frequency bands that anyone can use freely without paying license fees, provided devices adhere to transmit power limits (e.g., 2.4 GHz and 5 GHz for Wi-Fi and Bluetooth).

### Q5. What does ISM stand for? Give examples.
**Answer:** **Industrial, Scientific, and Medical** bands. Common examples include $2.4\text{ GHz}$ ($2.400 - 2.4835\text{ GHz}$) and $5.8\text{ GHz}$.

### Q6. Which regulatory body manages spectrum in India?
**Answer:** The **WPC (Wireless Planning and Coordination Wing)** under the **DoT (Department of Telecommunications)**, Ministry of Communications, Government of India.

### Q7. In which RF band does Wi-Fi and Bluetooth operate?
**Answer:** **UHF (Ultra High Frequency)** at $2.4\text{ GHz}$, and **SHF (Super High Frequency)** at $5\text{ GHz}$ (Wi-Fi).

### Q8. What is millimeter wave (mmWave) in 5G?
**Answer:** Frequencies in the **EHF band ($30 - 300\text{ GHz}$)** with wavelengths between $1\text{ mm}$ and $10\text{ mm}$, used for ultra-high-speed 5G mobile data transmission.

### Q9. Why does microwave oven operation sometimes disrupt Wi-Fi?
**Answer:** Because microwave ovens heat food using electromagnetic radiation at $2.45\text{ GHz}$, which lies directly inside the unlicensed 2.4 GHz Wi-Fi band. Radiation leakage from the oven interferes with nearby Wi-Fi channels.

### Q10. What is Li-Fi and which spectrum region does it use?
**Answer:** Li-Fi is high-speed bidirectional wireless communication using light-emitting diodes (LEDs). It operates in the **Visible Light spectrum ($430 - 770\text{ THz}$)**.


---
[⬅️ Back to Lab Overview / README](../README.md) | [📘 Master Viva Q&A](../VIVA_QUESTIONS_AND_ANSWERS.md)
