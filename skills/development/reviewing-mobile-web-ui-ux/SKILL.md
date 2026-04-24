---
name: reviewing-mobile-web-ui-ux
description: Review the source code of mobile-first web apps and PWAs to evaluate mobile UI/UX quality, identify anti-patterns, and deliver prioritized recommendations. Use when the user asks to review, audit, analyze, or validate a mobile web app or PWA for navigation, forms, accessibility, touch ergonomics, dark patterns, consistency, localization, URL state, deep-linking, or mobile design patterns. Also trigger on Spanish requests such as revisar, auditar, analizar, or validar the UX/UI of a mobile web app or PWA, including navegacion, formularios, accesibilidad, ergonomia tactil, dark patterns, consistencia, localizacion, estado en URL, deep-linking, or patrones mobile.
---

# Mobile Web UI/UX Reviewer

Review the source code of a mobile-first web app or PWA to evaluate whether it follows strong mobile UI/UX practices, identify anti-patterns, and propose prioritized improvements. The sources in `references/sources.md` are recommended and reliable starting points for evaluation criteria and further research when needed.

**Review mode:** choose between `quick review` and `deep audit` based on user intent and project scope.
- Use `quick review` when the user asks for a fast validation, a narrow check, or a lightweight pass over a small surface area
- Use `deep audit` when the repo is medium/large, the flow is business-critical, multiple screens or components are involved, or the user explicitly asks for an audit
- If the intent is ambiguous, default to `deep audit` for medium/large repos and `quick review` for clearly narrow requests

**Primary input:** the app source code, including components, HTML, CSS, routes, and state logic. Always review the code first. If the app can be run locally without disproportionate setup effort, prefer rendering it locally before requesting extra context. Screenshots complement the analysis when they help validate visual hierarchy, density, states, or the rendered behavior of key screens, and the user may provide them from the start if desired.

**Scope:** mobile-first web apps and PWAs. Do not use this skill for backend logic or desktop-first design reviews.

**Review-only rule:** this skill is for analysis and recommendations, not implementation. Do not modify the code while performing the review. After the audit, the user decides whether any of the recommended changes should actually be implemented.

---

## Analysis Protocol

### Inspection order from source code
1. **Navigation** - routes, navigation components, screen hierarchy, active state
2. **UI components** - primary action per screen, visual weight implied by markup and layout
3. **Forms** - input types, `inputmode`, `autocomplete`, validation, error handling
4. **Accessibility** - `aria-label`, `role`, `alt`, `tabindex`, semantic HTML such as `button` vs clickable `div`
5. **Responsive behavior and ergonomics** - breakpoints, CSS units, interactive target sizing
6. **States and feedback** - loaders, skeleton screens, empty states, network error handling
7. **Dark patterns** - rejection copy, flow exits, repeat interruptions, manipulative defaults

### When to request visual context
Request screenshots or a flow description when:
- Visual behavior cannot be inferred from code alone, such as runtime data, animation timing, or conditional rendering
- A visual hierarchy problem is suspected and needs rendered confirmation
- The user mentions a specific screen without clearly identifying it in code
- It is unclear whether an element is visible in the primary flow
- The project is medium or large, many components are involved, or screenshots of the main screens would materially improve analysis accuracy

**Before requesting screenshots:** if the repo appears runnable and local rendering is feasible, try to run the app and inspect the relevant screens first. Ask for screenshots only when local rendering is not possible, too costly, or still insufficient to resolve the UX question.

**If the user already provides screenshots:** use them as a complement, not a replacement for code review.

**If the code is partial or no flow is specified:** review what is available, state what could not be assessed, and say what extra context would improve the audit.

### Per-screen audit matrix
When reviewing one or more concrete screens, explicitly check:
- navigation and orientation
- empty state
- loading state
- error state
- primary CTA clarity
- accessibility basics
- touch ergonomics
- copy and locale consistency

---

## Criteria and Sources

Apply principles documented in reliable reference sources. Use [references/sources.md](references/sources.md) as a recommended starting point for evaluation criteria, then consult additional reliable sources when the case requires more specific validation, fresher evidence, or deeper support.

Prioritize clarity, visual hierarchy, predictable navigation, readability, accessibility, touch comfort, consistency, and friction reduction.

Treat specific thresholds as practical heuristics unless a stricter source-backed requirement is clearly relevant. Cite sources when the recommendation is non-obvious, likely to be challenged, or benefits from technical backing. Skip citations for widely established findings.

---

## Recommended Bias

**Favor:** visible and stable navigation, compact headers, short and consistent labels, one clear primary action per screen, clean layouts, intentional empty states, visible loading feedback, and incremental improvements before radical redesigns.

**Be skeptical of:** overloaded headers, unnecessary hamburger menus for small information architectures, cramped tabs, secondary actions with too much prominence, mixed languages, labels that truncate badly, desktop layouts compressed into mobile, color-only state communication, visible elements that do not respond to touch, and screens with no empty or loading states.

