# Forms, booking flows and staff screens

## Ask for as little as possible

- Every field must earn its place. The owner removed name, area picker,
  landmark, registration number from a
  booking form, and booking got faster each time. Don't add fields back without
  asking.
- **No OTP by default.** A call from the business before the visit verifies the
  customer, and an OTP costs money and loses customers on the way.
- **No sign-up.** Customers book without an account.
- Ask one thing at a time, with tap-only choices first (service, date and slot),
  in the style of Typeform. A question with only one answer is skipped.
- Typed fields come last: phone, address, pincode. Remember them on the device,
  with a visible "Clear" link.
- Use the right `inputmode`, `autocomplete` and `type`. Validate as the customer
  types, but only show an error after they leave the field or press the button.
- Inputs use a font size of at least 16 px, or iOS zooms the page on focus.
- A refresh never loses input. Keep the flow's state in the URL plus
  `sessionStorage`.
- Use big tap targets, with a sticky bottom bar showing the price and the next
  step.

## Identity without logins

- **Booking links:** each booking gets a random 128-bit token, and its page
  `/b/<token>` shows the status and offers cancel, "I've paid" and review. Show
  the link on the confirmation screen with Copy and "Send to my WhatsApp".
  Booking pages send `Referrer-Policy: no-referrer` and are `noindex`. A short
  human code (`DS-7K3QX`) is for phone calls only and grants no access.
- **Device memory:** an httpOnly cookie holding a random token, stored hashed,
  lists the bookings made on that device and fills in the last details.
- **Never use a phone number as an access key.** Anyone could type someone
  else's number and see their address. Mask phone numbers on shared pages
  (`98•••••210`).
- Put tokens in the URL fragment (`#`) when the server doesn't need them, so
  they never reach logs or analytics.

## Abuse protection

Fake bookings steal capacity. Add:

- a hidden honeypot field, where a filled-in submission gets a fake success;
- rate limits per IP, per hour and per day;
  from this device, each with a Cancel button;
- an admin switch to block a number ("Please call us to book");
- server-side validation of everything.

## Capacity and time

- Enforce slot capacity in the database with a lock or an atomic check, and test
  it with concurrent inserts.
- Booking closes a set number of minutes before a slot (lead time), stored as
  configuration.
- Work in the business's time zone (IST) on the server, and test around
  midnight.

## After booking

- The confirmation says exactly what happens next ("Our crew will call you on
  98•••••210 before arriving") and how payment works.
- The booking page polls for status only while it is visible (`visibilitychange`).
- Notify the owner and the crew of each new booking (SMS or WhatsApp,
  `india.md`), and the customer when the status changes.
- Cancellation and date changes follow the owner's terms, shown before booking
  and enforced by the server.

## Owner and staff screens

- **Admin:**
  - a bookings board with crew assignment;
  - services and prices (switch a service off rather than delete it; its page
    redirects to the services still on offer);
  - areas, team logins with password reset, customers, reviews and settings.
- **Crew app:**
  - a job list;
  - Call, WhatsApp (pre-filled with the booking link) and Navigate buttons;
  - "Confirmed on call", status steps, a full-screen UPI QR with the amount,
    "Payment received", and cancel with a reason.
- Staff log in with a password (argon2id) and get rate-limited sessions
  (`security.md`). Staff pages load no analytics and are `noindex`.
- Keep the public site's contact bar or floating buttons off admin pages; they
  cover save bars on phones.
- Changes made in admin reach the public pages at once (purge any cache).
