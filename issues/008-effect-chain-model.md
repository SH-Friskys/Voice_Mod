---
id: 8
title: Model Presets, the Effect Chain, and the Voice Converter seam
labels: [wayfinder:grilling]
parent: 0
status: open
assignee:
blocked_by: [4, 6]
---

## Question

How is the Effect Chain structured: fixed order or user-reorderable, and can an Effect appear twice? What exactly does a Preset capture? What interface does a stage in the chain implement so that a Voice Converter can slot in alongside classic Effects without a rewrite?

Also: Presets are shareable files in v1 (see [Design Preset files and sharing](011-preset-files-and-sharing.md)), so a Preset must be self-contained. It must not depend on anything on the machine that made it.
