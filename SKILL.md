---
name: replicate-websites-faithfully
description: "Reproduce a reference website by following the complete Content Architecture replication case study: source forensics, exact content capture, layout reconstruction, responsive architecture, ASCII image conversion, interactive character-ring motion, micro-interactions, performance correction, and browser acceptance testing. Use when the user asks to 复刻网站, 完整复刻, 像素级还原, clone a reference URL, preserve its interactions and motion, or build a faithful baseline before replacing content."
---

# Replicate Websites Faithfully

Use the Content Architecture project as the worked method. This skill records what actually succeeded, what failed first, how the source was investigated, how each effect was rebuilt, and where the final local implementation still differs from the reference.

Treat “replicate” as a fidelity contract. Do not reinterpret it as “make something with a similar style.”

Keep two layers distinct:

- **Reusable layer** — the contract, evidence workflow, five maps, implementation order, validation, and fidelity reporting in this file.
- **Worked-case layer** — the exact Content Architecture page structure, runtime discoveries, formulas, component behavior, performance corrections, and known gaps in `references/`.

## Read the case in the right order

1. Read [references/case-study.md](references/case-study.md) before planning or coding. It explains the chronology, the initial failure, and the corrected workflow.
2. Read [references/source-forensics.md](references/source-forensics.md) when a reference URL or opaque animation must be investigated.
3. Read [references/implementation-blueprint.md](references/implementation-blueprint.md) before implementing layout, ASCII visuals, motion, or interactions.
4. Read [references/verification-and-gaps.md](references/verification-and-gaps.md) before claiming fidelity or handing work back.

These files form one case-study playbook. Do not use only the generic checklist while skipping the implementation evidence.

## Preserve the replication contract

Classify the request:

- **Faithful baseline**: copy observable content, order, proportions, responsive behavior, interactions, and signature motion.
- **Personalized derivative**: first establish the faithful baseline, then replace copy, identity, media, and destinations.
- **Style adaptation**: borrow the visual language without claiming fidelity.

When the user says “直接复刻” or “先完全复刻”, choose the faithful baseline. Use mocks only for unavailable or unsuitable assets, centralize them, and disclose them.

Stay within accessible, authorized sources. Do not bypass authentication, paywalls, safety interstitials, or technical controls. Flag third-party assets and copy that need permission or replacement before public release.

## Follow the corrected workflow

### 1. Capture before interpreting

Inspect the live site at desktop and mobile widths. Record the complete section order, exact copy, images, links, grid, type, surfaces, fixed and sticky regions, responsive changes, and every visible interaction state.

Archive or inventory publicly delivered HTML, CSS, JavaScript, fonts, and images when useful. Search concrete labels and component clues before inventing an effect.

### 2. Build five maps

Create working maps for:

1. Content and destinations
2. Section and component hierarchy
3. Visual tokens and layout geometry
4. Interaction states and event triggers
5. Motion variables, timing, and rendering technique

Mark the three most distinctive or technically risky elements. In this case they were the interactive hero rings, image-to-ASCII showcase transition, and full-screen ASCII entrance curtain.

### 3. Prove signature effects early

Do not finish the generic page shell before testing whether the signature effect can be matched. Inspect the reference mechanism, derive its state model, build a minimal proof at the target viewport, and compare it with the reference.

For the Content Architecture hero, the decisive correction was discovering that the visual was 30 instanced character rings driven by a WebGL shader—not a procedural wood-grain texture. Rebuilding a visually adjacent noise field was therefore a failed approach.

### 4. Implement the page system

Establish tokens, fonts, page shell, responsive grid, repeated frames, data collections, and mock boundaries. Then integrate signature effects, secondary interactions, responsive variants, accessibility, and performance controls.

Prefer the project’s existing framework unless the user requests a migration. Preserve unrelated changes.

### 5. Validate behavior, not just screenshots

Reload the local app and test rest, hover, press, charged, release, scroll, resize, focus, and reduced-motion states where applicable. Compare geometry at matching viewports. Check console errors, missing assets, animation load, keyboard behavior, and responsive clipping.

Never call a dynamic effect complete from one still image.

### 6. Report the truth

Separate:

- Reference mechanism
- Local implementation
- Performance-driven compromises
- Unverified behavior
- Mock assets and links
- Licensing-sensitive content

Do not call a Canvas fallback “identical” when the reference uses WebGL instancing and the observable behavior differs.

## Core lessons from this replication

- Start from evidence, not visual memory.
- Exact content and section order do not compensate for a missing signature interaction.
- Analyze difficult motion before polishing low-risk sections.
- Search public runtime chunks using visible UI labels and module identifiers.
- Translate minified code into geometry, state, timing, and input rules; do not paste opaque source.
- Match the underlying mechanism when it determines density, orientation, smoothness, or input response.
- Batch Canvas primitives, cap DPR, pause hidden work, and consider WebGL when item counts grow.
- Keep desktop and mobile as distinct layout states, not merely scaled screenshots.
- Treat user feedback about a missing effect as a failed acceptance criterion and return to evidence gathering.

## Completion conditions

Complete the task only when:

- Section content and ordering are accounted for.
- Desktop and mobile layouts have been checked.
- Signature effects have mechanism notes and interaction evidence.
- Links, forms, accordions, tabs, carousels, and cursor states have been exercised.
- Runtime and asset errors are clear.
- Known differences are reported explicitly.
- The user can distinguish real content from mocks and replaceable assets.
