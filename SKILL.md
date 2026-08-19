---
name: replicate-websites-faithfully
description: Reproduce an existing website with evidence-backed fidelity across content, architecture, responsive layout, interactions, and motion. Use when the user asks to 复刻网站, 完整复刻, 像素级还原, clone a site, match a reference URL, preserve all interactions or animations, or build a faithful local baseline before replacing it with their own portfolio content.
---

# Replicate Websites Faithfully

Treat “replicate” as a fidelity requirement, not a style reference. Reconstruct the observable system before personalizing it.

## Establish the replication contract

Infer the highest reasonable fidelity from the request:

- **Faithful replication**: match content, section order, proportions, responsive behavior, interactions, and signature motion.
- **Adaptation**: preserve the design language while replacing structure or behavior.
- **Personalization**: start from a faithful baseline, then replace content and identity.

When the user says “直接复刻”, “完整复刻”, or equivalent, default to faithful replication. Do not silently downgrade it to an adaptation.

Keep work within accessible, authorized sources. Do not bypass authentication, paywalls, safety interstitials, or technical access controls. Record third-party fonts, images, video, and code that require replacement or licensing before public release.

## Execute in this order

### 1. Capture evidence before coding

Inspect the live page at representative desktop and mobile widths. Capture:

- Section order, copy, links, images, and forms
- Grid, spacing, type scale, colors, borders, and layering
- Sticky, fixed, overflow, and scroll behavior
- Hover, focus, click, drag, hold, release, and scroll states
- Entrance, transition, and idle animation timing
- Canvas, WebGL, SVG, video, shader, or particle-driven regions
- Responsive changes rather than simple scaled screenshots

Save or inventory publicly delivered HTML, CSS, scripts, fonts, and assets when useful. Read [references/evidence-and-motion.md](references/evidence-and-motion.md) before analyzing a signature effect or opaque runtime bundle.

### 2. Build a replication map

Create a short internal map with five layers:

1. **Content** — exact copy, labels, media, and destinations
2. **Structure** — sections, components, hierarchy, and reusable patterns
3. **Visual system** — tokens, grids, typography, breakpoints, and surfaces
4. **Behavior** — navigation, accordions, carousels, forms, cursor states
5. **Motion** — state machine, triggers, timing, easing, rendering technique

Identify the three highest-risk signature elements. Prove their implementation approach early. A generic page shell is not evidence that the difficult parts are solved.

### 3. Scaffold the native architecture

Use the project’s existing framework and conventions unless the user requests a migration. Establish:

- Global tokens and fonts
- Page shell, navigation, and section containers
- Reusable content models
- Asset and mock-data boundaries
- Responsive breakpoints
- Reduced-motion behavior

Preserve unrelated user changes. Keep copied or temporary source evidence outside production assets.

### 4. Reconstruct signature behavior from evidence

Do not replace a distinctive effect with a visually adjacent generic algorithm.

For each signature effect:

1. Observe the rest, hover, active, held, released, scrolling, and resized states.
2. Identify the rendering model: DOM/CSS, SVG, Canvas 2D, WebGL, video, or a combination.
3. Inspect public runtime modules, asset names, shader strings, data attributes, and event labels when visual observation is insufficient.
4. Derive the state variables, geometry, timing, and input mapping.
5. Implement a minimal proof at the target viewport.
6. Compare it with the reference before integrating it into the whole page.

Prefer matching the underlying mechanism when it materially determines the result. For thousands of independently moving glyphs or particles, use an appropriate GPU or batched rendering approach rather than an expensive per-item DOM or Canvas loop.

### 5. Implement from high risk to low risk

Use this sequence:

1. Global visual system and page geometry
2. Signature hero or transition proof
3. Static content sections and repeated components
4. Secondary interactions and motion
5. Responsive variants
6. Accessibility and reduced motion
7. Performance refinement
8. Personalization only after the faithful baseline is accepted

Use mock assets only when the real asset is unavailable or should not be reused. Label mocks in code and centralize them so the user can replace them later.

### 6. Validate in the browser

Reload the local app after relevant changes. Verify actual pixels and input behavior, not only compilation.

Read [references/acceptance-matrix.md](references/acceptance-matrix.md) and cover every applicable row. At minimum, check:

- Desktop and mobile screenshots at the same scroll positions as the reference
- Navigation, links, accordions, and forms
- Hover, hold, release, drag, and scroll-driven states
- Responsive reflow and clipping
- Console errors and failed assets
- Animation smoothness and CPU/GPU load
- Keyboard focus, labels, contrast, and reduced motion

Use overlays, side-by-side screenshots, or image diffs for geometry. Use frame-by-frame observation for motion. Never call a signature effect complete from one still screenshot.

### 7. Report fidelity honestly

Finish with:

- What matches
- What remains approximate
- Which assets or links are mocked
- Which viewports and interactions were tested
- Any licensing-sensitive assets to replace before publication

Do not claim “fully replicated” while a defining interaction is missing or implemented with a materially different mechanism. Treat user feedback as a failed acceptance criterion, investigate the cause, and update the replication map before patching.

## Guardrails

- Do not start from memory when the reference is available.
- Do not conflate visual similarity with behavioral equivalence.
- Do not postpone every difficult animation until the end.
- Do not scrape protected or private content.
- Do not publish the reference site’s proprietary assets without authorization.
- Do not replace exact source content with invented copy unless the user asks for adaptation.
- Do not optimize away an interaction before measuring its fidelity and performance.
