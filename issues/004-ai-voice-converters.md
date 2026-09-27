---
id: 4
title: Learn how open-source real-time Voice Converters run
labels: [wayfinder:research]
parent: 0
status: open
assignee:
blocked_by: []
---

## Question

How do existing open-source real-time Voice Converters (e.g. RVC, w-okada voice-changer, Beatrice, so-vits-svc) process live audio? Determine: chunk/block sizes and resulting latency; GPU/CPU requirements; model formats and runtimes (PyTorch, ONNX Runtime, DirectML); what language they run in; whether they run in-process or as a separate server; and licenses. The goal is to learn what interface our app must expose so a Voice Converter can later be plugged into the Effect Chain.

Findings: `research/ai-voice-converters.md` on branch `research/ai-voice-converters`.
