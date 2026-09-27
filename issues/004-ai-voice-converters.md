---
id: 4
title: Learn how open-source real-time Voice Converters run
labels: [wayfinder:research]
parent: 0
status: closed
assignee:
blocked_by: []
---

## Question

How do existing open-source real-time Voice Converters (e.g. RVC, w-okada voice-changer, Beatrice, so-vits-svc) process live audio? Determine: chunk/block sizes and resulting latency; GPU/CPU requirements; model formats and runtimes (PyTorch, ONNX Runtime, DirectML); what language they run in; whether they run in-process or as a separate server; and licenses. The goal is to learn what interface our app must expose so a Voice Converter can later be plugged into the Effect Chain.

Findings: `research/ai-voice-converters.md` on branch `research/ai-voice-converters`.

## Resolution

**Voice Converters are chunked, heavy, mostly Python and PyTorch, and increasingly run out-of-process. Our Effect interface must allow for all of that from day one.**

- **Latency:** chunked converters run about 2× chunk plus compute. RVC claims about 170 ms (90 ms on ASIO) and Seed-VC measures 430 ms. Beatrice 2 is the outlier: 10 ms streaming on one CPU thread, about 50 ms measured. This feeds [Set the latency budget](007-latency-budget.md).
- **Runtimes:** Python + PyTorch (CUDA), with ONNX Runtime + DirectML for AMD and Intel GPUs. DirectML is in sustained engineering, and new Microsoft work has moved to WinML. Beatrice is C++ over a closed, prebuilt library.
- **Hosting trend:** RVC's own plug-in (Aug 2026) uses a C++ front end and a Python worker process connected by Windows shared memory and events. The audio thread only touches lock-free buffers. w-okada and Applio use Socket.IO or WebSocket servers.
- **Code licences:** RVC, w-okada, Applio, DDSP-SVC and Beatrice are MIT. Avoid so-vits-svc (AGPL) and Seed-VC (GPL, archived).
- **Recommended seam:** every Effect implements `prepare(sampleRate, maxBlock, channels)` and `process(in, out, frames)`, and reports `latencySamples()` (which can change), status (Loading, Error, Overloaded) and per-block compute time. A generic worker-backed adapter hosts a Voice Converter in-process on its own thread, or out-of-process over shared memory. The chain runs at 48 kHz float32, and converters resample internally. User-imported model files (RVC `.pth`) can run code when loaded, so they must load out-of-process. This feeds [Model Presets, the Effect Chain, and the Voice Converter seam](008-effect-chain-model.md) and [Pick the language and framework](005-language-and-framework.md).
