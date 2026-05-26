# Vacuum Sample Lab v1.9

## Status

Vacuum Sample Lab v1.9 is a single-file Web Audio prototype for building a vacuum cleaner style sound from one editable motor loop plus synthetic runtime noise layers.

This version is intended as the final lab/reference version before extracting a smaller engine for the Elsi app.

## Main concept

The tonal motor sound should stay as one morphing sample loop. Similar duplicated motor loops are avoided because they easily create beating and unstable phasing.

The runtime sound is built from:

- motor loop sample
- synthetic air noise
- velvet low layer
- velvet high layer
- harmonic noise layer

The edited motor clip can be exported separately as a WAV asset. Runtime noise layers are not included in the exported motor clip.

## Current features

### Motor sample workflow

- Loads default sample from `assets/test-loop.wav` when available.
- Supports file input.
- Supports microphone recording.
- Supports using the recording as the loop source.
- Supports exporting the raw recording.
- Supports clip selection from the source buffer.
- Supports edited clip export as WAV.
- Supports live clip preview.

### Clip editor

- Start/end loop selection.
- Loop length display.
- Output length display.
- Fixed outside-handle crossfade.
- Period estimation from selected clip.
- Snap loop end to integer motor periods.
- Output gain and output RMS/peak display.

The clip editor is intentionally still present in the Lab version. It should not be removed before the Elsi integration unless a separate production-only file is created.

### Runtime layers

- Motor loop source.
- Two decorrelated synthetic air noise buffers.
- Harmonic noise buffer.
- Velvet low synthetic layer.
- Velvet high synthetic layer.

Each main layer has independent public controls.

### Load behaviour

The `Load` control is signed:

- `-50%` slows motor playback down.
- `+50%` speeds motor playback up.
- Playback rate range is approximately `0.50 .. 1.50`.

A small Load LFO adds slow life to the vacuum behaviour.

Default Load LFO depth is intentionally subtle: `4%`.

The LFO cycle period varies slowly between about `3 .. 8 s`, avoiding a fixed mechanical breathing tempo.

## Public API

The intended public control API is:

```js
window.vacuumSampleLabApi.updateControl({
  loadSigned01: 0.0,
  loadLfoDepth01: 0.04,
  motorVolume01: 0.40,
  airVolume01: 0.65,
  velvetLow01: 0.70,
  velvetHigh01: 0.60,
  harmonicNoise01: 0.75,
  masterVolume01: 0.70
});
```

Available API methods:

```js
window.vacuumSampleLabApi.start();
window.vacuumSampleLabApi.stop();
window.vacuumSampleLabApi.getState();
window.vacuumSampleLabApi.updateControl(params);
```

## Layer notes

### Motor

The motor loop is the only tonal looped source. It is pitch/load morphed via playback rate.

### Air

The air layer is broad synthetic turbulence. It provides the continuous airflow bed.

### Velvet low / high

The velvet layers use dense, softened overlapping grains. The parameter previously called density was clarified internally as impulses per second, because it is not the same as the normalized/probability-style density used in the earlier Velvet Noise Mixer API.

The current velvet layers are smoother than earlier versions and are intended to avoid a crackly or rakeinen texture.

### Harmonic noise

Harmonic noise has its own independent slider/API control. It is no longer hidden behind the air volume control.

## Metering

The former air RMS badge now represents the combined runtime flow-noise estimate:

- air noise
- harmonic noise
- velvet low
- velvet high

This is displayed as `Flow RMS`.

The value is an estimate based on source RMS assumptions and current mapped gains. It is useful for balancing layers, but it is not a true realtime output measurement.

## Architecture notes

v1.9 keeps everything in one HTML file, but the JavaScript has been organized into logical sections:

- UI references
- shared state
- audio graph
- runtime layer synthesis
- source helpers
- control modulation/mapping
- runtime transport
- sample loading/recording
- clip editor/export
- metering
- public API
- UI event binding

This keeps the lab easy to run as a standalone browser file while making later engine extraction safer.

## Recommended Elsi extraction plan

Do not move the entire Lab into Elsi.

Extract only the engine-facing parts:

- audio graph setup
- motor sample loading
- runtime layer synthesis
- control modulation/mapping
- transport start/stop
- public API

Keep the Lab version as a separate tool for editing and exporting motor assets.

Elsi should likely use:

- a pre-exported motor asset from the Lab
- the runtime layer engine
- the public control API
- no clip editor UI

## Known limitations

- Flow RMS is estimated, not measured from the actual mixed output.
- Runtime noise layers are regenerated when playback starts.
- The velvet layers are still subjective and should be tuned by listening.
- The Lab is not yet split into separate JS files.
- The editor is useful for asset creation but should not be part of the final baby app UI.

## Suggested next step

Commit this Lab milestone, then start a new Elsi integration session by extracting a minimal `vacuumEngine` from v1.9.
