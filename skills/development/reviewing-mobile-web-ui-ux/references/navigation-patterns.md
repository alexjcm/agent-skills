# Mobile Navigation Patterns

Reference guide for deciding which navigation pattern is likely to work well and when. Use this when evaluating or recommending navigation changes in mobile-first web apps or PWAs.

---

## 1. Bottom Navigation (Fixed Tab Bar)

**What it is:** a bottom bar with 3-5 icons and labels for primary sections. It often works best when it remains available during navigation and scrolling.

**When to use it:**
- There are 3-5 primary sections of similar importance
- Users need to switch sections frequently
- The app has parallel, independent flows

**When to avoid it:**
- There are more than 5 sections and touch targets become crowded
- The flow is linear or one-directional, such as a wizard or onboarding
- Screens rely on horizontal content gestures that would conflict with a bottom navigation pattern

**Common anti-pattern:** a bottom tab bar that disappears during scroll even though users rely on it for orientation. Evaluate whether keeping it persistently available improves wayfinding without harming usable space too much.

---

## 2. Top Navigation Bar

**What it is:** a top bar with a screen title, back action, and contextual actions.

**When to use it:**
- The navigation is hierarchical, such as drill-down flows
- Detail screens make back navigation the primary action
- It is combined with bottom navigation for secondary hierarchy

**Sticky vs. non-sticky:**
- **Sticky:** often useful if the screen has frequent top actions, such as search or filters, or if the section name helps users stay oriented
- **Non-sticky:** often preferable if the content needs the vertical space and users do not lose orientation without the header

**Common anti-pattern:** a header with too many competing actions, which splits attention and consumes premium screen space.

---

## 3. Hamburger Menu (Drawer)

**What it is:** a three-line icon that opens a side panel or modal with the full navigation.

**When to use it:**
- There are more than 5 sections with different usage frequency
- The navigation hierarchy is deep
- Some sections are infrequent destinations, such as settings, profile, or help

**When to avoid it:**
- There are only a few primary sections that users visit often
- The main navigation needs to be highly discoverable and low-friction
- The hamburger would increase taps for common tasks without clear benefit

**Useful heuristic:** if a hamburger menu is used, adding a text label such as "Menu" often improves discoverability, especially for new or infrequent users.

---

## 4. Horizontal Tabs (Top Tabs / Segmented Control)

**What it is:** a row of horizontal tabs below the header for switching between related views in the same context.

**When to use it:**
- There are 2-4 views of the same content, such as Month / Week / Day
- Users frequently switch or compare between related views
- The views are parallel, not hierarchical

**When to avoid it:**
- Labels are long and become truncated or hard to scan
- Too many tabs create overflow without clear discoverability
- The tabs actually represent separate app sections rather than alternate views of the same context

---

## 5. Overflow Tabs (Horizontally Scrollable Tabs)

**What it is:** a row of tabs that exceeds the screen width and can be scrolled horizontally.

**When to use it:**
- There are many categories of similar relevance and users choose their context from them
- The content behaves like a feed filtered by category, such as news or products

**Strong recommendation:** provide a visible overflow cue, such as a fade or shadow at the edge. Without it, users may assume the visible tabs are the only options.

**Common anti-pattern:** horizontally scrollable tabs with no visual overflow signal, so users do not discover the hidden options.

---

## 6. Back Navigation (Hierarchical Navigation)

**What it is:** a back action or gesture that returns the user to the previous screen in a hierarchical flow.

**When to use it:**
- Drill-down flows such as list -> detail -> sublevel
- Sequential task flows where users may need to return

**Key rules:**
- Back navigation should usually return users to the previous meaningful state, including scroll position or filters when that context matters
- In PWAs, browser back and app-level back behavior should feel consistent rather than competing with each other
- When a view can be shared, refreshed, or resumed, evaluate whether URL state or deep-linking should preserve the navigation context

---

## Quick Decision Heuristic

```
How many primary sections does the app have?
│
├─ 1-2 -> Tab bar or linear flow without strong primary navigation
├─ 3-5 -> Bottom navigation often works well
├─ 4-5 with different weights -> Bottom nav for frequent sections + drawer for secondary sections
└─ 6+ -> Drawer / menu becomes more likely

Are the views part of the same context or different sections?
│
├─ Same context, alternate view -> Top tabs
└─ Independent sections -> Bottom nav or drawer

Does the user mainly navigate deeper into hierarchy?
└─ Yes -> Top navigation bar + back
```

Use this as a quick heuristic, not a rigid rule set.

---

## Common Combinations in Mobile Web Apps

| Pattern | Example use case |
|--------|---------------|
| Bottom Nav + Top Bar | E-commerce app: sections such as home, catalog, cart, and profile, with a contextual header inside each section |
| Top Bar + Tabs | Dashboard with alternate views such as Week / Month / Year |
| Bottom Nav + Drawer | Complex app: frequent destinations in bottom nav, secondary sections in a drawer |
| Top Bar Only | Checkout or step-by-step wizard flow |
| Top Bar + URL state | Search, filters, or tabbed views where users may refresh, share, or resume the current state |
