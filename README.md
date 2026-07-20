# 🎼 SPACEBOARD

***Where melody stays on your fingertips...***

[![Play Now](https://img.shields.io/badge/!_Play_Now_!-4B0082?style=for-the-badge)](https://kanishka2023.github.io/SPACEBOARD/)

No keys, no strings, no MIDI controller — just a webcam, your fingers, and Hindustani classical Sargam notes floating in space. Pinch a note to sound it, move your hand to control volume, and play with both hands at once, each holding its own note.

![Made with JavaScript](https://img.shields.io/badge/Made%20with-JavaScript-F7DF1E?logo=javascript&logoColor=black)
![Mediapipe](https://img.shields.io/badge/Hand_Tracking-MediaPipe-blue)
![Tone.js](https://img.shields.io/badge/Audio-Tone.js-red)

---

## What is this?

SPACEBOARD turns your webcam into an _**Musical Air Instrument**_. It tracks your hands in real time, maps a pinch gesture to note-triggering, and synthesizes Sargam swaras (S R G m P D N — the Indian classical solfège, analogous to Do Re Mi...) live in the browser.

I built this because I wanted to see if the ROLI Seaboard's note-bending concept could be played with gesture. Turns out, yes. And it's genuinely fun.

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

## 🔭 What's next?

- **Portamento / pitch glide** — ROLI Seaboard-style smooth pitch bending between notes, instead of discrete note snapping.
- Demo GIFs and an embedded walkthrough video in the in-app Learn modal.

## 🙏 Story Behind...

I wanted to create bass-line for my Sitar compositions, since this Indian instrument mostly covers the mid and high pitch regions, so just thought of making something at the intersection of two things I love — Indian classical music and building things — mostly because I wanted to know if it was possible, and the process of getting hand-tracking, audio synthesis, and a raga-aware note system to actually talk to each other in real time was genuinely one of the most rewarding builds I've done. If you try it out and something about the raga mapping or the gesture feel resonates (or doesn't), I'd love to hear about it.

---
