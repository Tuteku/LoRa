# LoRa PHY SDR Implementation

![Status](https://img.shields.io/badge/Status-Work_in_Progress-yellow)
![Language](https://img.shields.io/badge/Language-Python_3-blue)
![Focus](https://img.shields.io/badge/Focus-DSP_%26_SDR-green)

## 📡 Overview

This project is a **pure software implementation (SDR)** of the LoRa (Long Range) Physical Layer (PHY). 

Unlike standard libraries that interface with LoRa chips, this project performs **reverse engineering and mathematical synthesis** of the LoRa waveform "from scratch" in baseband. It covers the entire signal processing chain: from bit generation and channel coding (Hamming, Whitening) to Chirp Spread Spectrum (CSS) modulation and demodulation.

The goal is to provide a comprehensive tool for analyzing LoRa modulation, researching DSP algorithms for signal recovery (CFO/SFO compensation), and transmitting/receiving frames using SDR hardware (HackRF/USRP).

## 🚀 Project Status

* **Transmitter (Tx):** ✅ **Completed**. Capable of generating valid LoRa frames (Preambule + SFD + Header + Payload + CRC) and modulating them into IQ samples.
* **Receiver (Rx):** 🚧 **In Development**. Currently working on advanced synchronization algorithms (Dechirping, Peak Merging) to decode signals in noisy environments.

## ✨ Key Features

### 1. Transmitter (Baseband Synthesis)
* **Full Frame Assembly:** Implementation of the complete LoRa packet structure including Variable Length Header and CRC.
* **Channel Coding:**
    * Forward Error Correction (FEC) using Hamming Code (4/5 to 4/8).
    * Data Whitening and Diagonal Interleaving.
* **Modulation (CSS):**
    * Mathematical generation of **Up-Chirps** and **Down-Chirps**.
    * **Waveform Former:** Precise symbol-to-chirp mapping based on Spreading Factors (SF7 - SF12).
    * Generation of normalized IQ samples for SDR transmission.

### 2. Receiver & DSP (Work in Progress)
Research and implementation of the reception chain:
* **Dechirping:** Multiplying incoming signal by conjugate chirps to transform frequency sweeps into constant frequency tones (for FFT analysis).
* **Synchronization:**
    * **Window Alignment:** Using Preamble and SFD to align demodulation windows and compensate for Carrier Frequency Offset (CFO).
    * **Clock Recovery:** Tracking bin drift to correct Sampling Frequency Offset (SFO) between Tx and Rx.
* **Peak Merging:** Combining FFT peaks from oversampled chirp segments (CPA/FPA methods) to determine symbol values.

### 3. Analysis & Debugging Tools
* Spectrogram visualization of the generated frames.
* Time-domain and Frequency-domain analysis tools.
* Phase tracking for chirp continuity validation.

## 🛠️ Technical Stack

* **Language:** Python 3.x
* **Core Libraries:** `numpy` (Vectorized math), `scipy` (Signal processing), `matplotlib` (Visualization).
* **Hardware Integration:**
    * HackRF One
    * USRP B210
    * (Interfaced via raw IQ file transfer or GNU Radio sink).

## 📊 Architecture

*(You can insert your block diagram image here)*

The implementation follows the theoretical breakdown of the LoRa modulation:
$$f(t) = f_c + \frac{BW}{T_s} t$$
Where the instantaneous frequency is modulated to encode symbol values.

## 📓 Usage

The core logic is implemented in Jupyter Notebooks for step-by-step analysis.

1.  **Generate a Frame:**
    Run the transmitter notebook to encode the string "Hello World".
    ```python
    # Example pseudo-code
    phy = LoRaPHY(sf=7, bw=125e3)
    iq_samples = phy.transmit("Hello World")
    phy.save_to_file("output.iq")
    ```

2.  **Transmit via SDR:**
    Use `hackrf_transfer` or similar tools to transmit the generated `.iq` file.

## 📚 References

This implementation is based on mathematical analysis from:
1.  *Principles of Digital Communication: A Top-Down Approach* - Bixio Rimoldi.
2.  Reverse engineering analysis of Semtech LoRa transceivers.
3.  Academic papers on Frequency Shift Chirp Modulation.

---
*Disclaimer: This project is for educational and research purposes only.*
