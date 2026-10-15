# Serving customers in India

## Contact: WhatsApp first

- Every WhatsApp action is a real link (`https://wa.me/91XXXXXXXXXX?text=…`,
  with `target="_blank" rel="noopener noreferrer"`), so it works without
  JavaScript, with a long press and in a new tab. Call (`tel:+91…`) comes second.
- Pre-fill WhatsApp messages with what the business needs: the service, date
  and booking link. Encode them with `encodeURIComponent`.
- To share the site or app, use `navigator.share` where it exists, Capacitor's
  Share plugin inside the Android app, and otherwise a panel with WhatsApp and
  "Copy message".

## Phones and addresses

- Normalise mobile numbers to `+91` plus 10 digits starting with 6–9. Accept
  spaces, `0` and `+91` prefixes as typed. Store them in E.164 format.
- Pincodes are 6 digits. Check that the business serves them and say plainly
  when it doesn't, with a WhatsApp or Call option.
- Address: one free-text field is enough. "Share my location" is an optional
  extra, and its error should say why it failed (permission, GPS off, timeout).

## Payments: UPI

- Payment usually happens after the service, straight to the owner's UPI ID,
  with no payment gateway.
- The `upi://pay?pa=&pn=&am=&cu=INR&tn=&tr=` link needs every parameter
  URL-encoded, and the amount written with two decimal places.
- Many UPI apps refuse intent payments to personal UPI IDs, and iOS has no
  handler. So:
  - recommend a merchant UPI ID (GPay, PhonePe or Paytm for Business);
  - always offer a QR code and a one-tap "Copy UPI ID" next to any pay button;
  - on a page meant for scanning from another phone, offer a downloadable QR
    image ("scan from gallery").
- Draw the QR on the server as SVG. A bank-issued QR is signed (`sign=`), so
  decode it and redraw it unchanged, then check that the redrawn QR decodes to
  the same bytes.
- A plain UPI payment doesn't notify the site. Confirmation is manual: the
  customer may enter the 12-digit UTR, and staff tap "Payment received".
- Payment pages are unlisted: `noindex`, out of the sitemap and menus.

## SMS (DLT)

Transactional SMS in India needs DLT registration: an entity (PE), a header and
approved templates. Rules learned from rejections:

- The PE's (entity's) registered name must appear in the message text, for
  example as a sign-off ("-ENTITY"). A second sender header is not a brand name.
  A template signed with the header's name instead was rejected ("Template
  should have Entity / Trade / Brand Name"), even though the product's name
  appeared in the text.
- A rejected template's content is read-only. To fix one, register a new
  template under a new name (for example with a `v2` suffix).
- Variables are typed (`{#url#}`, `{#alphanumeric#}`; there is no `{#var#}`).
  Keep to one or two. Three or more risks rejection.
- URLs in templates must be whitelisted first. The whitelisted static part must
  end in `?` with no `/` after it, and the template's sample URL must be exactly
  that entry. Design links to fit, for example `https://example.in/b?t=<token>`.
- Template names allow only letters and digits.
- An SMS provider such as MSG91 needs the sender header, the DLT template ID and
  content that matches exactly. Keep template IDs in code, and the auth key in
  the host's secret store.
- The owner signs in to the DLT portal; never type their password.

## Languages

- Common pairs: English plus Hindi, plus the state's language (Odia, Bengali,
  Telugu…).
- Indic fonts: ship whole script blocks (`design.md`).
- JavaScript's `\b` matches ASCII letters only. After Indic words, use
  `(?![\p{L}\p{M}])` with the `u` flag.
- Some runtimes lack Indic locales (workerd has no Odia, for example), so `Intl`
  falls back to English. Keep day and month names in the dictionaries.
- Spoken audio: use recorded neural voices only, never the browser's speech
  synthesis (`content.md`).
- Model translations need a native review before launch. Say so in the report.

## Formats

- ₹ with Indian grouping (`₹1,50,000`) through `Intl.NumberFormat('en-IN', { style: 'currency', currency: 'INR', maximumFractionDigits: 0 })`.
- 12-hour times with AM/PM. Store times in IST (`Asia/Kolkata`) and say "IST"
  when it's ambiguous.
- Most visitors use Android phones on mobile data; many are on older WebViews
  or low-end devices. Test at 360 px with CPU throttling.
