# Content and copy

## Voice

- Write precise, short, human copy, the way the owner would explain it on the
  phone. Use plain words and concrete facts: price, duration, what is included,
  how payment works.
- No "Welcome to…" opener, no long About Us, and no adjectives standing in for
  facts ("premium", "world-class", "best").
- Answer first. The first sentence of a page or section answers the question the
  visitor came with.
- Write button labels as verbs that say what happens: "Book a ring", "Pay ₹300
  with UPI", "Copy UPI ID". Avoid "Submit" and "Click here".
- Error messages say what went wrong and what to do: "This number has 10 digits;
  check the last few". Never "Something went wrong" alone.

## Truth

- Never invent prices, reviews, ratings, testimonials, customer counts, years in
  business, team members or awards. Leave the space empty until real material
  arrives.
- An unverified number is worse than none. Benchmarking competitors' prices is
  not knowing your own: explain how the price is worked out until the owner gives
  real rates.
- No placeholder wording ("coming soon", "to be added", "indicative"). A missing
  photo or section is simply absent.
- Never promise a service the business doesn't give. Confirm every operational
  claim with the owner: who brings water and power, which areas are served,
  whether the crew calls first, how cancellation works. Copy in the FAQ, privacy
  page, SMS and app listing must match what the product actually does and what
  the form actually collects.
- Data you gathered from sources (timetables, distances, opening hours) carries
  the date it was checked and its source, kept next to the data, with a visible
  "Last checked" line where it matters.
- Take distances and drive times from Google Maps driving directions (the
  default, fastest route). Record the from/to place names and the date checked.
- When sources disagree on a figure, publish one figure, said plainly: the
  authoritative source if there is one (Google Maps for distances), otherwise
  the highest. Never print two values, a range of estimates, a list of what
  different sites say, or "figures vary". If nothing is reliable at all
  (handwriting, one doubtful page), hold that entry back and note why.
- Never give the reader homework: no "check locally", "confirm before you go"
  or "check with the railway". Link the site's own page that answers it, or
  leave the fact out and ask the owner. Don't announce gaps ("not confirmed",
  "unknown", "we could not find").
- Before publishing facts about places, history or public services, check them
  against an authoritative source.

## Formats

- Times use the 12-hour clock with AM/PM wherever the audience reads it that way
  (India: yes). Store `HH:MM`, convert when rendering, and cover every output:
  pages, JSON-LD, SMS, WhatsApp text and client scripts.
- Money: `₹1,500`, not `Rs. 1500` or `INR 1500.00`, formatted with
  `Intl.NumberFormat('en-IN')`. Show prices per what they cover ("₹300 for any
  ring", never a necklace price shown as a earrings price).
- Phone numbers: show `+91 98765 43210`, link `tel:+919876543210`, and store
  E.164.
- Dates: "4 September 2026" in prose; relative words ("today", "tomorrow") in
  booking flows.

## Structure

- Keep facts in one place (site config, data files) and render them everywhere.
  A phone number change should be one line.
- Don't repeat whole sections (booking terms, vehicle details, cancellation) in
  every guide. Link to the one page that has them.
- Pages written for search must still be real, distinct content (`seo.md`).
- Translations come from one master dictionary. Components hold no hard-coded
  English. Translations made by a model need a native speaker's review before you
  call them final. Say so in the report.
- If a translated or recorded asset depends on the text (audio narration, for
  example), changing the text means regenerating it. Otherwise hide the feature
  for that page; never fall back to a worse version. The owner has rejected
  robotic device voices.
