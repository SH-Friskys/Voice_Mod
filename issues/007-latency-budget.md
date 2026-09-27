---
id: 7
title: Set the latency budget
labels: [wayfinder:grilling]
parent: 0
status: closed
assignee: sean
blocked_by: [3, 4]
---

## Question

What is the maximum acceptable end-to-end delay (Input Mic to Virtual Microphone) with v1 classic Effects, and what relaxed budget is acceptable when a Voice Converter is active?

Inputs: the app's own path is about 10 ms ([Survey Windows low-latency audio I/O options](003-windows-audio-io.md)). VB-CABLE adds roughly 32–150 ms depending on tuning ([Choose the Virtual Microphone driver](001-virtual-mic-driver.md)). Voice Converters add about 50–430 ms ([Learn how open-source real-time Voice Converters run](004-ai-voice-converters.md)). If VB-CABLE can't meet the budget, VAC (paid) is the lower-latency alternative. Pitch-shift algorithm latency comes from [Survey open-source DSP libraries for classic Effects and Noise Suppression](002-dsp-libraries.md), but the budget doesn't need to wait for it.

## Resolution

**Classic Effects: aim for 80 ms, never more than 120 ms. With a Voice Converter active: never more than 250 ms.** Decided with the user in a grilling session.

- **The main concern is conversation lag on Discord.** The user also records the voice in an existing app for dubbing, but that app handles timing, so lip sync doesn't constrain the budget. Dubbing is out of scope.
- **What's measured:** Latency end to end, from the Input Mic to what "CABLE Output" delivers to the receiving app. Discord's network delay is excluded. It's measured with a loopback test, not calculated.
- **Rough split of the 120 ms ceiling:**

  | Part | Share |
  | --- | --- |
  | Our audio path (WASAPI + drift buffer) | ≤ 10 ms |
  | VB-CABLE | ≤ 40 ms |
  | Noise Suppression (RNNoise) | ~10 ms |
  | Pitch shift | the remainder, ~40–60 ms; tested by [Prototype pitch shift at the latency budget](010-pitch-shift-prototype.md) |
  | Other classic Effects | ~0 ms |

- **Voice Converters:** 250 ms fits RVC (~170 ms) and Beatrice (~50 ms). Slower converters such as Seed-VC (~430 ms) aren't supported.
- **Steadiness:** Latency stays constant while the user talks. It changes only when Effects or Presets change, and it always uses the lowest possible delay (no padding to the worst case). A short crossfade hides the jump.
- **The UI shows the current Latency**, summed from each Effect's reported `latencySamples()` plus the I/O path.
- **If VB-CABLE exceeds its share:** tune its buffer first, then try Voicemeeter's virtual inputs, then recommend VAC (paid, optional). The output device is already user-selectable, so no code changes are needed.
- **Monitoring** is used occasionally to check the sound, not to talk over, so it needs no separate, tighter budget.
