# Velvet Noise Mixer v4.12

Experimental browser-based velvet noise generator for producing adjustable, pleasant, living noise textures. The current use case is gentle sound design for child sleep support, with emphasis on controllable softness, movement, and non-harsh noise.

## Version

**v4.12**

Test focus: multi-buffer velvet noise summing with adjustable buffer count.

This version explores whether summing several independent velvet-noise buffers makes the texture smoother and more natural while still allowing rougher grain at low density values.

## Origin

This v4.x branch was rebuilt from the earlier working **v2.9 Velvet Noise Mixer with LFO** version. The goal of the refactor is to move from a fixed slider demo toward a more reusable voice-engine architecture.

## Main features

- Browser-based Web Audio implementation.
- Velvet-noise buffer generation.
- Adjustable lowpass frequency.
- Adjustable density.
- Adjustable Q.
- Volume and pan controls.
- LFO depth and LFO frequency for lowpass modulation.
- EQ compensation based on frequency.
- Shared stereo reverb.
- Pulse test button using an internal envelope gain.
- Mute mode that allows pulse testing separately from continuous noise.
- Adjustable **Buffer Count** from 1 to 10.

## Buffer Count test

The main new test in v4.12 is the `Buffer Count` slider.

Each voice can now sum multiple independent velvet-noise buffers. Each buffer:

- has its own random impulse pattern,
- loops continuously,
- starts with a different random offset,
- is scaled by `1 / sqrt(bufferCount)` to keep the approximate RMS level stable.

Expected behavior:

- Low buffer count gives more binary, sparse, grainy velvet noise.
- Higher buffer count gives a smoother, more conventional noise texture.
- Low density still preserves audible grain even with several buffers.

## Density model

The current density model is sample-based velvet noise:

```js
sample = 0, +A, or -A
```

The probability of an impulse is controlled by the density slider. Each impulse currently has fixed amplitude and random polarity. When several buffers are summed, overlapping impulses naturally create a stepped amplitude distribution.

## Pulse test

The Pulse button triggers a short amplitude envelope. This is intended as an early test for future short rustle/transient events.

The pulse envelope is scheduled on the Web Audio timeline rather than with JavaScript timers, so the shape is stable even if the UI trigger timing is loose.

## Architecture direction

The project is moving toward a procedural sound voice architecture:

```text
AudioEngine
  ├─ AudioContext
  ├─ master output
  ├─ shared reverb
  └─ voice/layer audio routing

Layer / future VelvetVoice
  ├─ params
  ├─ velvet buffers
  ├─ filter
  ├─ LFO
  ├─ envelope gain
  ├─ volume / mute / pan
  └─ pulse trigger
```

The longer-term idea is that each voice can later be controlled through an API, while the UI may remain a simple editor for the selected voice.

## Current limitations

- Still mostly a one-voice test UI.
- Buffer count changes rebuild the voice and can cause a small interruption.
- No preset save/load UI yet.
- No external API exposed yet.
- No automatic buffer regeneration during playback yet.
- Reverb is shared, not per-voice.

## Possible next steps

- Rename `Layer` toward `VelvetVoice`.
- Add `reverbSend` per voice.
- Add a small internal API skeleton.
- Add selected-voice editor model instead of many full layer cards.
- Test soft random impulse amplitude distribution.
- Add slow texture modulation over 50 ms to 500 ms timescales.
- Add occasional silent buffer regeneration when individual buffer gain is low.
- Add lightweight presets for sleep/rustle/soft-air/noise-bed use cases.

## Commit message

```bash
Refactor velvet noise mixer toward multi-buffer voice texture test v4.12

- Continue v4.x refactor from the working v2.9 LFO baseline
- Add adjustable Buffer Count control from 1 to 10
- Generate independent velvet-noise buffers per voice
- Start each buffer with a random playback offset
- Sum buffers with 1/sqrt(bufferCount) gain scaling
- Preserve low-density grain while allowing smoother high-density noise
- Keep pulse envelope test and mute-compatible pulse playback
- Update version description under the title
```
