---
id: 11
title: Design Preset files and sharing
labels: [wayfinder:grilling]
parent: 0
status: open
assignee:
blocked_by: [6, 8]
---

## Question

Shareable Presets are a v1 feature, like Voicemod's community voices. A user can export a Preset as a file, send it to a friend, and import it. No outside code runs; a Preset only arranges our own Effects and Control values. How does that work?

- **File format:** likely human-readable JSON. What goes in it: name, author, description, icon?
- **Storage:** where Presets live on disk, and how built-in Presets differ from user-made and imported ones.
- **Import:** how the user imports a file (a button, drag and drop, or double-clicking a file).
- **Compatibility:**
  - a Preset made by a newer app version, or one that names an Effect or Control this version lacks;
  - a Control value out of range;
  - name clashes on import.
- **Safety:** validating untrusted files before they touch the Effect Chain.

Context: the user chose shareable Presets over third-party audio plugins (VST3/CLAP) and AI voice models. The answer to [Model Presets, the Effect Chain, and the Voice Converter seam](008-effect-chain-model.md) fixes what a Preset captures; this ticket fixes how it's saved and shared.
