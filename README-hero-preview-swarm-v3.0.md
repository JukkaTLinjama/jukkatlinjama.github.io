# Hero Preview Swarm Prototype v3.0

Standalone Three.js hero-preview experiment for the portfolio / audio-coding landing page.

This version is based on the earlier 3D Wavelet Simulation idea, but reduced into a lightweight visual hero prototype:
- no full demo UI
- landscape preview frame
- target field + follower dynamics
- wavelet-driven motion
- touch/click interaction
- parameters for speed and follower dynamics

## Current file

`hero-preview-swarm-v3.0-z-suction.html`

## Purpose

The goal is to create a compact animated hero illustration for the landing page, not a full simulation UI.

The visual idea is:

- red target field in the background
- blue/white follower spheres in the foreground
- follower spheres react to the moving targets through spring-mass-damper dynamics
- the whole field feels like a small responsive 3D kinetic sculpture
- touch/click interaction creates a depth-based z-axis suction effect

## Main behavior

### Target field

The red targets form a wide landscape-oriented field.

Their motion is driven by wavelet-generated X/Y/Z modulation. The target field is intentionally calmer than the foreground followers.

Added in later versions:
- reduced target flicker
- brighter red target visibility
- slow global breathing modulation
- smoother slowdown behavior

### Followers

Followers are larger foreground objects.

They do not move independently. They follow selected target positions with spring-mass-damper dynamics.

The `Dynamics` slider adjusts the follower response:
- lower value = softer, slower, more delayed
- higher value = stiffer, faster, more reactive

### Speed

The `Speed` slider controls wavelet playback speed.

This affects the target motion, which indirectly affects follower motion.

### Touch / click interaction

The preview area responds to pointer interaction.

Current v3.0 behavior:
- XY swirl was removed
- touch/click creates a z-axis suction effect
- center pulls follower spheres toward the camera
- outer ring returns flow away from the camera
- XY movement is kept minimal so the interaction reads as depth flow, not planar rotation
- continuous touch creates repeated impulses at a slow interval

Speed boost is intentionally very small. The visible reaction mainly comes from direct follower velocity impulses.

## Version history summary

### v2.0

Landing-page hero preview with canvas animation and subtle replay control.

### v2.1

Fixed `frameScale` scope bug in the swarm test.

### v2.2

Made target field more visible and added a Restart button.

### v2.3

Removed target connecting lines. Restored larger foreground followers.

### v2.4

Replaced FPS slider with `Dynamics` slider. Target field changed to red.

### v2.5

Moved camera closer, increased depth impression, and added subtle color/opacity modulation.

### v2.6

Calmed target motion and target brightness pulsing. Added smooth slowdown instead of abrupt stopping.

### v2.7

Added tap/click impulse: nearby targets move away from touch and follower speed reacts through target motion.

### v2.8

Added direct follower vortex impulse and global breathing modulation.

### v2.9

Changed touch response toward z-vortex behavior. Added continuous touch impulses.

### v3.0

Removed visible XY swirl. Interaction is now z-axis suction:
- center pulls toward camera
- outer ring returns away from camera
- XY correction is very small

## Important implementation notes

This is currently a standalone HTML file.

It includes:
- Three.js from CDN
- CSS
- simulation code
- wavelet generation
- follower dynamics
- interaction logic

No external `utilities.js` is required in this prototype.

## Known questions / next development ideas

### 1. Global target-field metamorphosis

Instead of stopping or becoming almost static, the target field could continue a slow morphing behavior.

Suggested concept:

- keep an `activation` value that never reaches zero
- idle activation could decay to about `0.12–0.20`
- touch/click raises activation temporarily
- target field continues slow wavelet-based shape changes even without interaction

Possible shape layers:
- wide flat grid
- slightly curved surface
- low-amplitude cloud
- ribbon-like horizontal band
- shallow bowl / lens shape
- breathing expansion/contraction

This should be added as a slow global layer on top of the existing local wavelet motion, not as random per-target noise.

### 2. Better z-depth visibility

The z-axis suction effect works best if depth is visually readable.

Possible additions:
- follower opacity based on z position
- follower size based on z position
- mild brightness increase when closer to camera
- very subtle background fog or depth fade

### 3. Landing page integration

After the standalone behavior is stable, integrate this as the hero preview behind the landing-page header text.

Recommended approach:
- keep the standalone prototype as reference
- extract the swarm preview into a small `HeroSwarmPreview` object or function group
- mount it into the existing hero container
- preserve the v2.0 landing-page intro/replay opacity choreography

## Prompt for next session

Continue from `hero-preview-swarm-v3.0-z-suction.html`.

We are building a lightweight Three.js hero-preview animation for the portfolio / audio-coding landing page.

Current state:
- standalone HTML prototype
- red target field in the background
- blue/white foreground followers
- wavelet-driven target motion
- spring-mass-damper follower dynamics
- Speed slider controls target wavelet playback
- Dynamics slider controls follower stiffness/damping
- Restart button resets the prototype
- touch/click interaction creates z-axis suction:
  - center pulls followers toward camera
  - outer ring returns flow away from camera
  - XY swirl has been removed
- continuous touch pulses the impulse at intervals
- target field has slow breathing and reduced local flicker

Next goal:
Design and implement a slow global target-field metamorphosis on top of the existing wavelet motion.

Important constraints:
- do not add complex UI yet
- keep the file standalone
- keep the landscape form factor
- keep target field calmer than follower field
- do not reintroduce strong XY swirl
- keep touch response primarily z-directional
- use small, reversible changes
- comments in code should be in English

Desired behavior:
The animation should not fully die after the initial active phase. Instead, an `activation` or `idleMotion` value should decay to a low non-zero level so the target field keeps slowly changing shape. The field could morph between a wide grid, shallow curved surface, soft cloud, and/or ribbon-like form. This should be a global shape layer, not random per-target noise.
