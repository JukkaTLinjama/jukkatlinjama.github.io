# Elsi Vacuum App v0.4

Experimental vacuum sound engine extracted from Vacuum Sample Lab v1.10.

This version focuses on stabilizing tonal motor sample looping before expanding the UI or scene system.

---

# Main architectural change

Previous experiments used a runtime dual-source overlap scheduler for the motor sample.

This caused instability:
- uncontrolled source spawning
- excessive active voices
- comb filtering
- phase instability
- difficult cleanup timing

The motor engine now uses:

```txt
preprocessed loop buffer
+ native Web Audio source.loop playback
```

instead of runtime overlap scheduling.

---

# Current engine structure

## Motor layer
- tonal vacuum motor sample
- asset:
  `assets/vacuum-loop.wav`
- playbackRate controlled continuously by load mapping
- one active motor source only

## Synthetic layers
- air noise
- velvet low
- velvet high
- harmonic noise

These are mixed dynamically based on load and scene mapping.

---

# Motor preprocess loop

The raw motor sample is preprocessed once after loading.

Process:
- wrap-crossfade between end and beginning
- creates smoother cyclic loop transition
- avoids runtime overlap scheduling complexity

API:
```js
window.vacuumSampleLabApi.reprocessMotorLoop();
```

Control:
```js
motorPreprocessMs
```

Typical useful range:
```txt
60–250 ms
```

---

# Public API

```js
window.vacuumSampleLabApi.start();

window.vacuumSampleLabApi.stop();

window.vacuumSampleLabApi.updateControl({
  loadSigned01,
  loadLfoDepth01,
  motorVolume01,
  airVolume01,
  velvetLow01,
  velvetHigh01,
  harmonicNoise01,
  masterVolume01,
  motorPreprocessMs
});

window.vacuumSampleLabApi.getState();
```

---

# Important engine assumptions

## Tonal motor sample is NOT assumed seamless

The engine assumes:

```txt
tonal motor loops are never perfectly seamless
```

Therefore:
- runtime must handle seam smoothing
- engine must not depend on perfect loop exports

---

# Debug expectations

Healthy runtime state:

```js
motorLoop: {
  activeVoices: 1,
  nativeLoopUsedForMotor: true,
  processedLoop: true
}
```

---

# Next planned direction

Possible next steps:
- floor profile variants
- movement inertia
- scene presets
- volume modes
- stereo decorrelation
- improved preprocess algorithms
- phase-aware loop matching
