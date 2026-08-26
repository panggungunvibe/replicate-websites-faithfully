# Implementation Blueprint

This document contains the concrete recipe for the Content Architecture replication and the reusable decisions behind it.

## Contents

- Project architecture, visual tokens, and section structure
- Original WebGL character-ring mechanism
- Local Canvas 2D fallback and fidelity tradeoffs
- Showcase image-to-ASCII conversion
- ASCII entrance curtain
- Micro-interactions
- Responsive blueprint
- Accessibility and content replacement

## 1. Project architecture

The local reconstruction used a React client page with:

- Static data arrays for repeated content
- Small presentational components for recurring visual patterns
- React state only for low-frequency UI state
- Refs and Canvas loops for high-frequency pointer animation
- CSS variables and section classes for the visual system

Core components:

| Component | Responsibility |
|---|---|
| `DitherFrame` | Wrap content in an eight-pixel patterned frame with a dark inset surface |
| `RollText` | Duplicate a label and vertically translate both copies on hover |
| `AsciiVisual` | Draw the interactive character rings for hero and footer |
| `AsciiImageReveal` | Convert each showcase image into an ASCII preview and reveal the image on hover/focus |
| `AsciiCurtain` | Render the page-load ASCII disappearance curtain |
| `Home` | Own FAQ, review, and IDE-tab state and compose all sections |

For a larger production project, separate these components and content collections into modules. The single-page arrangement was a speed-oriented reconstruction choice, not a required architecture.

## 2. Visual tokens

Use a small explicit palette:

```text
deep black       #000000
charcoal         #232323
off white        #f1eee7
white            #ffffff
dark grey        #5b5a56
ghost grey       #dedede
mid grey         #cbcbcb
orange accent    #ff9100
dark separators  rgba(255,255,255,.10)
```

Use a proportional sans font for headlines and body copy and a monospaced font for labels, technical metadata, terminal rows, ASCII glyphs, and controls.

Recurring typography rules:

- Headlines use tight line-height around `.92–1.05` and negative tracking around `-.045em` to `-.055em`.
- Monospaced labels use approximately `9–12px`, uppercase.
- Body copy uses restrained widths and line-height around `1.45–1.7`.
- Large type uses `clamp()` rather than a fixed desktop size.

## 3. Page and section structure

Compose the page in this order:

1. Fixed navigation
2. Hero
3. Common-problems terminal
4. Benefits
5. Repository/IDE panel
6. Showcase
7. Reviews
8. Pricing
9. FAQ
10. Footer

### Fixed navigation

- Position at `top: 16px`, centered on desktop.
- Disable pointer events on the outer fixed container and re-enable them on the navigation.
- Use an eight-pixel dither frame built with a repeating conic gradient.
- Use a 54-pixel inner navigation height.
- Keep the logo mark, section anchors, and a light status/blog block.
- Add a small orange status dot with an expanding transparent box-shadow loop.

### Hero

- Use a desktop grid of `5fr 1fr 6fr` as a practical representation of a twelve-column split.
- Place copy in the first region and the character scene in the final six columns.
- Use `min-height: 100svh`.
- Apply desktop copy padding near `160px 0 48px 80px`.
- Place technical labels at the bottom of the copy column.
- Keep the scene clipped inside a charcoal surface.
- Add a central scroll cue with a descending white block.

### Problem section

- Reuse the `5fr 1fr 6fr` relationship.
- Place a black terminal card on the left and explanatory copy on the right.
- Store rows as data: ID, problem, estimated hours.
- Use thin separators and orange only for the total.

### Benefits

- Use a dark full-width section with a `4fr 8fr` heading grid.
- Offset the list by one third on desktop.
- Render each benefit as number plus content with consistent separators.

### Repository panel

- Use an off-white full-height section containing a framed black IDE surface.
- Build a fixed header, optional file sidebar, scrollable content pane, and status footer.
- Use React state to toggle tree and README content.
- Hide the sidebar on mobile.

### Showcase

- Use a two-column card grid on desktop and one column on mobile.
- Keep posters at `16:9`.
- Wrap each card in a real external link and include index, provenance label, title, and outbound arrow.
- Use `AsciiImageReveal` for the poster.

### Reviews

- Center a track around 65% of desktop width.
- Use React state and modulo arithmetic for previous/next navigation.
- Key the quote by slide index so a short fade-and-rise animation restarts.

### Pricing

- Use two cards on desktop and one column on mobile.
- Keep pricing, edition metadata, included features, and outbound CTA structurally separate.
- Reuse the orange availability indicator.

### FAQ

- Use a `4fr 8fr` grid.
- Keep the left heading and CTA sticky within the viewport on desktop.
- Store the open index in state.
- Animate the answer with `grid-template-rows: 0fr` to `1fr`; keep the paragraph overflow hidden.

