🎧🔊 Acoustic Communication System – Hybrid DTMF & LoRa-Style Audio Modem
<img width="627" height="780" alt="image" src="https://github.com/user-attachments/assets/cfaacdbc-40d6-4381-9264-5602377e8555" />

A Low-Data-Rate Digital Communication Stack Using Sound in Air

4
📌 Overview

This project develops a complete wireless acoustic modem that sends digital information using sound instead of radio waves.
It is designed as the foundation for future underwater communication, but this phase focuses on air-based communication to allow rapid prototyping, controlled experiments, and algorithm development.

The system integrates:

DTMF signaling (telecom dual-tone multi-frequency)

LoRa-inspired chirp spread spectrum audio modem

Mavlink-style message framing for structured, reliable messages

Each modulation method is implemented, tested, and visualized using the provided Jupyter notebooks.

🧱 System Architecture
┌────────────────────────────────────────────────────────────┐
│                       APPLICATION LAYER                    │
│                      (Message Builder)                    │
│                           Mavlink-lite                     │
└───────────────▲───────────────────────────▲────────────────┘
                │                           │
        DTMF Modem                    LoRa-Chirp Modem
        (Phase 1)                         (Phase 2)
                │                           │
┌───────────────┴───────────────────────────┴────────────────┐
│               Acoustic PHY (Speaker/Mic)                   │
│        Tone/Chirp Generation & Signal Processing           │
└────────────────────────────────────────────────────────────┘

🎯 Project Goals

Build a fully functional acoustic modem using speakers and microphones.

Implement and compare:

DTMF (simple, robust tone-based communication)

LoRa-style chirp spread spectrum (modern, noise-resistant)

Define a message protocol based on a simplified MAVLink structure.

Evaluate performance under:

distance

background noise

symbol timing errors

decoding accuracy

🥇 Phase 1 — DTMF Modulation & Decoding
📌 Summary

Phase 1 implements a classical DTMF modem, where each symbol is encoded using two simultaneous sinusoidal tones (one low-band, one high-band).

🔬 Implemented Features (from the notebook)
✔ Tone Generation

Each symbol generates:
s(t) = sin(2πf_low t) + sin(2πf_high t)

Exact frequencies follow the ITU DTMF standard.

✔ Decoding Algorithm

Recorded audio → windowed FFT

Peak detection finds the two strongest frequencies

Symbol determined by (low_freq, high_freq) lookup

Includes:

Noise filtering

Windowing

Normalization

Thresholding logic

Can decode sequences like:

# A 1 3 *

✔ Protocol Design

Start tone (e.g., "*")

Payload tones

End tone (e.g., "#")

Optional simple parity check

✔ Visualizations

Spectrogram of transmitted tones

FFT magnitude plots

Symbol-by-symbol decoding timeline

This phase produces a complete working audio modem with stable and explainable behavior.

🥇 Phase 2 — LoRa-Inspired Chirp Spread Spectrum Audio Modem
<img width="1292" height="756" alt="image" src="https://github.com/user-attachments/assets/15c3cfd1-5843-4a32-8924-8539c3cba213" />

📌 Summary

Phase 2 implements a chirp-based low-data-rate modem inspired by LoRa’s CSS (Chirp Spread Spectrum) but adapted for acoustic transmission.

🔬 Implemented Features (from the notebook)
✔ Chirp Generation

Upchirps: frequency sweeps low → high

Downchirps: high → low

Linear FM:
f(t) = f0 + kt

FFTs confirm clean spectral sweeps.

✔ Baseband Conversion

Audio → complex IQ baseband

Frequency de-rotation

Sample alignment for symbol extraction

✔ Symbol Encoding

Each symbol is encoded by circularly shifting an upchirp by M bins

Equivalent to LoRa’s symbol mapping:

symbol → frequency offset → chirp shift

✔ Decoding Algorithm

Multiply received chirp by reference downchirp

FFT to extract dominant bin

Determine symbol index

Includes:

Oversampling removal (OSR reduction)

Preamble detection

Fine time synchronization

Noise handling

Multi-symbol visualization tools

✔ Visualizations

Baseband IQ traces

Chirp correlation peaks

FFT waterfall plots

Symbol energy distributions

This notebook is extremely close to a true acoustic LoRa PHY layer.

🔧 Mavlink-Lite Message Framing Layer
📌 Role

This layer defines how messages are structured before modulation.

✔ Features from the notebook

Mavlink-style header (sync, length, msg-id)

CRC checksum generation

Sequence numbers

Packetization & serialization

Parsing state machine

Payload builder

The framing is modulation-agnostic, meaning the same packet can be:

encoded as DTMF tones

or encoded as LoRa chirps

This gives the project a real-world communication-protocol structure.

🎛️ Complete End-to-End Transmission Flow
          TX SIDE                                   RX SIDE
┌─────────────────────────┐               ┌─────────────────────────┐
│   Build Packet (Mavlink)│               │  Microphone Capture      │
└───────────────┬─────────┘               └───────────────┬─────────┘
                │                                         │
       Choose Modulation                                Detect
           ┌───────────────┬─────────────────┐            │
           │ DTMF Encoder  │  LoRa Encoder   │            │
           └───────┬───────┴──────┬──────────┘            │
                   │              │                         │
                Speaker Output     │                       │
                   │              │                         │
              ~ Sound Waves Through Air ~                  │
                   │              │                         │
       ┌───────────▼──────────┐ ┌─▼───────────────────┐    │
       │ DTMF Decoder         │ │ LoRa Symbol Extractor│    │
       └───────────┬──────────┘ └──────────┬──────────┘    │
                   ▼                       ▼                │
             Parse Mavlink Packet   Parse Mavlink Packet    │
                   ▼                       ▼                │
           Application Receives Message and Acts on It

📊 Performance Evaluation (Guidelines)

You can add measured results later. Recommended metrics:

Max reliable distance

SNR vs BER

Impact of background noise

Timing drift tolerance

Frequency response of speakers/mics

Spectrograms of received signals

Symbol confusion matrices

Placeholders for your plots:

[PLACEHOLDER: DTMF spectrogram]
[PLACEHOLDER: Chirp spectrogram]
[PLACEHOLDER: Correlation peaks]
[PLACEHOLDER: Mavlink packet logs]

🛠️ Installation
git clone https://github.com/yourrepo/acoustic-modem
cd acoustic-modem
pip install -r requirements.txt


Requirements include:

numpy

scipy

matplotlib

sounddevice / pyaudio

ipykernel

jupyter

(optional) numba for speed

▶️ Running the Modems
DTMF Modem
python dtmf_tx.py
python dtmf_rx.py

LoRa-Style Chirp Modem
python lora_tx.py
python lora_rx.py

Mavlink Encoding
python mavlink_encode.py
python mavlink_decode.py

🚀 Future Work

Underwater adaptation (hydrophones, waterproof drivers)

Bidirectional links

Error-correcting codes (Hamming, Reed-Solomon)

Adaptive symbol timing

OFDM-based acoustic PHY

Multi-path compensation

Synchronization sequences for long payloads

🏁 Conclusion

This project successfully delivers a complete acoustic communication system, spanning:

A tone-based DTMF modem (simple, stable, educational)

A LoRa-inspired chirp modem (modern, noise-resistant, scalable)

A Mavlink-style framing layer for structured messaging

Full signal processing pipelines, visualization tools, and modular architecture

It forms a solid foundation for advanced underwater or air-based communication research.
