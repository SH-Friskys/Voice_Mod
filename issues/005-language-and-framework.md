---
id: 5
title: Pick the language and framework
labels: [wayfinder:grilling]
parent: 0
status: open
assignee:
blocked_by: [1, 2, 3, 4]
---

## Question

Which programming language, audio I/O library, and UI framework should the app be built with? Constraints: Claude writes and maintains it, latency is critical, the chosen open-source DSP libraries must be callable, and a Voice Converter (likely Python/ONNX-based) must be pluggable later.

Research inputs: the real-time audio path rules out Python. Low-latency WASAPI shared mode is available via miniaudio (C), RtAudio (C++), or the `windows` crate / miniaudio wrapper (Rust). JUCE is ruled out by its AGPL licence. DSP picks (Signalsmith Stretch, RNNoise, Faust-generated code) all have C++ and Rust paths. Voice Converters are mostly Python + PyTorch in a separate worker process. See the Decisions so far on the map.
