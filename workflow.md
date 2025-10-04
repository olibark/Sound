

# 🎵 Real-Time Audio Visualisation — Workflow Overview

## **1. Project Concept**
This project creates a **real-time audio visualiser** that converts live sound input into animated graphical patterns.  
- **Python** handles audio capture and analysis.  
- **JavaScript (WebGL)** handles visual rendering.  
- The two communicate via a **local low-latency connection** (WebSocket, UDP, or pipe).

---

## **2. Workflow Diagram**

```text
 ┌───────────────────────────┐
 │        Microphone         │
 │ (System audio interface)  │
 └────────────┬──────────────┘
              │  Raw audio samples
              ▼
 ┌───────────────────────────┐
 │   Python Audio Engine     │
 │ ───────────────────────── │
 │ • Capture stream (sounddevice / pyaudio)
 │ • Buffer small chunks (256–512 samples)
 │ • Apply FFT + smoothing (NumPy)
 │ • Compute metrics:
 │     - Amplitude (RMS)
 │     - Frequency bands
 │     - Peak energy
 └────────────┬──────────────┘
              │  JSON / binary packet
              ▼
 ┌───────────────────────────┐
 │  Local Transport Layer    │
 │ ───────────────────────── │
 │ • WebSocket / UDP / Pipe  │
 │ • Persistent low-latency  │
 │   connection on localhost │
 └────────────┬──────────────┘
              │  Data stream (~60 fps)
              ▼
 ┌───────────────────────────┐
 │  JS Visualisation Engine  │
 │ ───────────────────────── │
 │ • Receive audio metrics   │
 │ • Map values to visuals:  │
 │     - Volume → brightness
 │     - Frequency → colour
 │     - Beat → motion pulse
 │ • GPU render (WebGL / Three.js)
 │ • Maintain 30–60 fps loop │
 └────────────┬──────────────┘
              │
              ▼
 ┌───────────────────────────┐
 │     Display Window        │
 │  (Electron / CEF app)     │
 └───────────────────────────┘
```

---

## **3. Stage Summary**

| Stage | Description | Technology |
|--------|--------------|-------------|
| **Audio Capture** | Record live sound from the system microphone. | Python (`sounddevice` / `pyaudio`) |
| **Processing** | Analyse the waveform using FFT to extract amplitude and frequency data. | Python (`NumPy`) |
| **Streaming** | Send processed data packets to the renderer with minimal delay. | WebSocket / UDP |
| **Rendering** | Draw visuals that react dynamically to audio data. | JavaScript (`WebGL`, `Three.js`) |
| **Display** | Show the animation in a standalone desktop window. | Electron / CEF |

---

## **4. Performance Targets**

| Stage | Target Time | Notes |
|--------|--------------|-------|
| Audio capture | 10–20 ms | System buffer dependent |
| FFT & processing | 1–5 ms | NumPy acceleration |
| Serialisation & transport | < 10 ms | Localhost latency |
| Rendering | 1–5 ms | GPU-accelerated WebGL |
| **Total latency** | **< 50 ms** | Real-time perception threshold |

---

## **5. Design Principles**
- **Separation of Concerns:** Keep audio logic, transport, and graphics modular.  
- **Low-Latency Streaming:** Maintain a persistent connection to avoid polling delay.  
- **GPU Utilisation:** Offload rendering to GPU for smooth performance.  
- **Cross-Platform Compatibility:** Works on Windows, macOS, and Linux.  

---

## **6. Project Milestones**

### **Milestone 1 (MVP)**
- Capture live mic input.  
- Compute amplitude (RMS).  
- Send amplitude to renderer to change colour brightness.  

### **Milestone 2**
- Add FFT frequency analysis.  
- Create bar-style visualiser.  
- Introduce simple particle effects.  

### **Milestone 3**
- Add smoothing and peak detection.  
- Experiment with multiple visual modes.  
- Record or export visualisation sessions.

---

## **7. Next Steps**
- Finalise data exchange format (JSON structure).  
- Prototype audio capture in Python.  
- Build basic WebSocket server.  
- Develop simple JS client to visualise amplitude.  

---

Created by **Oliver Barkham**  
For **A Level Computer Science NEA** — Real-Time Audio Visualisation Project