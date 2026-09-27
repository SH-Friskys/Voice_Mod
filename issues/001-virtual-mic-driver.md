---
id: 1
title: Choose the Virtual Microphone driver
labels: [wayfinder:research]
parent: 0
status: closed
assignee:
blocked_by: []
---

## Question

Which existing Windows virtual audio driver should the app depend on to expose the transformed voice as a Virtual Microphone? Compare candidates (e.g. VB-Audio VB-Cable, VoiceMeeter, and any maintained open-source drivers) on: license and whether we may bundle/redistribute it or must have the user install it; driver signing and Windows 11 compatibility; added latency; stability; how apps see it (device naming); and whether it can be installed silently. Recommend one, with a fallback.

Findings: `research/virtual-mic-driver.md` on branch `research/virtual-mic-driver`.

## Resolution

**Depend on VB-Audio VB-CABLE (free single cable); fallback Voicemeeter Standard/Banana (same vendor).** The app plays into "CABLE Input"; Discord or a game selects "CABLE Output" as its mic, and the UI must explain those names.

- No open-source driver is viable: Windows 11 only loads kernel drivers signed through Microsoft's Dev Portal. The open-source candidates (VirtualDrivers/Virtual-Audio-Driver, DeckX) are beta, unsigned or too new to trust.
- Supported from XP through Windows 11, including ARM64. Mature: driver 3.3.1.7, package Oct 2024.
- Bundling and silent install are allowed in free or commercial apps if we credit vb-cable.com, call it donationware, and make the expected distributor donation. Installing needs admin rights and a reboot. The silent-install switch is unverified and needs testing on a VM.
- Latency is set by the cable's buffer, about 32 ms tuned at 48 kHz by calculation (not measured). Render at 48 kHz to avoid resampling. Measuring it feeds [Set the latency budget](007-latency-budget.md).
- Design rule: don't hard-code the driver. The output target is any user-selectable playback device, with VB-CABLE preselected when present.
- Surfaced decision: bundle it or have users install it themselves; see [Decide how users get VB-CABLE](009-vb-cable-distribution.md).
