---
id: 3
title: Survey Windows low-latency audio I/O options
labels: [wayfinder:research]
parent: 0
status: closed
assignee:
blocked_by: []
---

## Question

What are the realistic ways to capture from the Input Mic and write to the Virtual Microphone with minimal latency on Windows 11? Cover WASAPI shared vs exclusive vs low-latency (IAudioClient3) modes and ASIO, and the libraries that wrap them (PortAudio, miniaudio, RtAudio, JUCE, Rust cpal, Python sounddevice). Report achievable round-trip latency figures from primary sources, language support, licenses, and pitfalls (buffer underruns, sample-rate conversion).

Findings: `research/windows-audio-io.md` on branch `research/windows-audio-io`.

## Resolution

**Use WASAPI low-latency shared mode (`IAudioClient3`), not exclusive mode or ASIO. At most, those could be optional.** Microsoft's built-in HD Audio driver supports periods of 128–480 samples (2.66–10 ms at 48 kHz), and shared mode doesn't lock other apps out of the device.

- **Libraries that actually support this mode:** miniaudio (C, public domain or MIT-0), RtAudio (C++, MIT-style), JUCE (C++, AGPLv3 or paid). PortAudio and Python `sounddevice` don't; their shared mode can't go below 20 ms. Rust `cpal` is stuck at about 10 ms. For Rust, use the `windows` crate or wrap miniaudio.
- **Python is a poor fit for the real-time audio path** (GIL, garbage-collector pauses, PortAudio limits). It's acceptable for UI or tooling.
- **The app must handle clock drift** between the Input Mic and the Virtual Microphone itself, using a ring buffer plus a small adaptive resampler. No library does this.
- **Run at the device's native rate (usually 48 kHz)**, because Windows' auto-conversion flag disables low-latency mode.
- **Estimate (not measured):** about 10 ms for the part the app controls. On top of that come VB-CABLE's internal buffer (default about 150 ms, tunable lower, see [Choose the Virtual Microphone driver](001-virtual-mic-driver.md)) and the receiving app's own buffering. VAC (Muzychenko, paid) claims sub-millisecond latency and could be a latency fallback if VB-CABLE can't meet the budget.
- The ASIO SDK has been GPLv3 since Oct 2025, if ASIO is ever added.
