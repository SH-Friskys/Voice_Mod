---
id: 10
title: Prototype pitch shift at the latency budget
labels: [wayfinder:prototype]
parent: 0
status: closed
assignee: sean
blocked_by: [7]
---

## Question

Does Signalsmith Stretch, configured with a short block to fit the agreed latency budget, sound acceptable to the user for pitch and formant shift on their own voice? If not, is the cheaper delay-line shifter (lower latency, warbly, no formant control) an acceptable v1 compromise, or must we pursue PSOLA? Build a rough A/B listening test on the user's recorded voice at several block sizes.

Context: [Survey open-source DSP libraries for classic Effects and Noise Suppression](002-dsp-libraries.md), [Set the latency budget](007-latency-budget.md).

Budget from [Set the latency budget](007-latency-budget.md): pitch shift gets about 40–60 ms of the 120 ms ceiling (80 ms target, end to end).

## Resolution

**Use Signalsmith Stretch with a 20 ms block as the default.** It sounds clean on the user's voice, and pitch shift then takes about 20 ms, which puts the total at about 80 ms: 10 (app) + 40 (VB-CABLE) + 10 (Noise Suppression) + 20 (pitch). That hits the latency **target**, not just the ceiling.

- **Listening test:** the user recorded their voice and compared the options in the browser (WASM) build of Stretch, 1.3.2. The options were the default preset (120 ms), the "cheaper" preset (140 ms), 90/60/45/30/20 ms blocks, and a delay-line shifter. Stretch's reported delay equals its block size.
- **Result:** the long blocks (about 90 ms and up) had an underlying metallic sound. The short blocks sounded fine. Formant shift and formant compensation ("keep voice natural") worked well.
- **Block size stays an internal setting.** It can be raised to 30–45 ms per voice if something sounds rough, and that still fits the 120 ms ceiling.
- **Dropped:** the delay-line shifter and PSOLA. Stretch fits the budget and keeps tone/formant control.
- **Caveats:**
  - Tested via the browser build on a recording, not live in the app. Re-listen once the native Rust bindings are in the app; the core is the same.
  - Shorter blocks cost more CPU per second. Keep an eye on it; not a concern at this size.
- **Prototype:** [`prototypes/pitch-shift`](https://github.com/SH-Friskys/Voice_Mod/tree/prototype/pitch-shift/prototypes/pitch-shift) on the throwaway branch `prototype/pitch-shift`. Run it with `python -m http.server 8710 --bind 127.0.0.1` from that folder.
