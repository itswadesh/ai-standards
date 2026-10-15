# Analytics and conversion tracking

- Analytics is off until the owner gives an ID. The config value is `null` and
  nothing loads. Before turning it on, add the tool to the privacy page in every
  language.
- Load nothing on staff, admin or noindex app pages.
- Never put personal data (phone, address, name) into events or URLs.
- Conversion events fire on the confirmed outcome: after the server accepts
  the form or the cart accepts the item. Never on page load, and never on the
  attempt.
- Send the event before a navigation that can unload the page.

## GA4 sales funnel

Track the sale, not just page views:

| Moment | Event |
|---|---|
| Booking flow opened | `begin_checkout` |
| Booking request sent | `generate_lead` |
| Payment info given (UTR sent, UPI ID copied, QR saved) | `add_payment_info` |
| Staff mark the payment received | `purchase` (`transaction_id` = booking ref, `value` in INR) |
| Refund | `refund` |

- Store the customer's GA client ID and session ID (`_ga`) with the booking, so
  the server can send `purchase` and `refund` through the Measurement Protocol,
  exactly once each. That needs an API secret the owner creates in GA4 (Admin >
  Data streams > Measurement Protocol API secrets) and stores in the host's
  secret store.
- Until that secret exists, the customer's booking page can send `purchase` once
  (guarded by a `localStorage` key per booking).
  Not `sessionStorage`: it is per tab, so a second tab sends it again.
- Never run funnel tests against production. They create real bookings and send
  real texts.

## Consent

- Google Consent Mode defaults to `denied` in regions that require opt-in (EEA,
  UK, Switzerland) using the `region` parameter, and `granted` elsewhere.
  Never default every region to `granted`.
- Re-apply a saved banner choice inline, before any tag loads, and update
  consent when the visitor chooses. With no banner at all, report it to the
  owner rather than inventing one.

## Search data

- Search Console is the source of keyword truth (`seo.md`). Read queries,
  pages, countries and devices before you rewrite copy.
- Microsoft Clarity or similar session tools only with the owner's approval and
  a privacy page update. Skip them on noindex pages.
