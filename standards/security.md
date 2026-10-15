# Security and privacy

## Secrets

- Secrets live in env files kept out of git, or in the host's secret store
  (Dokploy env, Cloudflare secrets, GitHub Actions secrets). Commit only an
  `.env.example` with empty values.
- Never print secrets in output, logs or chat, and never ask the owner to paste
  one into chat. If you need to check a secret is set, probe with a wrong value
  and expect a rejection.
- Signing keys and their passwords live outside every repo.
- On Cloudflare, add variables as type Secret. Plain-text variables can be
  removed by the next `wrangler deploy` unless `keep_vars` is true.
- Never type passwords, API keys, OTPs or payment details into web forms on the
  owner's behalf. The owner signs in and fills them in.

## Auth

- **Staff passwords:** argon2id, at least 8 characters. Limit login attempts per
  account and per IP. Cap parallel argon2 runs, because each one uses a lot of
  memory.
- **Sessions:**
  - a random token of 32 bytes or more, stored hashed (SHA-256);
  - an `httpOnly` cookie, `Secure` in production, `SameSite=Lax`;
  - sliding expiry;
  - deactivating a user deletes their sessions.
- **Customer access** uses booking links and device cookies, never a phone
  number (`booking-and-forms.md`).
- **Rate limits:** reserve the slot atomically, so parallel requests can't slip
  past a limit.

## Requests

- CSRF: `SameSite=Lax` cookies, plus mutation endpoints that accept only
  `Content-Type: application/json` and check the same-origin `Origin` header.
- Validate every input on the server: format, length, enum values. Use SQL
  parameters only.
- Errors show the user a human message. Log the details server-side with a
  request ID (`X-Request-Id`) as structured JSON.
- Customer-facing JSON never includes staff names, internal notes or other
  customers' data.

## Headers

- HSTS, `X-Content-Type-Options: nosniff`, `Referrer-Policy`
  (`no-referrer` on token pages), and a `Permissions-Policy` that allows only
  what's used (for example `geolocation=(self)`).
- CSP with hashes for inline scripts and JSON-LD. No `unsafe-inline` for
  scripts. Allow `data:` or `blob:` images only where a feature needs them.
  Verify on the live site: 0 CSP violations.
- `X-Robots-Tag: noindex` on app, token, payment and admin routes.

## Privacy

- Collect only what the service needs, and say exactly that on the privacy page.
  Keep the privacy page, the FAQ, the store's Data safety form and the actual
  form in agreement.
- Add a data deletion page or path (Google Play requires one for apps).
- Never place personal data in URLs, analytics events or logs.
- Never commit personal files (photos, ID documents, exports) that the owner
  drops into a project folder.
