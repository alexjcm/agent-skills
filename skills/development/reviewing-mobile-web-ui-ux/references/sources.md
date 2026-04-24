# Reference Sources

Recommended and reliable starting sources for the evaluation criteria used by this skill. Use them as a starting point for analysis and research, not as an exclusive or exhaustive source set.

## Primary Sources

- **Nielsen Norman Group** — https://www.nngroup.com
  Usability, mobile UX, touch targets, and navigation patterns.
- **web.dev (Google Chrome team)**
  - Responsive Design: https://web.dev/learn/design
  - Accessibility: https://web.dev/learn/accessibility
  - Core Web Vitals / Performance: https://web.dev/performance

## Supporting Sources

- **Toptal Design** — https://www.toptal.com/designers
  Mobile usability, touch ergonomics, and complex interface design.
- **UXPin** — https://www.uxpin.com/studio/blog
  Visual hierarchy, consistency, and responsive design principles.
- **Smashing Magazine** — https://www.smashingmagazine.com
  Mobile web typography, readability, and UI best practices.
- **Mockplus** — https://www.mockplus.com/blog
  UI/UX best practices, motion design, prototyping, and interface design.
- **Lyssna** — https://www.lyssna.com/blog
  User research methods, usability testing, accessibility, and design validation.

## Source Map by Criterion

Use this as a starting map when deciding where to look first.

| Criterion | Good starting sources | Notes |
|---|---|---|
| Navigation patterns | NNGroup, web.dev, UXPin | Bottom nav, tabs, discoverability, wayfinding |
| Forms and input flows | web.dev, NNGroup, Smashing Magazine | Input types, validation, error handling, mobile keyboards |
| Accessibility | web.dev, NNGroup, Lyssna | Semantics, labels, focus, contrast, keyboard access |
| Touch ergonomics | NNGroup, web.dev, Toptal | Touch targets, spacing, one-hand use, tap behavior |
| Visual hierarchy and readability | UXPin, Smashing Magazine, NNGroup | Scanning, layout density, labels, typography |
| States and feedback | NNGroup, web.dev, Mockplus | Loading, empty states, error states, responsiveness |
| Dark patterns and deceptive design | NNGroup, Lyssna, broader reputable UX/legal sources | Use additional reliable sources when legal or regulatory risk matters |
| i18n and localization UX | web.dev, Smashing Magazine, broader reputable frontend sources | Labels, truncation, date/number formatting, locale-sensitive UI |
| URL state and deep-linking | web.dev, broader reputable frontend architecture sources | Shareable state, navigation recovery, browser/app consistency |
| Motion and reduced motion | web.dev, Smashing Magazine | Animation quality, accessibility, `prefers-reduced-motion` |

## Technical Reference Concepts

- **Multi-input support** (web.dev — Interaction: https://web.dev/learn/design/interaction): design for mouse, keyboard, and touch at the same time, not touch alone. This matters when the same user moves between mobile and desktop contexts.
- **ARIA labels** (web.dev — Accessibility: https://web.dev/learn/accessibility): interactive elements should have accessible descriptors for screen readers, such as `aria-label` on icons, buttons, and controls without visible text.
- **Focus visibility and keyboard flow** (web.dev — Accessibility: https://web.dev/learn/accessibility): focus states should remain visible and navigation order should match the task flow.
- **Reduced motion** (web.dev — Accessibility: https://web.dev/learn/accessibility): motion should respect user accessibility preferences and avoid making key interactions harder to follow.
