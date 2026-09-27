# PROTOTYPE — pitch shift listening test

Throwaway. Answers [Prototype pitch shift at the latency budget](../../issues/010-pitch-shift-prototype.md): does pitch shift sound acceptable within the ~40–60 ms it gets from the latency budget?

Run from this folder, then open http://127.0.0.1:8710 in Edge or Chrome and allow the microphone:

    python -m http.server 8710 --bind 127.0.0.1

It records your voice, then plays it through Signalsmith Stretch (browser/WASM build 1.3.2, MIT, in `vendor/`) at several block sizes, and through a hand-written delay-line shifter. Each option shows the delay it reports. Rate them and paste the summary back.

Measured in the browser pane: Stretch's reported delay equals its block size (default preset 120 ms, "cheaper" 140 ms). The delay-line shifter's average delay is half its window.
