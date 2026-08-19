# Evidence and Motion Analysis

Use this reference when a page contains distinctive, canvas-based, WebGL, shader, particle, cursor, or scroll-linked behavior.

## Evidence ladder

Move from the least invasive source to the most technical source:

1. Observe the page visually at rest.
2. Exercise every visible input state.
3. Inspect accessible DOM, computed geometry, CSS, and data attributes.
4. Inventory network-delivered images, fonts, video, CSS, and JavaScript.
5. Search public bundles for visible labels, component names, asset paths, shader declarations, constants, and dynamic-import identifiers.
6. Retrieve a dynamically loaded public chunk only when it is required to understand an observable behavior.
7. Reconstruct the behavior in the local project; do not paste opaque minified implementation code as the solution.

Stop if the evidence requires bypassing access controls.

## Capture sheet

For each signature effect, record:

| Field | Questions |
|---|---|
| Rendering surface | DOM, SVG, Canvas 2D, WebGL, video, or mixed? |
| Geometry | What is repeated? How is position, scale, and clipping calculated? |
| Content | Text, glyph atlas, images, particles, paths, or procedural texture? |
| Time | Idle speed, phase offsets, duration, easing, and frame dependence? |
| Input | Pointer position, velocity, press duration, scroll, visibility, or resize? |
| State | Idle, hovered, pressed, charged, released, leaving, reduced-motion? |
| Transition | Which variables interpolate and which change discretely? |
| Performance | Item count, batching, draw calls, DPR cap, visibility throttling? |
| Responsive | Same model with different scale, or a separate mobile behavior? |

## Runtime bundle search

Use focused searches rather than reading every minified file. Useful anchors include:

- Visible cursor labels such as `CLICK & HOLD`
- Component-like names such as `Spiral`, `Scene`, `Shader`, or `Canvas`
- WebGL strings such as `#version 300 es`, `uniform`, `attribute`, or `gl_Position`
- Canvas APIs such as `requestAnimationFrame`, `fillText`, and `drawImage`
- Interaction events such as `pointerdown`, `pointerup`, and `mousemove`
- Dynamic-import module identifiers and chunk paths
- Distinctive copy rendered inside the effect

Extract only the portion needed to infer observable behavior. Translate constants into a human-readable model before implementation.

## Choose the rendering approach

- Use **DOM/CSS** for semantic elements, small repeated sets, and layout-driven motion.
- Use **SVG** for resolution-independent paths and modest interactive vector counts.
- Use **Canvas 2D** for batched sprites, pixels, image processing, and moderate glyph counts.
- Use **WebGL** for thousands of independently transformed instances, shaders, complex distortion, or sustained high-frame-rate particle work.
- Use **video** when the reference is pre-rendered and has no meaningful per-pixel interaction.

Match the reference technique when technique shapes the look or performance. Otherwise choose the simplest mechanism that remains observably equivalent.

## Motion reconstruction

Write the effect as a state machine before coding:

```text
idle -> hover -> press -> charged -> release ripple -> idle
          \---------------- pointer leave ----------------/
```

For each transition, specify:

- Trigger
- Duration
- Easing or smoothing constant
- Animated variables
- Cancellation behavior
- Reduced-motion result

Test deterministic snapshots at known elapsed times when possible. For continuous effects, compare at least rest, input peak, and recovery.

## Performance correction

If the faithful implementation stalls:

1. Measure before simplifying.
2. Batch identical primitives.
3. Cache static layers or glyph atlases.
4. Cap device pixel ratio.
5. Pause outside the viewport or on hidden documents.
6. Reduce draw frequency only if motion remains equivalent.
7. Move high-count independent transforms to instancing or shaders.

Do not “optimize” by removing the defining response to input.
