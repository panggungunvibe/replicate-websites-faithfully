# Replication Acceptance Matrix

Use this matrix before calling a replication complete. Mark non-applicable rows explicitly rather than silently skipping them.

| Area | Desktop | Mobile | Evidence |
|---|---:|---:|---|
| Section order and content | ☐ | ☐ | Screenshots or DOM inventory |
| Page width, grid, and alignment | ☐ | ☐ | Overlay or side-by-side comparison |
| Typography and line breaks | ☐ | ☐ | Matched viewport screenshots |
| Colors, borders, shadows, and surfaces | ☐ | ☐ | Token check plus screenshots |
| Navigation and anchor behavior | ☐ | ☐ | Interaction check |
| Sticky and fixed positioning | ☐ | ☐ | Scroll-state screenshots |
| Hover and focus states | ☐ | ☐ | Before/after evidence |
| Click, drag, hold, and release states | ☐ | ☐ | Interaction sequence or frames |
| Entrance and transition motion | ☐ | ☐ | Start, peak, and settled frames |
| Scroll-linked motion | ☐ | ☐ | Multiple scroll positions |
| Signature Canvas/WebGL/SVG effect | ☐ | ☐ | Mechanism notes plus frames |
| Images, crops, and aspect ratios | ☐ | ☐ | Screenshot comparison |
| Accordions, carousels, and forms | ☐ | ☐ | State checks |
| External links and mock destinations | ☐ | ☐ | Link inventory |
| Keyboard navigation and labels | ☐ | ☐ | Focus traversal |
| Reduced-motion behavior | ☐ | ☐ | Media-query test |
| Console and runtime errors | ☐ | ☐ | Clean error log |
| Asset failures and layout shifts | ☐ | ☐ | Network/visual check |
| Animation smoothness | ☐ | ☐ | Interaction observation |

## Severity

- **Blocker**: missing signature effect, wrong section architecture, broken primary interaction, or unusable responsive layout.
- **Major**: clearly wrong proportions, typography, crop, timing, or navigation behavior.
- **Minor**: small spacing, easing, or anti-aliasing differences that do not change the design read.
- **Mock**: intentionally temporary content, asset, or destination that is centralized and disclosed.

Do not deliver with unresolved blockers. Report major and mock items explicitly.

## Final report template

```text
Fidelity status:
Tested viewports:
Tested interactions:
Matched signature elements:
Known approximations:
Mock assets or links:
Assets requiring replacement or licensing:
Console/runtime status:
```
