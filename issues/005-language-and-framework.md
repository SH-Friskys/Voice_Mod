---
id: 5
title: Pick the language and framework
labels: [wayfinder:grilling]
parent: 0
status: closed
assignee: sean
blocked_by: [1, 2, 3, 4]
---

## Question

Which programming language, audio I/O library, and UI framework should the app be built with? Constraints: Claude writes and maintains it, latency is critical, the chosen open-source DSP libraries must be callable, and a Voice Converter (likely Python/ONNX-based) must be pluggable later.

Research inputs: the real-time audio path rules out Python. Low-latency WASAPI shared mode is available via miniaudio (C), RtAudio (C++), or the `windows` crate / miniaudio wrapper (Rust). JUCE is ruled out by its AGPL licence. DSP picks (Signalsmith Stretch, RNNoise, Faust-generated code) all have C++ and Rust paths. Voice Converters are mostly Python + PyTorch in a separate worker process. See the Decisions so far on the map.

## Resolution

**Rust + Tauri v2, with a React + TypeScript UI, and WASAPI called directly.** Decided with the user in a grilling session.

- **Language:** Rust (stable, MSVC toolchain) for everything except the UI's contents. Chosen for memory safety in real-time code that Claude will keep changing, a one-command build, and Rust paths for every DSP pick.
- **App shell:** Tauri v2. A Rust backend owns the audio engine, and the window renders a web UI. It all runs in one process.
- **UI:** React + TypeScript. Polished and custom-styled, but intuitive first.
- **Audio I/O:** WASAPI low-latency shared mode (`IAudioClient3`) at 48 kHz, called through Microsoft's `windows` crate. We don't use a miniaudio wrapper, because the Rust wrappers are stale and we want to own device handling. A ring buffer and adaptive resampler, written by us, correct drift between the Input Mic and the Virtual Microphone.
- **DSP from Rust:**
  - Signalsmith Stretch through its Rust bindings.
  - RNNoise as `nnnoiseless`.
  - Hand-written Effects in plain Rust.
  - Reverb from Faust-generated Rust or FunDSP. The exact pick belongs to [Define the v1 Effect list and each Effect's Controls](006-v1-effect-list.md).
- **Tray app:**
  - The voice keeps being transformed while the app is in the tray. Quitting happens from the tray.
  - **Close-to-tray** is a setting, on by default. The first close shows a one-time "still running in the tray" notice.
  - **Start with Windows** is a setting, off by default. It starts the app minimised to the tray.
- **Real-time rule:** the audio thread never waits. It takes no locks, no allocations and no I/O. Controls arrive via a lock-free mailbox and are smoothed. See [ADR 0001](../docs/adr/0001-audio-thread-never-waits.md).
- **Voice Converter compatibility:** a future converter runs out-of-process (likely a Python worker) and exchanges audio with the Rust engine over shared memory, per [Learn how open-source real-time Voice Converters run](004-ai-voice-converters.md). Python never enters the audio engine.
- **Global hotkeys:** the default route is Tauri's global-shortcut plugin. Behaviour inside games is still on the map's Not yet specified list.
