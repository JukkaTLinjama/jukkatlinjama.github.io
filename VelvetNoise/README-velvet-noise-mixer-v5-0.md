# Velvet Noise Mixer v5.0

Milestone: API-documented lightweight velvet-noise ambience engine.

Date: 2026-05-22

## Overview

Velvet Noise Mixer v5.0 is a single-file HTML + JavaScript Web Audio prototype for soft granular noise textures, sleeping ambience, living background sounds, and future interaction-driven ambience scenes.

This version marks the transition from a UI-only sound experiment into a small reusable browser audio engine with a documented internal API:

```js
window.velvetNoiseApi
```

The goal is still to keep the system lightweight:

- no external frameworks
- no ES module complexity yet
- single HTML file for fast iteration
- serializable voice configs
- runtime-only Web Audio nodes
- API usable from future demo pages or app shells

## Main milestone

v5.0 documents the internal API directly in the code and freezes the current architecture as a usable base for the next app direction.

The current prototype is now suitable as an engine lab for a future Elsi ambience app.

Suggested split going forward:

```txt
Velvet Noise Mixer v5.0 = engine lab / technical tester
Elsi app v0.1          = user-facing scene shell on top of the API
```

## Architecture

Current structure:

```txt
VelvetEngine
  ├─ shared master/output chain
  ├─ shared stereo reverb
  ├─ voice list
  └─ public API methods

VelvetVoice
  ├─ serializable params
  ├─ runtime audio nodes
  ├─ generated velvet buffers
  ├─ triggerEnvelope()
  ├─ generateBuffers()
  └─ updateParam()
```

The important design rule is that voice parameters remain copyable and serializable, while Web Audio nodes are created only at runtime.

## Public internal API

The documented browser API is:

```js
window.velvetNoiseApi.start()
window.velvetNoiseApi.stop()
window.velvetNoiseApi.addVoice(config)
window.velvetNoiseApi.removeVoice(id)
window.velvetNoiseApi.cloneVoice(id)
window.velvetNoiseApi.updateVoiceParam(id, param, value)
window.velvetNoiseApi.triggerVoice(id, options)
window.velvetNoiseApi.getState()
window.velvetNoiseApi.exportConfig()
window.velvetNoiseApi.loadConfig(config)
```

This API is intended for use by the built-in UI and by future demo/app code.

The UI should call the API instead of directly controlling the audio objects whenever possible.

## Voice config

A voice config is intended to be serializable:

```js
{
  params: {
    frequency,
    bufferCount,
    volume,
    pan,
    density,
    q,
    lfoDepth,
    lfoFrequency,
    textureDrift
  },
  isMuted,
  customName
}
```

Audio nodes, buffers, oscillators, filters, panners, gains, and convolver state are not stored in the config.

## DSP status

Current core sound experiment:

- multiple independent velvet-noise buffers per voice
- summed buffer playback
- per-buffer gain scaled by `1 / sqrt(bufferCount)`
- randomized playback offsets
- randomized buffer loop lengths
- density compensation for sparse impulse textures
- frequency-first UI
- lowpass filtering per voice
- shared stereo reverb
- 10 Hz output high-pass
- pulse envelope test for transient/noise events

The sound goal is to move from sparse velvet impulses toward softer natural granular noise while preserving texture at low density.

## Texture drift

v4.15–v4.21 introduced slow texture drift:

- `textureDrift` parameter per voice
- slow randomized LFO rate/depth changes
- drift target shown visually as ghost markers behind LFO sliders
- non-synchronized drift timing using randomized delays
- slower LFO settings can persist longer than fast ones

The drift system is intentionally a meta-modulation layer, not a second audio-rate LFO.

## Important fixes from this development sequence

The v4.12 → v5.0 sequence fixed several important behavior issues:

- added internal API architecture
- decoupled UI control from engine control
- added export/load config support
- added preset-ready structure
- added 10 Hz output high-pass
- extended frequency range to 16 kHz
- reduced excessive bass buildup in the reverb impulse response
- added texture drift modulation
- added ghost drift markers behind LFO sliders
- randomized buffer loop lengths and playback offsets
- fixed copied voice startup after unmute
- fixed LFO startup flutter

The LFO flutter issue was caused by oscillator startup behavior: the LFO had to be initialized at the correct frequency before being started, and LFO depth had to fade in from zero. Otherwise a strong startup modulation sweep could be heard, especially with high `lfoDepth`.

## Current known limitations

The prototype is still an engine lab, not yet a finished app.

Known limitations:

- UI is still technical and slider-heavy
- no polished scene system yet
- no long-timescale scene evolution yet
- no dedicated pattern scheduler yet
- no app shell / PWA behavior yet
- no safety-oriented sleep timer yet
- no external config file workflow yet

## Suggested next direction: Elsi app

The next development direction should be a new app shell built on top of the v5.0 API.

Recommended first app file:

```txt
elsi-ambience-app-v0-1.html
```

Do not replace the engine lab yet. Keep Velvet Noise Mixer v5.0 as the technical reference and build Elsi as a separate user-facing layer.

## Recommended next technical step

The most important next technical step is a pattern / scene engine above the audio API.

Example concepts:

```txt
Scene
  ├─ voices
  ├─ automation
  ├─ pattern controllers
  └─ slow ambience evolution
```

Possible scene types:

- Soft Sleep Bed
- Walking / Footstep Texture
- Rain-like Soft Granules
- Breathing Room Tone
- Calm Night Texture

## Walking mode concept

A walking mode should not be hardcoded into the audio engine.

Better structure:

```txt
WalkingPatternController
  ├─ leftFoot voice id
  ├─ rightFoot voice id
  ├─ tempo / step interval
  ├─ timing jitter
  ├─ gain jitter
  └─ alternating trigger logic
```

The controller would call:

```js
window.velvetNoiseApi.triggerVoice(leftId, options)
window.velvetNoiseApi.triggerVoice(rightId, options)
```

Left and right foot voices should have slightly different:

- density
- cutoff / frequency
- Q
- pan
- bufferCount
- envelope peak/release

This keeps the audio engine generic and lets future scenes control it through the API.

## Design principle going forward

Keep the engine small and generic.

Put behavior such as walking, breathing, rain, and sleep-scene evolution into controllers above the engine.

This avoids turning the DSP layer into a special-case app script.

## Commit message

```txt
v5.0: document Velvet Noise Mixer API milestone

- Document window.velvetNoiseApi directly in code
- Mark current architecture as reusable internal browser API
- Keep voice configs serializable and runtime audio nodes separate
- Preserve v4.21 DSP behavior without sound changes
- Establish v5.0 as the engine-lab base for future Elsi ambience app
```