---

## What to Evaluate

Evaluate only the areas that are relevant to the case. Omit the rest without calling them out.

### Fixed finding categories
Use these categories when they improve clarity:
- `navigation`
- `forms`
- `accessibility`
- `touch-ergonomics`
- `states-feedback`
- `theming-dark-mode`
- `content-density`
- `i18n-l10n`
- `url-state-deep-linking`
- `dark-patterns`

### 1. Navigation
- Clarity, placement, and predictability of the navigation pattern
- Ease of moving between sections and visibility of active state
- If there is a primary tab bar, evaluate whether keeping it available during scroll improves orientation more than it harms usable space
- If links do not fit on screen, prefer horizontally discoverable overflow over collapsing everything into a hamburger menu by default
- If a hamburger menu is used, evaluate whether a text label such as "Menu" would improve discoverability
- Check whether tabs, filters, searches, and alternate views preserve useful state while navigating
- Check whether the URL reflects meaningful state when users need to share, refresh, or resume a view

> For detailed navigation-pattern guidance, read [references/navigation-patterns.md](references/navigation-patterns.md).

### 2. Visual hierarchy
- What users see first and whether the primary action stands out
- Whether too many elements compete for attention or the header consumes too much space
- Users often scan before they read; hierarchy should guide attention without relying on dense text

### 3. Clarity of use
- Whether the screen is understandable quickly
- Whether labels are clear, actions are grouped logically, and the next step is obvious

### 4. Mobile ergonomics
- Interactive controls should usually provide a comfortable touch target, commonly around 44-48 px, with enough spacing to reduce accidental taps
- View-tap asymmetry: elements that look actionable but do not respond to touch
- One-handed comfort and how quickly useful content appears
- State persistence: mobile users get interrupted, so forms and flows should recover gracefully when the app loses focus

### 5. Copy, consistency, and theming
- Short labels, consistent language, and clear action/state wording
- Line length, wrapping, and truncation appropriate for mobile readability
- Consistent behavior for filters, search, sort, and view switching across the app
- If dark mode exists, whether contrast, icons, and hierarchy remain usable
- Consistent tone across CTAs, success messages, warnings, and errors

### 6. Localization and i18n
- Mixed languages in labels, CTAs, errors, and navigation
- Truncation or wrapping issues in longer translations
- Date, time, currency, and number formatting consistent with locale
- Technical or ambiguous copy that works in one language but becomes confusing when translated
- Inconsistent tone or terminology across error, success, and CTA copy

### 7. Data-dense content
- For tables in mobile, choose a strategy based on task and density: simplify columns if a few are enough, use horizontal scroll with a visible cue when exact values matter, or use expandable cards when the data has hierarchy
- Consider replacing tables with visual summaries when the user needs trends rather than exact values
- Evaluate empty states when no data exists
- Evaluate loading states and whether feedback is visible while data is fetched

### 8. Accessibility and interaction
- WCAG AA contrast as a baseline: 4.5:1 for normal text, 3:1 for large text and many UI elements
- States and data should not rely on color alone; combine color with icon, label, or pattern
- Main controls should remain reachable by keyboard and with increased text size
- Verify focus visibility explicitly on interactive elements
- Verify accessible names such as labels or `aria-label` on key controls and icon-only actions
- Verify touch-target comfort for primary and frequent actions
- Verify that state communication does not depend on color alone
- `prefers-reduced-motion` should be respected when motion is relevant
- Check `touch-action`, `tap-highlight`, safe areas, and touch-target comfort in mobile web contexts
- Check viewport behavior when the keyboard opens near bottom-screen fields

### 9. PWA and mobile-web runtime behavior
- Safe-area handling around notches, home indicators, and bottom actions
- Keyboard overlap, especially with sticky bottom actions and form CTAs
- Pull-to-refresh conflicts with custom scroll regions, tabs, or gestures
- Install prompts and banners that interrupt key tasks or create repeated friction
- Offline banners, offline fallback states, and reconnect behavior
- Standalone mode behavior when the app is installed as a PWA

### 10. UX risks
- Unnecessary friction, loss of context, low-visibility actions, visual overload, and predictable user errors

### 11. Mobile forms
- Keyboard type appropriate to the data: `type="email"`, `type="tel"`, `inputmode="numeric"`, and so on
- Inline validation is often preferable to submit-only validation for critical fields
- Error messages should be specific, placed near the affected field, and persistent until corrected
- Required fields should be indicated with more than a bare asterisk
- `autocomplete` should be configured where autofill reduces friction
- Check that the keyboard does not cover the active field without compensating scroll or layout adjustment
- Multi-step flows should show progress, allow back navigation, and preserve data when users go back

