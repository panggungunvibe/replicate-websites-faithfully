# Content Architecture Replication Case Study

This case turns the reusable replication method into a concrete implementation sequence for `contentarchitecture.dev`.

## Contents

- The two-layer model
- What went wrong first
- Corrected end-to-end sequence
- Reusable conclusions

## The two-layer model

| Reusable method | Application in this project |
|---|---|
| Capture all observable states | Inspect the hero, fixed navigation, long-form sections, showcase cards, reviews, pricing, FAQ, and footer at desktop and mobile widths |
| Identify signature elements | Prioritize the interactive character rings, showcase ASCII conversion, and ASCII entrance curtain |
| Inspect public runtime evidence | Search downloaded HTML and chunks for `Spiral`, `SpiralScene`, `CLICK & HOLD`, shaders, and dynamic imports |
| Reconstruct the mechanism | Derive ring geometry, glyph distribution, hover dissolution, hold charge, release ripple, and performance controls |
| Build low-risk structure from data | Store benefits, works, reviews, FAQs, repository files, and pricing content in arrays and map them into repeated components |
| Validate actual behavior | Build, server-render, inspect the local browser, capture rest and hover states, and check console errors |
| Report gaps | Distinguish the original WebGL scene from the local Canvas 2D fallback and list lost behavior |

## What went wrong first

The first pass treated “replicate” as “make a similar portfolio.” It correctly reproduced broad visual cues but invented an organic concentric-noise field for the hero. The reference did not use a noise-based wood texture. It used text and dot glyphs placed on 30 independently animated circles.

The failure was procedural:

1. Visual inference happened before source evidence was exhausted.
2. Low-risk page scaffolding was treated as progress on the high-risk signature effect.
3. A still screenshot was used as the mental target for an interaction-driven scene.
4. “Similar visual category” was mistaken for “same mechanism.”

The correction was to reopen the reference implementation, inspect the publicly delivered JavaScript, identify the real `SpiralScene`, and rebuild from its geometry and state model.

## Corrected end-to-end sequence

### Phase 1: Establish scope

Interpret the request as:

- Preserve the original content initially.
- Preserve section order and destinations.
- Use mock images only when necessary.
- Keep English inside signature graphics when it is part of the visual texture.
- Postpone personal portfolio replacement until the baseline is accepted.
- Run locally first; do not deploy unless asked.

### Phase 2: Capture content and structure

Collect:

- Navigation labels and anchors
- Hero kicker, title, lead, CTA, and technology labels
- Problem rows and estimated time values
- Benefits list
- Repository tree and README content
- Eleven showcase entries, links, images, and labels
- Three reviews and avatars
- Two pricing editions and included features
- Full FAQ list
- Footer copy, form, navigation, and credit links

Implement repeated content as data collections rather than duplicated JSX. This makes later personalization a data replacement task instead of a layout rewrite.

### Phase 3: Reconstruct the page architecture

Build the page in this order:

1. Global tokens, fonts, and background system
2. Fixed dithered navigation and rolling labels
3. Hero grid and a placeholder for the signature scene
4. Problem terminal section
5. Benefits list
6. Full-height repository/IDE panel
7. Showcase grid with real or mock screenshots
8. Reviews carousel
9. Pricing cards
10. Sticky FAQ
11. Full-height footer with background scene

Do not treat the placeholder hero as completed work. Keep the signature-effect task open until source analysis and interaction testing pass.

### Phase 4: Investigate and implement the signature effects

Use the exact procedures in [source-forensics.md](source-forensics.md) and [implementation-blueprint.md](implementation-blueprint.md).

The resulting local components were:

- `DitherFrame` — reusable dotted/noisy frame shell
- `RollText` — two-line vertical hover transition
- `AsciiVisual` — interactive character-ring scene used by hero and footer
- `AsciiImageReveal` — image sampling and character-density conversion for showcase cards
- `AsciiCurtain` — full-screen entrance overlay

### Phase 5: Add stateful interactions

Keep simple UI state local:

- `faq` stores the open FAQ index; clicking the open item closes it.
- `slide` stores the current review and wraps modulo three.
- `terminal` switches the IDE panel between repository tree and README.
- Pointer refs keep high-frequency hero input out of React render cycles.

Use CSS for deterministic presentation transitions and Canvas/WebGL for high-frequency pixel work.

### Phase 6: Build responsive states

At widths below `900px`:

- Collapse the navigation to the mark and status link.
- Stack hero copy above a `75vh` visual region.
- Reverse the problem section so explanatory text precedes the terminal visually.
- Collapse benefits, showcase, pricing, FAQ, and footer grids to one column.
- Remove sticky FAQ behavior.
- Hide the IDE file tree.
- Reduce section padding from desktop values around `80px/160px` to `16px/72px`.
- Hide the central scroll cue.

Treat this as a designed mobile state, not automatic flex wrapping.

### Phase 7: Verify and correct performance

The first Canvas character-ring reconstruction attempted thousands of per-glyph save/rotate/draw operations every frame and stalled the local browser. The correction was:

- Cap DPR at 2.
- Limit slot counts.
- Render at roughly 24 FPS.
- Batch all dots into one path and one fill.
- Avoid per-dot `save`/`restore` calls.
- Keep pointer data in refs.

This restored usability but reduced fidelity. The correct high-fidelity route for production is WebGL instancing with a glyph atlas, matching the original mechanism.

## Reusable conclusions

Apply these beyond this site:

- Separate content capture from visual interpretation.
- Convert repeated content to data early.
- Preserve a named list of unresolved signature elements.
- Search source evidence when a visual cannot be explained confidently.
- Write interactive effects as state machines before choosing a renderer.
- Match the renderer to instance count and transformation complexity.
- Validate at the same viewport and interaction state as the reference.
- Keep a fidelity ledger that distinguishes exact, reconstructed, approximate, mocked, and unverified work.
