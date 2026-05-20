# Hero Preview Swarm Prototype v3.12

Standalone Three.js hero-preview experiment for the portfolio / audio-coding landing page.

Current file:

`hero-preview-swarm-v3.12-intro-envelope-calm-start.html`

## Purpose

This prototype is a lightweight animated hero section, not a full simulation UI.

The concept is a responsive browser-based kinetic sculpture:
- red target field in the background
- blue/white follower spheres in the foreground
- target motion driven by wavelet-like modulation
- followers driven by spring-mass-damper dynamics
- touch interaction behaves like a 3D attractor
- title text is part of the 3D scene so moving spheres can visually pass over it

## Visual text layout

The main title is rendered inside the Three.js scene:

- `Experiments with`
- `Interactive Content`

The subtitle is also part of the scene and behaves as a soft floating follower object:

- `Realtime audiovisual and physics-driven browser demos`

Metadata remains in the DOM below the preview:

- Created by
- Jukka Linjama
- v2.0 · 2026-05-20

## Main behavior

### Intro envelope

v3.12 adds a calm-start envelope.

Instead of starting at full activity immediately:
1. the scene begins quietly
2. activity rises gradually
3. the animation becomes more energetic for a short period
4. it slowly settles unless the user interacts

This avoids the earlier abrupt startup caused by followers beginning far from their targets.

### Calm follower initialization

Followers are now initialized close to their own target positions.

This avoids large initial spring errors:

```js
follower.position ≈ followedTarget.position
follower.velocity = 0
```

A small offset is still allowed so the system does not look perfectly locked.

### Activity system

The old `Speed` idea has been replaced by an `Activity` concept.

Activity controls several layers together:
- target waveform speed
- target waveform amplitude
- follower effective period
- follower effective damping
- subtle breathing modulation

The UI shows:
- `Set`: user-selected maximum/manual value
- `Now`: effective current value after intro, settling, breathing and tap boost
- `Status`: Active / Waking / Settling / Idle

### Tap boost

Tap/click no longer instantly resets the animation to full activity.

Current behavior:
- each tap adds about +20% activity
- tap boost can temporarily exceed the slider setting by up to +50%
- boost decays back with about a 10 s time constant
- this makes touch feel additive rather than like a hard reset

### Touch attractor

The pointer is projected onto a perspective plane inside the visible camera frustum.

Instead of a z-only impulse, followers receive a 3D impulse toward the projected touch point:
- center touch creates depth movement
- side touch also produces XY movement toward the finger
- randomized per-axis impulse multipliers keep the response organic

### Target-field morph

A slow global morph remains active even when the main motion settles.

This layer shifts and scales the target field slowly:
- global translation
- global scale
- low-amplitude target breathing
- target-specific phase offsets

The goal is to avoid a dead/static end state.

### Follower dynamics

Followers use spring-mass-damper behavior.

UI parameters:
- `Period`: base follower response period
- `Damping`: base damping percent

The UI also shows live `Now` values for debugging:
- effective period after activity/idle modulation
- effective damping after activity/idle modulation

Follower masses and dynamic parameters are randomized so the spheres do not move in perfect sync.

## Important implementation notes

This is still a standalone HTML file.

It includes:
- Three.js via CDN
- CSS layout
- scene creation
- text canvas textures
- target/follower creation
- wavelet generation
- activity envelope
- touch attractor logic
- UI controls

No external JavaScript modules are required yet.

## Current UI controls

- Activity
- Period
- Damping
- Restart

The activity readout includes:
- set activity
- current status
- tap boost
- now activity

Period and damping readouts include:
- set value
- now/effective value

## Version summary

### v3.0

Z-axis suction interaction:
- center pulls toward camera
- outer ring returns away from camera
- XY swirl removed

### v3.1

Added slow global target-field morph:
- translation
- scale
- persistent idle movement

### v3.2

Extended slowdown:
- 30 s settling
- more random follower dynamics
- random touch pulse components

### v3.3

Stronger random dynamics:
- follower mass range expanded to 0.3–4.0
- stronger per-axis touch randomization

### v3.4

Replaced generic dynamics slider with:
- Period
- Damping %

Mass now affects perceived response more clearly.

### v3.5

Perspective touch attractor:
- pointer projected onto visible plane
- followers pulled toward 3D touch point

### v3.6

Activity wake model:
- Speed renamed to Activity
- interaction wakes the animation
- idle movement remains

### v3.7 / v3.7.1

Activity readout and breathing:
- Set / Now display
- slow activity breathing
- fixed missing `clamp()` helper

### v3.8

Target breathing and ramped wake:
- target-specific breathing motion
- status indicator
- touch wake ramps instead of jumping

### v3.9

Tap boost and scene title:
- tap adds activity boost
- boost can exceed set value temporarily
- scene title added
- tap hint added

### v3.10

Floating subtitle:
- subtitle becomes a soft follower object
- metadata moved below canvas

### v3.11

Large scene text and live dynamics:
- larger title text
- title pushed forward in depth
- live Period and Damping NOW values

### v3.12

Intro envelope and calm start:
- follower spheres start near their target locations
- calm start before activity rises
- title split into two lines
- text moved closer to top/bottom canvas edges
- live debug values checked for Activity / Period / Damping

## Known next ideas

### 1. Tune title depth and occlusion

The current title is in the 3D scene. Spheres can pass in front of it, but the exact depth balance may need tuning.

Possible parameters:
- title z-position
- follower z-range
- text material opacity
- render order

### 2. More cinematic intro

The current intro envelope is functional.

Possible next refinement:
- slower first 1–2 seconds
- stronger acceleration peak
- title fade-in linked to activity
- target field fade-in before follower movement

### 3. Better mobile layout

The preview is landscape-oriented. It should be tested on:
- desktop Chrome
- iOS Safari
- narrow mobile layout

Potential issue:
- controls may consume too much vertical space on small screens

### 4. Extract reusable component

Once the behavior is stable, extract the preview into a small component/function group for the actual landing page.

Suggested future structure:
- `HeroSwarmPreview`
- `initHeroSwarm(container)`
- `destroyHeroSwarm()`
- optional config object

## Prompt for next session

Continue from `hero-preview-swarm-v3.12-intro-envelope-calm-start.html`.

We are building a lightweight Three.js hero-preview animation for a portfolio / audio-coding landing page.

Current state:
- standalone HTML prototype
- red target field
- blue/white follower spheres
- target wavelet motion
- spring-mass-damper followers
- perspective-based touch attractor
- Activity / Period / Damping controls
- Activity has Set / Now / Status / Tap boost readout
- Period and Damping have Set / Now readouts
- title text is inside the Three.js scene:
  - `Experiments with`
  - `Interactive Content`
- subtitle is a soft floating follower:
  - `Realtime audiovisual and physics-driven browser demos`
- metadata is below the canvas:
  - Created by / Jukka Linjama / v2.0 · 2026-05-20
- followers now start close to their target positions for a calm startup
- intro envelope ramps activity up, then lets it settle
- taps add activity boost instead of resetting instantly

Important constraints:
- keep this standalone for now
- make small reversible changes
- keep comments in English
- do not add complex architecture yet
- preserve live debug readouts for Activity / Period / Damping
- keep the hero cinematic and not too UI-heavy

Likely next steps:
1. tune intro envelope timing and strength
2. tune title/subtitle depth and occlusion
3. test mobile layout
4. later extract this into the actual landing-page index.html
