---
id: 7
title: Set the latency budget
labels: [wayfinder:grilling]
parent: 0
status: open
assignee:
blocked_by: [3, 4]
---

## Question

What is the maximum acceptable end-to-end delay (Input Mic to Virtual Microphone) with v1 classic Effects, and what relaxed budget is acceptable when a Voice Converter is active?

Inputs: the app's own path is about 10 ms ([Survey Windows low-latency audio I/O options](003-windows-audio-io.md)). VB-CABLE adds roughly 32–150 ms depending on tuning ([Choose the Virtual Microphone driver](001-virtual-mic-driver.md)). Voice Converters add about 50–430 ms ([Learn how open-source real-time Voice Converters run](004-ai-voice-converters.md)). If VB-CABLE can't meet the budget, VAC (paid) is the lower-latency alternative. Pitch-shift algorithm latency comes from [Survey open-source DSP libraries for classic Effects and Noise Suppression](002-dsp-libraries.md), but the budget doesn't need to wait for it.
