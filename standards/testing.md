# Testing and verification

## Layers

- **Unit:** formatting (₹, dates in IST, 12-hour times), phone and pincode
  validation, UPI link and QR building, pricing, status transitions, token and
  password helpers, SEO and JSON-LD builders.
- **API integration:** against a real database, with a fresh database per test.
  Cover booking without login, the capacity race (concurrent inserts), limits
  and the honeypot, token access (another device can't read a booking), staff
  roles, and the full status flow.
- **End to end (Playwright):**
  - at a phone viewport (Pixel 7), against a production build and a throwaway
    test database;
  - the whole business flow: a customer books, the admin sees it, the crew
    confirms and advances it, the customer pays and reviews;
  - a returning customer sees their details filled in;
  - a reduced-motion run.
- **Type checks:** `svelte-check`, `astro check` or `tsc` must show 0 errors and
  0 warnings.
- Add a test with every feature and every bug fix. A test of a fix must fail
  without the fix.
- Prove each check can fail. For every validator (a watertight-mesh test, a
  width rule, a schema check), break the code it guards on purpose and watch a
  test turn red. A checker that only ever sees good input can break without
  any test noticing.

## Writing robust e2e tests

- Select by role and accessible name (`getByRole('button', { name: … })`), not
  CSS classes.
- Pick enabled items explicitly, such as `.slot:not(:disabled)`, because booked
  slots and days are disabled buttons.
- Fake browser features to cover every branch. Replace `navigator.share` with
  a recorder or remove it, and fake `window.Capacitor.Plugins` to test the
  in-app paths: success, cancel and failure.
- Normalise line endings when comparing clipboard text; the Windows clipboard
  stores CRLF.
- Never point tests at production. They create real bookings and send real SMS.

## Visual and behaviour checks

- Screenshot every changed page at 360 or 390 px and 1440 px, and look at each
  one. For multilingual sites, add one non-Latin language.
- When restyling some pages, prove the untouched ones did not change: take a
  screenshot before you start and pixel-diff it after.
- Check the console: 0 errors, and 0 CSP violations with the production headers.
- Test with the main script blocked: the content and the main links must still
  work.
- Test in reduced-motion mode (`motion.md`).
- Run axe and a keyboard walk (`accessibility.md`).
- Run Lighthouse (mobile) on the home page, a content page and the booking page,
  against the production build (`performance.md`).

## Test environment traps

- Browser tabs driven by an extension may report `visibilityState: hidden`, so
  View Transitions never run, `IntersectionObserver` callbacks never fire, and
  CSS transitions stall halfway. Before calling something a site bug, force the
  end state or check in headless Chrome over CDP.
- The automation window may refuse to shrink to phone width. Use a page of
  390 px iframes of the site and inspect `contentDocument`, or use Playwright's
  own viewport.
- Screenshots of heavy pages time out in batches. Take them one at a time.
- Wait for entrance animations to finish before screenshots or axe.
- A second local dev server may refuse to start because of a lock file. Stop the
  first one, or use the tool's own flag for a second server.
- Rebuilding while a local Worker or dev server runs can crash it. Stop it,
  build, then start it again.
- Login rate limits apply in local tests too. Start each run from a fresh local
  database.
