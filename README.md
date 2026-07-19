# 🎼 [SPACEBOARD]([https://kanishka2023.github.io/SPACEBOARD/])

**Where melody stays on your fingertips**

No keys, no strings, no MIDI controller — just a webcam, your fingers, and Hindustani classical Sargam notes floating in space. Pinch a note to sound it, move your hand to control volume, and play with both hands at once, each holding its own note.

![Made with JavaScript](https://img.shields.io/badge/Made%20with-JavaScript-F7DF1E?logo=javascript&logoColor=black)
![MediaPipe](https://img.shields.io/badge/MediaPipe-Hand%20Tracking-blue)
![Tone.js](https://img.shields.io/badge/Audio-Tone.js-orange)
![Single File](https://img.shields.io/badge/Build-Single%20HTML%20File-brightgreen)

---

## What is this?

SPACEBOARD turns your webcam into an air instrument. It tracks your hands in real time, maps a pinch gesture to note-triggering, and synthesizes Sargam swaras (S R G m P D N — the Indian classical solfège, roughly analogous to Do Re Mi) live in the browser. No installs, no backend — it's a single `.html` file that runs entirely client-side.

I built this because I wanted to see if the raga system — something I've always related to more by feel than by theory — could be played with gesture instead of a keyboard. Turns out, yes. And it's genuinely fun.

## ✋ How to play

1. **Grant camera access** and hold your hand up in frame.
2. **Pinch your index finger and thumb** together near a note circle to sound it.
3. **Move your pinched hand up or down** — up is louder, down is softer. This tracks continuously, even before you pinch, so you can set your level first.
4. **Use both hands** — each one plays and controls volume independently, so you can hold two notes at once.

## 🎛️ Customization

- **Octave range (L / U)** — toggle the lower and upper octave rows independently. At least one stays on at all times.
- **Transpose** — shift every note by up to ±6 semitones from the base scale (C).
- **Note filtering** — tap any note bar to tuck it into a side box and hide it; tap it again to bring it back. Great for practicing with just a few notes at a time, like S R G.
- **Capstone Sa** — a Sa is always rendered one octave above whichever row is currently on top, so you've always got a clean resolution point to land on.

First time playing? Hide everything except S R G, pinch to sound one note, and get a feel for the volume control before turning octaves and transpose back on.

## 🛠️ Tech stack

| Piece | What it does |
|---|---|
| **[MediaPipe Tasks Vision](https://developers.google.com/mediapipe)** (`HandLandmarker`) | Real-time two-hand landmark detection, running on GPU with automatic CPU fallback |
| **[Tone.js](https://tonejs.github.io/)** (v14.8.49) | Two independent `PolySynth` instances — one per hand — sharing a Chorus → Reverb bus for a cohesive sound |
| Vanilla JS + Canvas | All UI, gesture logic, and audio-reactive visuals — zero frameworks, zero build step |

### A few implementation details I'm proud of

- **Pinch detection** is a simple Euclidean distance check between index and middle fingertip landmarks, tuned to a threshold that feels natural without excessive false triggers.
- **Volume is spatial** — each hand's Y-position continuously maps to a decibel range (0 dB at the top of frame, −30 dB near the bottom), smoothed with `rampTo()` so there's no zipper noise.
- **Per-hand color coding** — hand 1 and hand 2 get visually distinct skeleton and note colors, so it's clear which hand is triggering which note when both are active.
- **Glossy, contiguous note bars** — each note's hit-zone renders as a single soft-edged bar using a smootherstep-based gloss mask, rather than hard-edged boxes, so the whole playable row reads as one continuous surface.
- **Canvas mirroring** is handled in JS math (not CSS transforms) so hand landmarks line up exactly with what's drawn, with no coordinate drift.

## 🚀 Running it locally

Since it's a single HTML file with no build step, all you need is a local server (opening the file directly won't work — camera access requires a proper origin):

```bash
git clone https://github.com/<your-username>/SPACEBOARD.git
cd SPACEBOARD
python -m http.server 8080
```

Then open `http://localhost:8080` in a browser that supports `getUserMedia` (Chrome or Edge recommended), grant camera access, and start playing.

## 🔭 What's next

- **Portamento / pitch glide** — ROLI Seaboard-style smooth pitch bending between notes, instead of discrete note snapping.
- Demo GIFs and an embedded walkthrough video in the in-app Learn modal.

## 🙏 Why this exists

I wanted to create Bass-line for my Sitar compositions, since this Indian instrument mostly covers the mid and high pitch regions, so just thought of making something at the intersection of two things I love — Indian classical music and building things — mostly because I wanted to know if it was possible, and the process of getting hand-tracking, audio synthesis, and a raga-aware note system to actually talk to each other in real time was genuinely one of the most rewarding builds I've done. If you try it out and something about the raga mapping or the gesture feel resonates (or doesn't), I'd love to hear about it.

---
