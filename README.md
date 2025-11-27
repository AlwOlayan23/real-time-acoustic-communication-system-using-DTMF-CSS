# 🌊🔊 Acoustic Communication System

### **DTMF + LoRa-Style Acoustic Modem with MAVLink-Lite Framing**

*A compact, modern, low-data-rate digital communication system using sound.*

---

## 🚀 1. What This Project Is

This project builds a **complete acoustic modem** that transmits digital data using **sound** instead of radio waves.
It operates in **air** for easy experimentation and includes:

- **DTMF modem** (tone-based, robust)
- **LoRa-style chirp spread-spectrum modem** (modern, noise-resilient)
- **MAVLink-lite message protocol** (framing, CRC, packet structure)

<img width="650" alt="arch" src="https://github.com/user-attachments/assets/cfaacdbc-40d6-4381-9264-5602377e8555"/>

---

## 🧱 2. System Overview

```
Application Layer  →  MAVLink-lite Messages  
                    ↓
        ┌─────────────────────────┐
        │      Modulation         │
        │  DTMF      |     LoRa   │
        └────────────┴────────────┘
                    ↓
        Speaker → Sound Waves → Microphone
                    ↓
     Decoder (DTMF / LoRa) → MAVLink Parser
```

Compact. Modular. Swappable PHY layers.

---

## 🎯 3. Goals (Short + Clear)

- Build a **working acoustic modem** with off-the-shelf speakers/mics
- Compare **DTMF** vs **LoRa chirps**
- Design a **unified message protocol**
- Evaluate:
  - Range
  - Noise resilience
  - Symbol timing
  - Decode accuracy

---

## 🥇 4. Phase 1 — DTMF Modem (Tone-Based)

**What it does**  
Dual-tone signaling using standard telecom frequency pairs.

**Key features**

- Symbol = *(low freq + high freq)*
- Start/stop framing
- Optional parity
- Real-time using joystick

**Why it matters**  
Excellent baseline modem: stable, explainable, and easy to debug.

---

## 🥇 5. Phase 2 — LoRa-Inspired Chirp Modem

**What it does**  
Encodes symbols using **frequency-swept chirps** (upchirps + cyclic shifts).

**Key features**

- Linear FM chirp generator
- Baseband IQ extraction
- Preamble detection
- Downchirp de-rotation
- FFT-based symbol recovery
- Multi-symbol visualization tools

**Why it matters**  
LoRa-style CSS provides excellent noise robustness even at low SNR.

---

## 🎛️ 6. MAVLink-Lite Framing

A lightweight MAVLink-inspired protocol:

- Sync byte
- Payload length
- Message ID
- CRC
- Sequence counter
- Serializer + parser state machine

**Modulation agnostic** → Works over DTMF or LoRa.

---

## 🔄 7. End-to-End Data Flow

```
TX: Build Packet → Modulate (DTMF/LoRa) → Speaker  
RX: Microphone → Demodulate → Parse MAVLink → Application
```

Short. Understandable. Exactly how modern modems are described.

---

## 📊 8. Performance Evaluation

Recommended metrics:

- Distance vs Error Rate
- SNR sensitivity
- Chirp timing robustness
- DTMF confusion matrix
- Spectrogram analysis

---

## 🔧 9. Installation

```bash
git clone https://github.com/AlwOlayan23/acoustic-modem
cd acoustic-modem
pip install -r requirements.txt
```

**Core dependencies**  
`numpy`, `scipy`, `matplotlib`, `sounddevice/pyaudio`, `jupyter`

---

## ▶️ 10. Running the Examples

**DTMF**

```bash
python dtmf_tx.py
python dtmf_rx.py
```

**LoRa Chirp**

```bash
python lora_tx.py
python lora_rx.py
```

**MAVLink**

```bash
python mavlink_encode.py
python mavlink_decode.py
```

---

## 🚀 11. Future Extensions

- Underwater adaptation (hydrophones)
- Forward-error correction
- Adaptive symbol timing
- OFDM acoustic PHY
- Long-payload sync sequences

---

## 🏁 12. Summary

This project delivers a **clean, modular acoustic communication stack**:

- DTMF modem → simple & robust
- LoRa-style chirp modem → modern & resilient
- MAVLink-lite framing → structured messaging

A complete foundation for future **underwater acoustic communication** research.

---

## 👤 Author
KFUPM 
