# Velvet Noise Mixer v4.3

Experimental browser-based velvet noise generator for producing adjustable, pleasant, slowly living noise. The practical target use case is a soft sound bed for helping a child fall asleep.

## Background

This version was rebuilt from the working `v2.9` Velvet Noise Mixer with LFO prototype. Version 4.x starts moving toward a cleaner architecture with an `AudioEngine` class and a `Layer` class, while keeping the first implementation intentionally limited to one layer for easier debugging.

The previous `v2.9` version worked reliably but used a more direct global-function style. The new architecture is intended to make it easier to add multiple mixable noise layers later without duplicating large blocks of code.

## Current goal

Create a controllable, calm and organic noise source that can be tuned for comfort instead of technical loudness. The sound should avoid harsh static noise and instead feel slightly alive through density, filtering, stereo placement, reverb and slow LFO modulation.

## Features in v4.3

- One velvet noise layer
- Master volume
- Stereo reverb with wet/dry and decay controls
- Layer controls:
  - Density
  - Lowpass frequency
  - Q
  - Pan
  - Volume
  - LFO Depth
  - LFO Frequency
- LFO depth now uses a clearer `0...1` control range
- Minimum density changed to `0.05`
- Density changes update the existing noise buffer during playback
- Partial amplitude compensation added for low density values, so sparse impulses do not become too quiet
- Text colors improved for better readability on the dark UI
- Frequency slider uses logarithmic mapping
- EQ/lowshelf compensation values are displayed for debugging and tuning

## Important design notes

Density affects both texture and loudness. Physically, the RMS level of velvet noise roughly follows the square root of impulse density. If density becomes very low, the result naturally becomes quieter. In v4.3 this is only partially compensated, because full RMS compensation can make sparse impulses too peaky and intrusive.

LFO Depth is not a raw oscillator amplitude. It is a musical/perceptual modulation amount:

```js
// LFO depth controls modulation amount: 0 = no modulation, 1 = strong modulation.
```

The current version keeps the UI simple and avoids multiple compensation modes. The aim is not laboratory-correct loudness matching but a pleasant and safe-sounding practical control response.

## Known limitations

- Only one layer is currently implemented.
- No preset system yet.
- No fade-in/fade-out sleep timer yet.
- No limiter or final safety gain stage yet.
- No mobile-specific layout polish yet.
- Compensation display is still technical and may later be hidden under a details toggle.

## Suggested next steps

1. Add layer generation through a reusable UI builder function instead of copying HTML manually.
2. Add 2–4 layers with slightly different density, pan, frequency and LFO settings.
3. Add a global start/stop button with soft fade-in and fade-out.
4. Add a sleep timer, for example 15, 30, 45 and 60 minutes.
5. Add a final limiter or conservative output cap to prevent accidental loud peaks.
6. Add presets such as:
   - Soft Air
   - Warm Room
   - Gentle Rain-like Texture
   - Deep Sleep Noise

## Safety note

This is an experimental audio prototype. For child sleep use, playback level should be kept clearly quiet and comfortable. Avoid placing speakers close to the child, and test the level yourself before use.
