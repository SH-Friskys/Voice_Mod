# The audio thread never waits on anything else

The engine processes the voice in ~10 ms cycles, and any stall on that thread is an audible glitch on the user's live voice. So the audio thread never takes a lock, never allocates memory, never does I/O, and never waits on the UI or a Voice Converter. Control changes reach it through a lock-free mailbox, are picked up at the start of the next cycle, and are smoothed to avoid clicks; Voice Converters exchange audio through lock-free buffers (shared memory when out-of-process). A slow or frozen UI can therefore never make the voice stutter.

## Consequences

- Anything the audio thread needs (buffers, Effect state, resampler) is allocated in `prepare`, before audio starts.
- Swapping Presets or Effect Chains means building the new chain off the audio thread and handing it over atomically, never mutating the live chain.
