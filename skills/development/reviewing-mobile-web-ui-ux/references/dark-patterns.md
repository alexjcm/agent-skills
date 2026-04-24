# Mobile Dark Patterns

Catalog of deceptive design patterns to detect during UX/UI audits. Read this when the analysis includes conversion flows, subscriptions, permissions, notifications, or any point where business incentives may conflict with user interests.

---

## Common Patterns

### 1. Confirmshaming
**What it is:** the rejection option uses guilt-inducing or shaming language to pressure the user into accepting.

**Examples:**
- "No thanks, I'd rather pay more"
- "No, I'm not interested in discounts"
- "I'd rather lose money"

**How to detect it in code:** review the text of secondary, dismiss, or close actions in modals, popups, permission prompts, and notification banners.

**Why it is a problem:** it manipulates users emotionally instead of informing them. This damages trust and can create negative brand perception.

---

### 2. Nagging
**What it is:** repeated, persistent interruptions for permissions, notifications, ratings, or subscriptions until the user gives in out of fatigue.

**Examples:**
- Notification permission modal shown in every session
- "Install the app" banner that cannot be dismissed permanently
- Rating prompt shown on every app open

**How to detect it in code:** review the logic that controls when prompts, alerts, or banners reappear, especially if there is no durable "already dismissed" or "already rejected" state.

---

### 3. False urgency / False scarcity
**What it is:** creating artificial pressure through time limits or scarcity cues that do not reflect reality.

**Examples:**
- Countdown timer that resets
- "Only 2 left" when that is not true
- "Offer expires in 10 min" shown to every user at all times

**How to detect it in code:** inspect timer logic, stock or availability values, and whether urgency messages are hardcoded, recycled, or detached from real state.

---

### 4. Misdirection
**What it is:** the design steers the user toward the business-preferred action while visually minimizing the option the user is more likely to want.

**Examples:**
- Large, colorful "Accept" button vs. tiny gray "Decline" text
- Annual plan preselected when the user may want the monthly plan
- "Continue" action that advances a flow the user did not intentionally choose

**How to detect it in code:** inspect default states, preselected options, and components that create asymmetry between choices of similar functional weight. Visual confirmation may be needed to verify severity.

---

### 5. Roach Motel
**What it is:** signing up or enabling something is easy, but canceling or disabling it is deliberately difficult.

**Examples:**
- Subscription in 2 steps, but cancellation requires a phone call
- "Delete account" buried under 4 levels of settings
- Cancellation option available only at limited times

**How to detect it in code:** compare the depth, friction, and number of steps between enabling and disabling a feature, subscription, or account setting.

---

### 6. Trick Questions
**What it is:** questions or checkboxes written in a confusing way, often with double negatives or ambiguous wording, to obtain consent the user did not intend to give.

**Examples:**
- "Uncheck this box if you do not want to keep receiving communications"
- Pre-checked newsletter subscription checkbox
- "Do you not want to stop receiving offers?" (double negative)

**How to detect it in code:** inspect checkbox labels, helper text, consent copy, and default checked states in HTML or component props.

---

### 7. Sneak into Basket
**What it is:** adding items, fees, or services to the cart or checkout without the user's explicit consent.

**Examples:**
- Travel insurance preselected in checkout
- Suggested donation added automatically
- Processing fee revealed only in the last step

**How to detect it in code:** inspect preselected checkout items, hidden defaults, and charges that appear without a clear user action.

---

### 8. Disguised Ads
**What it is:** advertising content designed to look like organic content or navigation UI.

**Examples:**
- Sponsored result that looks identical to an organic one
- "Continue" button that is actually an ad
- "Recommended articles" that are ads without clear labeling

**How to detect it in code:** inspect differences between sponsored and organic content blocks, and check whether ad labeling is present, visible, and unambiguous.

---

## How to Report Dark Patterns

When identifying a dark pattern:
1. Name the specific pattern, for example `confirmshaming`
2. Indicate where it appears, such as the screen or component
3. Explain why it is problematic, focusing on user impact and any regulatory or legal risk if relevant
4. Propose an ethical alternative that preserves the function without manipulation

**Prioritization:** dark patterns should generally be reported as **Critical** or **Important**, not Optional, because they directly affect trust and may create regulatory or legal risk depending on the jurisdiction.

---

## Context Notes

Not everything that looks questionable is a dark pattern. Before reporting one:
- Check whether there is a legitimate functional reason, such as reducing genuine friction in a well-understood flow
- Distinguish between careless design and manipulative design
- Do not flag a pattern only because one option is visually stronger; look for unjustified asymmetry, hidden cost, repeated pressure, or deliberate friction against the user-preferred path
- The impact on the user is the main criterion, not the design team's intent

## Legitimate Friction vs. Manipulative Friction

- **Legitimate friction:** confirmation steps, warnings, or extra verification that protect the user from mistakes, fraud, privacy loss, or irreversible actions
- **Manipulative friction:** extra steps, wording, defaults, or interruptions designed mainly to steer the user away from their preferred choice or toward a business-preferred outcome
