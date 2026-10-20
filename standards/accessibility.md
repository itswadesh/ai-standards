# Accessibility

The target is Lighthouse accessibility 100 and axe with zero violations on every
page, plus a keyboard walk of every flow.

## Rules

- **Contrast:** 4.5:1 for body text, 3:1 for large text, icons and UI
  boundaries such as input borders. Check every state: hover, disabled, error,
  and the hidden state before a reveal. Text over photos needs a dark shade.
  Measure contrast over the real photo pixels, not over the fallback colour.
  Text set with an alpha colour or inside a faded parent is measured after
  compositing: black at 50% on white is about 3.5:1 and fails.
- **Dark mode:** check every state mark in both themes, measured against what
  it sits on: the chosen tab's underline, a pressed toggle, a selected card's
  edge, a focus ring, a filled chip on a card. A brand or "primary" colour that
  is dark in the dark theme makes all of them vanish at about 1:1. Give the
  marks one token that flips (the brand colour in light, the text colour in
  dark), and fills that say "on" a matching text colour. A chip or tile filled
  with the card's own colour vanishes the same way.
- **CSS variables:** a `var(--x)` naming a variable nothing declares fails
  silently: the rule falls back to inheriting, and a `color-mix()` naming it
  is dropped whole. Keep a test that fails on any variable read but never
  declared (not counting declarations inside print rules or comments).
- **Focus:** a visible global `:focus-visible` ring. Don't remove outlines
  without a replacement.
- **Semantics:**
  - Buttons are `<button>` and links are `<a href>`; never a clickable `<div>`.
  - Use one `h1` per page, with headings in order.
  - Use landmarks: `header`, `nav` with an `aria-label` when there is more than
    one, `main` and `footer`.
- **Names:**
  - Every icon-only control has an `aria-label`.
  - Decorative SVGs and duplicate content are `aria-hidden="true"`.
  - Every `<img>` has `alt`, which is empty for decorative images.
- **Forms:**
  - Every input has a visible `<label>`.
  - A floating label is always in the DOM and bound with `for`/`id`. A label
    rendered only while the field is focused or filled leaves the empty field
    named by its placeholder alone.
  - Each error sits next to its field, is linked with `aria-describedby`, and is
    announced.
  - After a failed submit, an error summary takes focus, and each item in it
    links to and focuses its field. Keep what the visitor typed. Show success
    only after the server accepts.
  - Use the right `type`, `inputmode` and `autocomplete`: `tel`, `email`,
    `postal-code`, `street-address`.
  - Radio-style choices are real radios, or have `role="radio"` with
    `aria-checked`.
- **Dialogs:** use a native `<dialog>` opened with `showModal()`. It traps focus,
  makes the page inert and closes on Escape. Return focus to the control that
  opened it.
- **Live updates:** "Copied", "Booked" and status changes are announced through
  `aria-live="polite"`.
- **Touch:** targets are at least 44×44 px with space between them.
- **Motion:** follow the reduced-motion path in `motion.md`. Moving content that
  lasts more than 5 seconds can be paused (WCAG 2.2.2).
- **Breadcrumbs:** an `<ol>`, with `aria-current="page"` on the last item and
  the separators drawn by CSS.
- **Language:** set `lang` on `<html>`, and `lang` on any passage in another
  language. Translated pages carry `hreflang` alternates.
- **Zoom:** never disable zoom. The layout holds at 200% text size.

## Checking

- Run axe on each page type once the page has settled. Wait for fade-ins to
  finish, or axe reports contrast failures that aren't real. Use a
  `?still`-style switch or wait for the animations.
- A strict CSP blocks an injected axe script. Run it in a browser context that
  bypasses CSP (Playwright `bypassCSP: true`).
- Walk each main flow with the keyboard alone: open the menu, book, pay, close
  the dialogs.
- Check the reduced-motion mode and the phone viewport, not just the desktop.
