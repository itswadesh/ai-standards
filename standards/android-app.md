# Android app (Capacitor wrapper of the live site)

## Shape

- A Capacitor app whose `server.url` is the live site, in its own folder
  (`mobile/` or `android-app/`) with its own `package.json`, so the site's deploy
  is untouched. A website change reaches the app as soon as it comes to the
  foreground. Build a new version only to change native code, the name or the icon.
- The package ID (`com.example.app`) is permanent once uploaded to Play. Choose
  it carefully.
- Target the latest API level that Play requires. The minimum SDK is 24
  (Android 7) unless the owner needs older phones.

## Must-haves

- **Back button:** goes back a page; on the first page, leave the app with
  `moveTaskToBack`.
- **External links:** `upi://`, `tel:`, `mailto:`, WhatsApp, maps and other
  domains open the phone's own apps.
- **Offline:** a bundled `www/offline.html` with a Try again button that also
  retries on its own. Clear the history once it loads, so Back doesn't step into
  the failed page. Send only 5xx errors and network errors there, not the site's
  own 404 page.
- **Edge to edge:** pages draw under the status bar. The site uses
  `env(safe-area-inset-top)` for its bars, which is zero in a browser.
- **Share:** a WebView has no `navigator.share`. Add `@capacitor/share` and call
  `window.Capacitor?.Plugins?.Share` from the site. A "canceled" error does
  nothing; any other error shows the site's own share panel.
- **Resume event:** the native side dispatches an `app-resume` event, so the
  site's foreground-only update check runs (`pwa.md`).
- **Service worker requests:** set `android.resolveServiceWorkerRequests` to
  false unless a plugin needs it. When it is on, every service-worker request
  goes through Capacitor's Java handler. That made the home screen lag in the
  app while Chrome on the same phone was smooth.
- **Play in-app updates:** use immediate mode (a forced, full-screen update) or
  flexible mode (Restart now or Later), as the owner chooses. These only work for
  installs from Play, so test them through internal app sharing.

## Signing and building

- Keep the upload key outside every repo (for example
  `C:\projects\android-keys\<app>\`), with its passwords in a properties file
  next to it. Tell the owner to back that folder up. Use Play App Signing, so a
  lost upload key can be reset.
- Never print the keystore passwords. CI gets them as encrypted secrets
  (base64 keystore, passwords, alias).
- For each release, raise `versionCode` by one (Play refuses a number it has
  seen) and set `versionName`. Build with `gradlew bundleRelease` and upload the
  `.aab`. Use `assembleRelease` for an APK to sideload on a test phone.
- Web-to-app links (App Links) need `/.well-known/assetlinks.json` with the
  **Play App Signing** SHA-256, not the upload key's.

## Testing

- Without a real device, say so in the report. Emulator notes:
  - API 31 with 2 cores works on standard CI runners; API 34 and 35 images often
    crash there.
  - Old emulator WebViews lack modern CSS; real phones update their WebView.
  - Heavy WebGL overwhelms software rendering. Disable WebGL for smoke tests.
  - Script-only navigations make Chrome skip history entries, so test Back with
    a real user gesture (CDP `userGesture: true`).
- Smoke-test through adb and the WebView DevTools socket. Give every call a
  timeout and reconnect after navigations.

## Google Play

See `play-store.md`. Reviewers need a way in: put any login they need under App
access. The owner types those credentials into Play Console themselves.
