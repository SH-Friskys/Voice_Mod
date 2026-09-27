---
id: 10
title: Prototype pitch shift at the latency budget
labels: [wayfinder:prototype]
parent: 0
status: open
assignee:
blocked_by: [7]
---

## Question

Does Signalsmith Stretch, configured with a short block to fit the agreed latency budget, sound acceptable to the user for pitch and formant shift on their own voice? If not, is the cheaper delay-line shifter (lower latency, warbly, no formant control) an acceptable v1 compromise, or must we pursue PSOLA? Build a rough A/B listening test on the user's recorded voice at several block sizes.

Context: [Survey open-source DSP libraries for classic Effects and Noise Suppression](002-dsp-libraries.md), [Set the latency budget](007-latency-budget.md).
