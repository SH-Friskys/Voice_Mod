---
id: 3
title: Survey Windows low-latency audio I/O options
labels: [wayfinder:research]
parent: 0
status: open
assignee:
blocked_by: []
---

## Question

What are the realistic ways to capture from the Input Mic and write to the Virtual Microphone with minimal latency on Windows 11? Cover WASAPI shared vs exclusive vs low-latency (IAudioClient3) modes and ASIO, and the libraries that wrap them (PortAudio, miniaudio, RtAudio, JUCE, Rust cpal, Python sounddevice). Report achievable round-trip latency figures from primary sources, language support, licenses, and pitfalls (buffer underruns, sample-rate conversion).

Findings: `research/windows-audio-io.md` on branch `research/windows-audio-io`.