### Footer

- Use at least one viewport of height.
- Reuse a compact, low-opacity character-ring scene as an absolute background.
- Place the primary message, email form, and navigation in a `6fr 4fr 2fr` desktop grid.
- Put legal and credit metadata on a separated bottom row.

## 4. Original hero character-ring mechanism

The reference uses WebGL 2 and instanced quads. This is the fidelity target.

### Glyph atlas

Create an offscreen atlas containing `THE CONTENT ARCHITECTURE.`:

- Atlas width: `512px`
- Tile size: `64px`
- Columns: `8`
- Font size: approximately `57.6px`
- Render letters in white, centered in tiles.
- Render the period as a filled circle with radius approximately `5.76px`.

Upload the atlas as a texture. Each character instance selects a tile by glyph index.

### Ring generation

Generate thirty rings. For ring index `i`:

```text
t = i / 29
radius = 0.06 + 1.39 * t
speed = alternatingSign(i) * (0.006 + (1 - t) * 0.029)
letterSizePx = 14 + 16 * t
charsCount = max(8, floor(2π * radius / (0.6 * letterSizePx * 0.00185185185)))
```

Create a randomized text band per ring:

- Most band centers cluster around a forward angle with a spread of about `.65π`.
- Some rings use fully random narrow bands.
- Band width generally grows toward outer rings.
- Band softness varies from about `.07π` to `.20π`.

Fill slots with the phrase sequence separated by one to three blank slots. Convert slots outside the band or in soft random edges to the dot glyph.

Store per-instance attributes:

- Radius
- Initial angle
- Base speed
- Glyph size
- Glyph index
- Ring index

### Geometry and fitting

Render one quad per instance. Position the instance center with polar coordinates:

```text
center = [cos(theta), sin(theta)] * effectiveRadius
theta = initialTheta + time * speed + perRingOffset
```

Rotate each glyph tangent to the circle. Preserve circular geometry by fitting against the larger viewport dimension and allowing outer rings to clip.

### Entrance ripple

Start with a ripple at time zero. For each ring:

- Derive arrival time from radius after a small inner dead zone.
- Move the wavefront from the center to design radius `1.6` over `1.8s`.
- Fade each ring in over approximately `.5s` after arrival.
- Add a small radial push and glyph-size swell at the wavefront.

### Hover dissolution

Convert the pointer from pixel coordinates into fitted design coordinates. Smooth the pointer position rather than snapping it.

- Mouse radius: `.35` design units
- Use a bell-shaped distance falloff without a flat inner plateau.
- Multiply hover strength by approximately `2.5`.
- Compare a stable per-instance hash with the influence.
- Replace affected glyphs with the dot tile and dot size.

This produces a probabilistic dissolve, not a hard erased circle.

### Hold and charge

On pointer down:

- Enter `holding`.
- Increase charge toward one over about `.9s`.
- Slowly increase gather while the hold continues.
- Pull effective radius inward by up to `12%`.
- Freeze per-ring rotation without snapping by accumulating offsets.
- Add sparse glyph-to-dot glitches at approximately `9Hz`.
- Apply small per-instance tremors to a sparse subset.

When fully charged, change the cursor label to `RELEASE`.

### Release ripple

On pointer up after charging:

- Create a ripple lasting `1.8s`.
- Move the wavefront outward to radius `1.6`.
- Use a spatial bell width around `.85`.
- Push ring radius outward by up to `.045`.
- Increase glyph size by up to `50%` near the wavefront.
- Dissolve some letters into dots.
- Release gather and charge ring by ring as the wave reaches them.
- Allow multiple recent ripple events, capped to a small fixed array.

### Scroll, visibility, and reduced motion

- Feed smoothed scroll velocity into per-ring rotation offsets.
- Cap absolute scroll velocity before applying it.
- Pause animation outside the viewport with `IntersectionObserver`.
- Pause when the document is hidden.
- Debounce resize work.
- Cap device pixel ratio at 2.
- For reduced motion, render a settled static state rather than running the loop.

## 5. Local Canvas 2D fallback

The local reconstruction used a simpler Canvas version to avoid introducing the original rendering library. Preserve these facts when evaluating it:

- Thirty rings in the hero and twenty-two in the compact footer.
- Deterministic hashes replace `Math.random()` so reloads are stable.
- Radius, speed, size, band center, width, and softness follow the original distributions.
- Pointer position is held in a ref.
- A hold longer than roughly `650ms` creates a release ripple.
- The render loop is throttled to about `24 FPS`.
- Ring slots are capped at `180` in the hero and `130` in the footer.
- Dots are accumulated into one Canvas path and filled once.
- DPR is capped at 2.
- The cursor label is drawn directly into the Canvas.

Known losses compared with the target:

