# Motion

Motion is part of done. A UI change is finished only when it ships with all three:

1. **Page transitions** between pages or views. On a multi-page site, use
   cross-document View Transitions (`@view-transition { navigation: auto; }`).
   In an app, use the framework's route transitions (in SvelteKit, `onNavigate`
   plus `document.startViewTransition`).
2. **Micro-interactions** on every interactive element. Hover, press and focus,
   and every state change (loading, progress, success, error, open and close),
   answer with short, visible feedback of 150–300 ms.
3. **A `prefers-reduced-motion` path** that removes the movement and keeps the
   feedback.

Animate only `transform` and `opacity`. Before you call the work done, confirm
that scrolling and the animations hold their frame rate.

## Tokens

```css
:root {
  --dur-1: 150ms;  /* hover, press, small swaps */
  --dur-2: 250ms;  /* open/close, state changes */
  --dur-3: 300ms;  /* upper limit for feedback */
  --ease: cubic-bezier(.2, .7, .2, 1);
}
```

Only scroll reveals (up to 0.8 s) and decorative loops may run longer.

## Patterns

- **Buttons:** lift at most 2 px on hover, and only inside
  `@media (hover: hover)`. Scale to 0.97 when pressed. Show a visible
  `:focus-visible` ring.
- **Cards:** lift at most 4 px on hover, on hover devices only.
- **Loading:** the button keeps its width, shows a spinner and is disabled. The
  screen never goes blank.
- **Success:** draw a check mark, or swap the icon (copy becomes a tick for about
  2 s, with `aria-live` text that says "Copied").
- **Error:** the message appears with a short shake, and without the shake under
  reduced motion. It names the problem and the fix.
- **Open and close:** use a native `<dialog>` with `@starting-style` and
  `transition-behavior: allow-discrete` on `display` and `overlay`. Show it as a
  bottom sheet on phones and a centred card from 600 px up. Fade the backdrop too.
- **Scroll reveals:**
  - opacity plus a 16–24 px rise, staggered 70 ms, at most 8 children;
  - never in the first screen;
  - content stays visible if JavaScript fails, so a class added by the script sets
    the hidden starting state, never the HTML or CSS alone.
- **Auto-moving content** (sliders, marquees):
  - don't autoplay before the visitor interacts, or show a visible pause button;
  - pause on hover, on focus, when off screen and when the tab is hidden (WCAG 2.2.2).
- **Status changes** (a booking moving from booked to on-the-way to done)
  animate the step that changed, not the whole page.

## Reduced motion

Write movement inside `@media (prefers-reduced-motion: no-preference)`, so the
default is still and movement is opt-in. Under `reduce`:

- Remove translation, scale, parallax, shake, autoplay and smooth-scroll hijacking.
- Keep opacity and colour changes, so every interaction still visibly answers.
- Reduce view transitions to a short cross-fade.

Many Windows machines report `reduce`, including test browsers and possibly the
owner's own screen. Check the site in both modes. Under `reduce` it must still
look complete and alive. Never leave content stuck at its hidden starting state,
and never drop a visual that is part of the design. If the owner wants the motion
regardless of the setting, follow them, offer a URL opt-out such as `?still`, and
record the decision. Some libraries honour reduced motion on their own (Lenis
defaults `respectReducedMotion` to true). Check that they follow the project's choice.

## Frame rate

- **Measure; don't guess.** Record a Chrome performance trace while scrolling,
  in Pixel 7 emulation with 4× CPU throttling, and read the frame times. In one
  project, jank blamed on the scroll libraries turned out to be a WebGL background.
- **Costly on every scroll frame:**
  - `backdrop-filter`;
  - `mix-blend-mode` overlays such as film grain;
  - big shadows or filters on moving elements.
  Remove them where they barely show. Removing a grain overlay and a frosted
  button took one page from 3.5 ms to 2.0 ms of compositor time per frame.
- **WebGL and three.js:** lazy-load, desktop only, capped at 30 fps, with low
  segment counts. Use `ResizeObserver`, not a layout read every frame.
- Never read layout (`getBoundingClientRect`, `offsetTop`) inside a scroll
  handler on every frame.
- When a real phone lags and the cause isn't clear, ship a temporary on-device
  probe. It times frames while the owner scrolls and switches each suspect off
  in turn. It ends on one table showing late frames, median, p95 and worst per
  suspect, which the owner screenshots. Remove the probe afterwards.
- Check the installed app as well as the browser. A WebView can lag where Chrome
  doesn't (`android-app.md`).
