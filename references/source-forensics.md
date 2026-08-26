# Source Forensics: From Reference URL to Implementable Model

Use this method when visual observation does not reveal how a signature interaction works. The goal is to explain observable behavior, not to transplant opaque minified source.

## Contents

- Reusable investigation ladder
- Practical collection pattern
- How the hero implementation was found
- Translating minified source into a specification
- Responsive geometry and content capture
- Evidence quality rules

## Reusable investigation ladder

1. Record screenshots and interaction states.
2. Inspect semantic DOM, computed geometry, CSS variables, data attributes, and accessible labels.
3. Inventory public HTML, CSS, JavaScript, fonts, images, video, and source maps.
4. Search for visible text and likely component names.
5. Trace dynamic imports to the chunk containing the effect.
6. Extract constants, geometry, state variables, event mappings, and performance controls.
7. Rewrite the findings as a renderer-independent specification.
8. Implement independently in the target project.

Stop if access would require bypassing authentication, paywalls, safety interstitials, or other controls.

## Practical collection pattern

For a publicly accessible page, save the response and referenced static assets into a temporary work area. Keep this evidence out of production assets and commits unless redistribution is authorized.

Use focused searches:

```bash
rg -n "CLICK & HOLD|Spiral|SpiralScene|Canvas|Shader" work/
rg -n "#version 300 es|uniform |gl_Position|requestAnimationFrame" work/
rg -n "pointerdown|pointerup|pointermove|visibilitychange" work/
```

When a loader refers to a numeric module, search for that identifier across chunks before guessing filenames:

```bash
rg -n "485059|452868|SpiralScene" work/ca-assets/*.js
```

## How the hero implementation was found

The initial HTML and route chunks exposed a component named `Spiral`. A loader in `111y8o_d2a781.js` dynamically imported module `485059` and selected `SpiralScene`, while listing module `452868` as the generated loadable module.

Another runtime chunk, `09mtc2cui9o70.js`, mapped module `485059` to two dynamically loaded files:

- `static/immutable/chunks/3h27dmotebbmr.js`
- `static/immutable/chunks/0-o87iokf3sck.js`

Retrieving those publicly referenced files revealed `SpiralScene`, including:

- The glyph phrase `THE CONTENT ARCHITECTURE.`
- A generated glyph atlas
- WebGL 2 instanced quad geometry
- Thirty ring configurations
- Vertex and fragment shader strings
- Hover, hold, charged, release, ripple, scroll, resize, visibility, and reduced-motion state

This evidence disproved the first noise-ring implementation.

## Translate minified source into a specification

Do not keep minified variable names. Create a table:

| Evidence | Meaning |
|---|---|
| `Array(30)` | Thirty concentric rings |
| Radius interpolation `.06 + 1.39 * t` | Rings span design radius `0.06` to `1.45` |
| Alternating speed sign | Adjacent rings counter-rotate |
| Glyph atlas with eight columns | Text and dot shapes are sampled as texture tiles |
| Instanced attributes for radius, theta, size, glyph, ring | Each slot is a GPU instance |
| `uMouseRadius = .35` | Cursor dissolution uses a circular falloff in design space |
| `RIPPLE_DURATION = 1.8` | Release wave lasts 1.8 seconds |
| `HOLD_GATHER_SCALE = .12` | Charged rings gather inward by up to 12% |
| `IntersectionObserver` and visibility events | Stop continuous rendering when hidden or offscreen |
| Scroll velocity input | Scroll energy changes per-ring angular offsets |

Then describe the state machine:

```text
mount -> entrance ripple -> idle rotation
idle -> pointer enter -> hover dissolve
hover -> pointer down -> holding/gathering
holding after ~0.9s -> charged/frozen/glitching
charged -> pointer up -> radial release wave -> idle
any active state -> pointer leave -> decay toward idle
```

## Capture responsive geometry

Do not infer only from class names. Record the visual box and its relationship to the viewport.

The reference hero used:

- A single-column layout with an `80vh` visual row on smaller screens.
- A twelve-column desktop layout with the visual occupying columns 7–12.
- A full-screen or minimum-screen-height hero.
- A dark scene background of approximately `#232323` beside an off-white copy panel.

The scene preserves circles by scaling against the larger viewport dimension. It is intentionally clipped rather than squeezed into an ellipse.

## Capture content without coupling it to layout

Extract content into typed or structured collections:

- Problems: identifier, description, time cost
- Benefits: identifier, heading, body
- Works: identifier, title, destination, image, alt text
- Reviews: quote, person, role, avatar
- Pricing: edition, description, audience, URL, included features
- FAQs: question, answer

This preserves exact baseline content while making later portfolio personalization mechanical.

## Evidence quality rules

- Prefer direct page evidence over memory.
- Prefer one authoritative runtime clue over multiple speculative guesses.
- Record which claim comes from visual observation and which comes from source analysis.
- Treat random generation as a parameter distribution, not as one frozen screenshot.
- Do not publish downloaded proprietary assets merely because the browser could retrieve them.
- Keep a list of unresolved behaviors instead of filling gaps with invented algorithms.
