# Verification, Fidelity Ledger, and Known Gaps

Verification must prove both the reusable workflow and this particular implementation.

## Contents

- Reusable acceptance sequence
- Acceptance matrix and fidelity labels
- What was verified locally
- What remains approximate or unverified
- Performance lesson
- Final delivery report

## Reusable acceptance sequence

### 1. Static correctness

- Run the project build with the required runtime version.
- Run server-render or component tests.
- Check that all required sections, headings, controls, links, and labels render.
- Check the working tree for unintended files or unrelated changes.

### 2. Matched viewport comparison

For each target viewport:

1. Capture the reference at a known scroll position.
2. Capture the local page at the same viewport and semantic section.
3. Compare section boundaries, grid lines, text wrapping, crops, sticky positions, and fixed overlays.
4. Use side-by-side images or an opacity overlay.
5. Record differences by severity rather than relying on general visual impression.

At minimum test:

- One wide desktop viewport
- One laptop viewport
- One narrow mobile viewport
- The exact breakpoint neighborhood

### 3. Interaction-state comparison

Capture or observe:

- Page entrance at start, midpoint, and settled state
- Hero at rest
- Hero with pointer hover
- Hero while holding
- Fully charged hero
- Release ripple near the center and outer rings
- Hero after pointer leave
- Navigation label hover
- Showcase at rest and hover/focus
- IDE tab switching
- Previous and next review
- Closed and open FAQ
- Footer form focus
- Scroll and sticky transitions
- Reduced-motion mode

### 4. Runtime health

- Inspect console errors and warnings.
- Check failed images, fonts, and script requests.
- Observe layout shifts.
- Measure whether the signature animation blocks browser interaction.
- Pause or leave the animated region and verify that unnecessary work stops.

## Acceptance matrix

| Area | Desktop | Mobile | Required evidence |
|---|---:|---:|---|
| Exact section order and content | ☐ | ☐ | DOM/content inventory |
| Grid, width, and alignment | ☐ | ☐ | Matched screenshots |
| Typography and line breaks | ☐ | ☐ | Overlay comparison |
| Color and surface system | ☐ | ☐ | Token check and screenshots |
| Fixed navigation | ☐ | ☐ | Rest and scrolled state |
| Hero entrance | ☐ | ☐ | Start/mid/end frames |
| Hero idle rotation | ☐ | ☐ | Motion observation |
| Hero hover dissolve | ☐ | ☐ | Before/after frames |
| Hero hold and charge | ☐ | ☐ | Timed interaction sequence |
| Hero release ripple | ☐ | ☐ | Peak and recovery frames |
| Hero scroll response | ☐ | ☐ | Scroll interaction |
| ASCII image accuracy | ☐ | ☐ | Multiple source images |
| Showcase hover and focus reveal | ☐ | ☐ | Pointer and keyboard checks |
| IDE tabs | ☐ | ☐ | Both content states |
| Review controls | ☐ | ☐ | Wraparound behavior |
| FAQ accordion | ☐ | ☐ | Open, close, and ARIA state |
| Sticky layout behavior | ☐ | ☐ | Multiple scroll positions |
| Footer background and form | ☐ | ☐ | Visual and focus check |
| Reduced motion | ☐ | ☐ | Media-query test |
| Console and asset errors | ☐ | ☐ | Clean logs |
| Animation responsiveness | ☐ | ☐ | Browser remains interactive |

## Severity and fidelity labels

Use one label per feature:

- **Exact** — same observable content and mechanism within measured tolerance.
- **Reconstructed** — independently implemented from strong evidence with equivalent behavior.
- **Approximate** — visually related but with a material mechanism or state difference.
- **Mock** — intentionally temporary asset, content, or destination.
- **Unverified** — implemented but not exercised against the reference.
- **Missing** — not implemented.

Do not collapse these labels into a single claim that the whole site is “done.”

## What was actually verified in the local reconstruction

- The project compiled successfully with the compatible bundled Node runtime.
- Server-render tests confirmed the page structure and accessible controls.
- The local page loaded at `localhost:3001`.
- A narrow `640 × 731` browser viewport showed the character rings after scrolling to the hero visual region.
- Pointer movement displayed the `CLICK & HOLD` cursor label and changed nearby glyph distribution.
- Browser console checks returned no errors or warnings during that inspection.
- The Canvas performance correction removed the browser stall observed in the first dense implementation.

## What remained unverified or approximate

### Hero scene

- The reference is WebGL 2 with instanced, tangent-rotated glyph quads; the local version is a throttled Canvas 2D fallback.
- Local glyph density is capped.
- Local glyphs are not all tangent-rotated.
- Scroll-velocity rotation is missing.
- Sparse hold tremor, glitch cadence, and ring-by-ring spring-back are simplified.
- A full automated hold-to-charge-to-release recording was not captured.
- Desktop scene fidelity was not established with systematic pixel overlays.

Classify the current hero as **Approximate/Reconstructed**, not Exact.

### Showcase ASCII

- The local method is deterministic and visually checked, but it is an independent reconstruction.
- Multiple images should be compared against the reference at matched sizes.
- Mobile hover has no direct equivalent; touch behavior needs an explicit design decision.

Classify it as **Reconstructed** until broader comparison passes.

### Entrance curtain

- The local curtain reproduces the visual idea with seeded cell disappearance.
- Timing and cell behavior were not measured frame by frame against the reference.

Classify it as **Approximate**.

### Reduced motion and lifecycle

- CSS disables transitions and hides the curtain.
- The Canvas character loop itself does not fully implement the reference’s media-query, visibility, and intersection lifecycle controls.

Classify reduced-motion behavior for the Canvas scene as **Incomplete**.

### Page-level cleanup

- The current JSX contains a duplicated `SHOWCASE` navigation item and should be corrected before treating the page as a clean baseline.
- Full desktop and breakpoint-neighborhood visual regression coverage is still missing.
- External reference content and prices should be recaptured if the source has changed since the original snapshot.

## Performance lesson from the browser timeout

The first Canvas reconstruction drew thousands of rotated glyphs with per-item `save`, `translate`, `rotate`, and `restore` calls every animation frame. The browser became slow enough that automated reload and screenshot operations timed out.

The mitigation—lower frame rate, cap slots, batch dots, remove per-dot state changes—restored responsiveness but changed the look. This is an important decision rule:

```text
If performance correction removes visible fidelity, do not keep optimizing the fallback.
Move the effect to the rendering model used by the reference or explicitly accept the approximation.
```

## Final delivery report

Use this format:

```text
Replication mode:
Reference capture date:
Tested viewports:
Exact features:
Reconstructed features:
Approximate features:
Mocks:
Unverified states:
Missing states:
Performance notes:
Licensing-sensitive assets:
Console/runtime status:
```
