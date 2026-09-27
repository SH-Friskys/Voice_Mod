---
id: 1
title: Choose the Virtual Microphone driver
labels: [wayfinder:research]
parent: 0
status: open
assignee:
blocked_by: []
---

## Question

Which existing Windows virtual audio driver should the app depend on to expose the transformed voice as a Virtual Microphone? Compare candidates (e.g. VB-Audio VB-Cable, VoiceMeeter, and any maintained open-source drivers) on: license and whether we may bundle/redistribute it or must have the user install it; driver signing and Windows 11 compatibility; added latency; stability; how apps see it (device naming); and whether it can be installed silently. Recommend one, with a fallback.

Findings: `research/virtual-mic-driver.md` on branch `research/virtual-mic-driver`.
