# Voice Modulator

A Windows app that transforms the user's microphone voice in real time and presents the result to other apps as a microphone.

## Language

### Signal path

**Input Mic**:
The physical microphone the user speaks into.
_Avoid_: Source, device

**Virtual Microphone**:
A software audio device, provided by a separately installed driver, that other apps (Discord, games, OBS) select as their mic to receive the transformed voice.
_Avoid_: Virtual cable, fake mic, output device

**Effect**:
One transformation applied to the voice, such as pitch shift, robot, or reverb.
_Avoid_: Filter, plugin, mod

**Effect Chain**:
The ordered set of Effects the voice passes through between the Input Mic and the Virtual Microphone.
_Avoid_: Pipeline, rack

**Control**:
A user-adjustable setting on an Effect (a slider or button) whose changes take effect immediately, without restarting.
_Avoid_: Parameter, knob

**Latency**:
The delay between speaking into the Input Mic and the transformed voice arriving at the app listening to the Virtual Microphone.
_Avoid_: Lag, delay

**Voice Converter**:
An AI model that re-synthesizes speech to sound like a different target speaker, as opposed to a classic Effect.
_Avoid_: AI voice, voice clone

### User features

**Preset**:
A named, saved combination of Effects and Control values that can be switched to in one action.
_Avoid_: Voice, profile, patch

**Bypass**:
The state in which the voice reaches the Virtual Microphone unaltered.
_Avoid_: Mute, off

**Monitoring**:
Playing the transformed voice back to the user's own headphones so they can hear themselves.
_Avoid_: Sidetone, listen-back

**Noise Suppression**:
A toggleable stage that removes background noise (keyboard, hum) from the voice.
_Avoid_: Noise gate, denoise
