---
id: 2
title: Survey open-source DSP libraries for classic Effects and Noise Suppression
labels: [wayfinder:research]
parent: 0
status: closed
assignee:
blocked_by: []
---

## Question

Which open-source libraries can supply real-time implementations of the classic Effects (pitch shift, formant/tone shift, robot via ring-mod or vocoder, echo/reverb, radio/telephone EQ, distortion) and of Noise Suppression (e.g. RNNoise, DeepFilterNet)? For each candidate (e.g. Rubber Band, SoundTouch, Signalsmith Stretch, WORLD, open effect collections): license (GPL vs permissive, and what that forces on our app), per-block latency, CPU cost, language bindings, and maintenance status. Output a shortlist per Effect.

Findings: `research/dsp-libraries.md` on branch `research/dsp-libraries`.

## Resolution

**Keep the app permissively licensed. Use Signalsmith Stretch for pitch and formant, RNNoise for Noise Suppression, Faust-generated reverb, and hand-write the rest.** No numbers were benchmarked; all figures come from docs or source.

| Effect | Pick | Notes |
| --- | --- | --- |
| Pitch + formant/tone | Signalsmith Stretch (MIT, single C++ header; Rust and Python bindings) | Default config adds about 120 ms (derived from source). Needs a shorter block config; sound quality at 40–60 ms is unverified. Fallbacks: delay-line shifter (Faust `transpose_windowed` or DaisySP: low latency, warbly, no formant control) or PSOLA via Q (Boost). |
| Robot (ring-mod), echo, radio/telephone EQ, distortion | Hand-written (biquads from the RBJ/W3C cookbook) | Zero latency, no dependencies. Faust `ve.vocoder` is optional for a vocoder-style robot. |
| Reverb | Faust `zita_rev1` or `freeverb`, compiled to C++/Rust and committed | Permissively licensable output. FunDSP (MIT/Apache) if Rust. **Trap:** about 31 Faust functions (all compressors, `vital_rev`) are GPL/AGPL; the build should check imports. |
| Noise Suppression | RNNoise (BSD) | About 10 ms, 480-sample frames at 48 kHz, very cheap. Rust `nnnoiseless`, Python `pyrnnoise`. DeepFilterNet (40 ms, stale since 2023) is a later option at most. The WebRTC NS alternative is unverified. |

**Avoid:** Rubber Band (GPL/commercial, 50 ms or more), SoundTouch (LGPL, about 100 ms), and JUCE (AGPL) and Pedalboard (GPL), which would force the whole app to be GPL. WORLD is offline-oriented, so it fits future Voice Converter work, not v1.

Surfaced: [Prototype pitch shift at the latency budget](010-pitch-shift-prototype.md).
