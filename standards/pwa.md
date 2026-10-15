# Installable, offline and self-updating sites

## Manifest

- Include `name`, `short_name` (12 characters or fewer, so it fits under the
  icon), `display: standalone`, `start_url: /?source=pwa`, `scope: /`, and
  theme and background colours that match the header.
- Icons: 192 and 512 px, plus a maskable 512 with the logo inside the safe zone.
  Generate them from one source with a script.
- Add `shortcuts` for the main actions (Book, My bookings) and `screenshots` for
  the richer install sheet.

## Service worker

- Precache the built assets and an `/offline` page.
- Navigations go to the network first and fall back to the offline page.
- Never cache API responses, personal pages or admin pages.
- The offline page has a Try again button and retries on its own when the
  connection returns.

## Updates: only on return to the foreground

The owner's rule: **check for a new version only when the app or tab comes back
to the foreground.** Never poll while it is on screen, and never reload it
silently in the background. Both of those were tried and rejected.

- The build writes `/version.json`, served with `Cache-Control: no-store`.
- The page reads it once after loading, then again on `visibilitychange` to
  visible and on `pageshow` with `persisted`. Inside the Android app, also listen
  for an `app-resume` event fired by the native side.
- If the version changed, reload, with a short toast, before the first tap.
- Never reload a page while the visitor is typing or has unsaved input. Wait for
  the next return.
- CDNs may send weak ETags (`W/"…"`) when compressing and strong ones when not.
  Strip `W/` before comparing.
- Chrome won't activate a waiting worker while the old one keeps receiving
  messages. Don't poll the old worker with `postMessage`; watch the registration
  states instead.

## Testing

- To stand in for a deploy, register the worker under a new URL, or rewrite
  `version.json` under open pages. Then fake `document.hidden` and fire
  `visibilitychange`. Restore the files afterwards.
- Playwright's fake clock running for minutes can crash the tab. Drive the
  events directly instead.
- Check the live site too. Local preview servers often skip the production
  headers (CSP), and the CSP can block the worker or its scripts.