- Glyphs are not all tangent-rotated after performance correction.
- Slot density is reduced.
- Hold shake, glitch, ring-by-ring spring-back, and scroll energy are simplified or missing.
- Continuous visibility and reduced-motion handling are less complete.

Use this fallback only when its visible differences are acceptable. For a faithful production implementation, prefer the WebGL design above.

## 6. Showcase image-to-ASCII conversion

For each image:

1. Load it into an `Image` object.
2. Size the visible Canvas to the card, capping DPR at 2.
3. Create an offscreen sample Canvas of `120 × 37`.
4. Center-crop the source using `cover` geometry to match the sample aspect ratio.
5. Draw the crop into the sample Canvas.
6. Read pixels and compute luminance:

```text
luminance = (0.2126R + 0.7152G + 0.0722B) / 255
```

7. Sort luminances and use approximately the 2.5th and 97.5th percentiles as black and white points.
8. Normalize each cell within that range.
9. Apply gamma near `.78` to preserve midtone detail.
10. Map the result to a long density ramp from whitespace and punctuation through letters to `@` and `$`.
11. Draw light-grey glyphs on black with opacity increasing by luminance.

Layer the real image above the Canvas:

- ASCII opacity: 1 at rest, 0 on hover/focus.
- Image opacity: 0 at rest, 1 on hover/focus.
- Transition: about `.5s ease-out`.
- Image uses `object-fit: cover` and may scale to about `1.035` on hover.
- Support `focus-visible`, not pointer hover only.

Reusable lesson: use percentile contrast normalization rather than absolute luminance mapping so both dark and bright source images retain readable ASCII structure.

## 7. ASCII entrance curtain

Use a fixed Canvas above the entire page:

- `position: fixed; inset: 0; z-index: 200`
- `pointer-events: none`
- Character cell near `12 × 17px`
- Monospace font near `14px`
- Character set mixing binary digits, brackets, operators, punctuation, and hexadecimal-like symbols
- Duration near `1050ms`

Give every cell a deterministic position-based seed. During each frame, hide the cell after the global progress exceeds its seeded threshold. Change the displayed glyph slowly using a time step. Hide the Canvas when progress reaches one.

Disable the curtain under `prefers-reduced-motion: reduce`.

## 8. Micro-interactions

### Rolling text

Place two copies of the label in a one-line overflow-hidden wrapper. On hover:

- Move the first copy above the viewport.
- Move the second copy into its place.
- Use about `.52s` with `cubic-bezier(.23,1,.32,1)`.

### Dither frame

Use a repeating conic gradient at roughly `4 × 4px`, eight pixels of outer padding, an eight-pixel radius, and an inset dark surface. This creates the repeated terminal-window language across navigation, IDE, and reviews.

### Status ping

Animate a six-pixel orange dot by expanding a box shadow to roughly eight pixels while fading it to transparent over about `1.8s`.

### Scroll cue

Place a small white block at the top of a tall narrow control. Move it downward by roughly `42px`, fade it, and repeat over about `1.8s`.

### Review transition

Key the quote by active index. Animate from `opacity: 0` and `translateY(12px)` to the resting state over about `.4s`.

### FAQ transition

Use one open index. Animate the answer wrapper between `grid-template-rows: 0fr` and `1fr` over about `.4s`. Keep the paragraph overflow hidden and move bottom padding into the open state.

## 9. Responsive blueprint

Use `900px` as the principal breakpoint for this reconstruction.

Desktop to mobile changes:

| Desktop | Mobile |
|---|---|
| Centered fixed navigation with all anchors | Full-width navigation showing only mark and status link |
| Hero copy and scene side by side | Copy above a `75vh` scene |
| Problem terminal left, copy right | Stacked with explanatory copy first |
| Offset benefits list | Full-width list |
| Full IDE sidebar | Sidebar hidden |
| Two-column showcase | One column |
| Reviews at 65% width | Reviews near 90% width |
| Two-column pricing | One column |
| Sticky FAQ heading | Static heading |
| Three-column footer | One column |

Also reduce section padding, hide the scroll cue, reduce fixed card sizes, and preserve poster aspect ratios.

## 10. Accessibility and content replacement

- Give navigation a label.
- Give Canvas visuals meaningful labels or mark decorative layers hidden.
- Keep real `button` elements for FAQ, tabs, and review controls.
- Maintain `aria-expanded` for FAQ controls.
- Give images useful alt text.
- Expose hover reveals through keyboard focus.
- Disable or settle nonessential motion under reduced-motion preferences.

For personalization, replace data collections in this order:

1. Identity and navigation labels
2. Hero copy and CTA
3. Problem and benefit statements
4. Repository or process demonstration
5. Work images, titles, and destinations
6. Testimonials
7. Services or pricing
8. FAQ
9. Footer contact and legal links
10. The phrase rendered inside the character rings, if the new identity needs it
