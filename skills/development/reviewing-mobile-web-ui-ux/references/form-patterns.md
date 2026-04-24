# Mobile Form Patterns

Detailed criteria for evaluating forms in mobile-first web apps. Read this when the analysis includes forms, signup flows, login, checkout, or other data-entry flows.

---

## 1. Input Types and Keyboards

Using the wrong keyboard type is one of the most common and easiest mobile form issues to fix.

| Field | Recommended attribute |
|-------|---------------------|
| Email | `type="email"` |
| Phone | `type="tel"` |
| Number (without spinner) | `type="text" inputmode="numeric"` |
| Decimal number | `type="text" inputmode="decimal"` |
| Search | `type="search"` |
| URL | `type="url"` |
| Password | `type="password"` |
| PIN / code | `inputmode="numeric" pattern="[0-9]*"` |

**Common anti-pattern:** using `type="text"` for every field. Users then get an alphabetic keyboard for numeric inputs.

**Note:** for numeric data that does not behave well with `type="number"` because of formatting, validation, decimals, or spinners, it is often better to use `type="text"` + `inputmode` + explicit validation.

---

## 2. Autocomplete (`autocomplete`)

Enabling `autocomplete` reduces friction and completion time on mobile.

| Field | Recommended value |
|-------|------------------|
| Full name | `autocomplete="name"` |
| First name | `autocomplete="given-name"` |
| Last name | `autocomplete="family-name"` |
| Email | `autocomplete="email"` |
| Phone | `autocomplete="tel"` |
| Address | `autocomplete="street-address"` |
| City | `autocomplete="address-level2"` |
| Postal code | `autocomplete="postal-code"` |
| Credit card | `autocomplete="cc-number"` |
| New password | `autocomplete="new-password"` |
| Current password | `autocomplete="current-password"` |

**Anti-pattern:** `autocomplete="off"` on fields where autofill would be helpful.

**Common exception:** for OTPs or verification codes, prefer `autocomplete="one-time-code"` instead of disabling autocomplete outright.

---

## 3. Validation

### When to Validate
- **Inline (on blur or on input):** preferred for critical fields so the user sees the issue before submitting
- **On submit:** acceptable for short, simple forms
- **Mixed:** validate on blur and re-validate on submit

### How to Show Errors
- Show a specific message next to the affected field, not just a generic "Form error"
- Keep the message visible until the field is corrected
- Do not remove the error on focus alone. It should disappear only when the input becomes valid
- Use color + icon for error states, not color alone
- Scroll to the first invalid field after the user tries to submit

### Common Anti-Patterns
- A generic error message at the top of the form without identifying the affected field
- An error that disappears as soon as the field receives focus
- Validation that only happens on submit in long forms
- Requiring a specific format without explaining it upfront, such as password rules

---

## 4. Active Field Visibility

On mobile, the on-screen keyboard can cover a large portion of the screen.

**Check:**
- Is there automatic scrolling to keep the active field visible above the keyboard?
- In flows with fields near the bottom of the screen, does the layout adjust when the keyboard appears?
- Do action buttons such as submit or next remain accessible while the keyboard is open?

**Anti-pattern:** the active field is covered by the keyboard and the user cannot see what they are typing.

---

## 5. Required Fields

- Indicate required fields with more than just `*`, since many users do not know what it means
- Consider marking only optional fields if most fields are required, since that adds less noise
- The HTML `required` attribute improves accessibility for screen readers
- Do not rely only on red color to indicate required fields

---

## 6. Multi-Step Flows

- Show progress, such as step 2 of 4 or a progress bar, so users know how much is left
- Allow users to go back without losing entered data
- Consider appropriate local persistence for long forms so they are resilient to interruptions such as calls or accidental app closure
- Avoid `localStorage` for sensitive information; choose persistence based on risk and context
- The "Next" button should validate only the current step, not the entire form
- Do not ask for information that is not needed in the current step, to reduce perceived length

---

## 7. Labels and Placeholders

| Element | Correct use |
|----------|-------------|
| `<label>` | Always visible, preferably above the field on mobile. Never rely on a placeholder alone |
| `placeholder` | Use as a format hint, not as a replacement for the label. It disappears when the user types |
| Floating label | Acceptable if implemented correctly, with the label moving on focus or blur |

**Anti-pattern:** using only a `placeholder` as the label. It disappears when the user starts typing and they lose the reference.

---

## 8. Form Accessibility

- Every `<input>` should have an associated `<label>` or `aria-label`
- Submit buttons should use descriptive text, such as "Create account" instead of "Submit"
- Icons inside fields, such as a password visibility toggle, should have an explicit accessible name, for example via `aria-label`
- Prefer the natural DOM order and avoid positive `tabindex` unless there is a clear, controlled reason to use it
- Associate error messages with fields using `aria-describedby`