> For detailed form guidance, read [references/form-patterns.md](references/form-patterns.md).

### 12. Dark patterns
- **Confirmshaming:** guilt-inducing rejection language
- **Nagging:** repeated interruptions for permissions, notifications, ratings, or subscriptions
- **False urgency / scarcity:** pressure cues that do not reflect reality
- **Misdirection:** business-preferred actions made disproportionately prominent
- **Roach motel:** easy signup, difficult cancellation or exit
- **Trick questions:** ambiguous or double-negative consent language

> For examples and reporting guidance, read [references/dark-patterns.md](references/dark-patterns.md).

---

## Output Format

Use only the sections that add real value. Omit irrelevant sections silently.

1. **General diagnosis** - open with the most critical issue in the first sentence
2. **Main findings** - relevant strengths and problems
3. **Prioritized recommendations** - Critical / Important / Optional
4. **Concrete proposals** - specific changes that can realistically be applied
5. **Anti-patterns detected** - what pattern is problematic and why it does not fit the case
6. **UX risks** - what may happen if the design remains unchanged

If the user provides concrete files or the issue is traceable in code, cite `file:line` where possible.

For each important finding, prefer this structure:
- **Finding**
- **Severity**
- **Impact**
- **Evidence**
- **Recommended change**
- **File:line** when available

If a recommendation involves layout, hierarchy, or navigation reorganization, include a short textual wireframe or screen-structure proposal.

Respond like a UX/UI consultant: critical but useful, concrete, justified, and decision-oriented. Do not flag issues based only on stylistic preference. Prioritize what affects clarity, usage, navigation, ergonomics, accessibility, or manipulation risk.

### Recommended prioritization
- **Critical** - blocks key tasks, creates likely errors, breaks baseline accessibility, causes serious loss of context, or constitutes a dark pattern / trust risk
- **Important** - creates recurring friction, reduces understanding, makes navigation harder, or worsens efficiency in important screens
- **Optional** - improves clarity, consistency, or refinement but does not block the main task or create high risk

### What not to flag on its own
- Pure stylistic preferences with no clear functional impact
- Unusual visual choices that remain understandable and usable
- Differences of taste around density, branding, or tone when they do not affect comprehension, navigation, accessibility, or ergonomics

---

## Decision Rules

- **Few primary sections** -> consider bottom navigation if the current navigation creates friction
- **Navigation disappears on scroll** -> evaluate whether sticky navigation or a persistent tab bar would improve orientation
- **Hamburger icon without text** -> consider recommending a visible "Menu" label
- **Header with too many responsibilities** -> separate branding, navigation, and secondary actions
- **One clearly primary action** -> give it clearer visual priority and reduce competition
- **Long labels or mixed language** -> simplify wording and unify language
- **Too much noise above the fold** -> reduce header weight and surface useful content earlier
- **Complex table on mobile** -> simplify, use visible horizontal overflow, or switch to expandable cards depending on task and hierarchy
- **Table appears problematic on mobile** -> do not flag it as a UX issue if the product already provides a real mobile variant in another responsive layout or state for the same task
- **Screen with no empty state** -> recommend an intentional empty state with a clear message and next action
- **No loading feedback** -> recommend skeletons or another visible loading state
- **Forms or multi-step flows** -> verify autosave or other context-preserving recovery behavior
- **Small changes are enough** -> prefer incremental improvements before redesign
- **Pattern is ambiguous** -> explain when it helps and when it hurts instead of giving a blanket verdict
- **Frequent and infrequent actions compete** -> optimize for the frequent path
- **URL state would materially help resume or sharing** -> recommend deep-linking or meaningful state persistence
- **Frequent motion or transitions** -> verify reduced-motion support and ensure motion does not harm legibility or control
- **Multi-language app or mixed locales** -> review truncation, formatting, and terminology consistency

---

## Recommendation Examples

- "Moving the main navigation to a fixed bottom bar could improve ergonomics and free vertical space if there are only four primary sections."
- "The logout action should not compete with primary navigation if it is secondary and infrequent."
- "A more compact header would surface useful content sooner and reduce the feeling of saturation."
- "Review the section names. If they mix technical and everyday language, a single naming strategy will reduce cognitive load."
- "Avoid cramped top tabs when labels are long and the available width is limited."
- "Add an empty state with guidance so users do not assume the screen is broken when there is no data yet."
- "A hamburger icon without a text label may reduce discoverability for new users. Adding 'Menu' could lower that friction."

## Trigger Examples

- "Review the mobile UX/UI of this PWA"
- "Audit this mobile web app for accessibility and navigation issues"
- "Check these forms for mobile UX problems"
- "Revisa la UX/UI móvil de esta app web"
- "Audita esta PWA en navegación, formularios y accesibilidad"
- "Detecta dark patterns y fricción UX en este flujo móvil"
