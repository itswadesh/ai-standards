# Visual design

## Direction

- Make it distinctive, not a template. Before you design, write the mood in
  4–5 words, and also what it is not. Example: "warm, cinematic, precise,
  local; not SaaS, neon, playful or corporate".
- Avoid the generic AI look:
  - purple or blue gradients;
  - glass on everything;
  - emoji as icons;
  - a centred hero over three identical cards;
  - stock photos of smiling people;
  - "Welcome to…" headlines.
- A proven look for a small business is a clean product site:
  - white and very light grey bands, with near-black text;
  - one strong action colour;
  - black bands for feature blocks and the footer.
- No colour gradients on backgrounds or cards unless the owner asks. Use texture
  instead (a subtle weave, grain or contour lines) or flat colour. The exception
  is a dark shade behind text that sits on a photo.
- The owner's reaction decides. Record every rejected palette, font and motif
  (`workflow.md`), and don't offer it again.

## Colour

- Use one action colour, one dark ink, a white or nearly white page, and black
  for heavy bands. Error red stays red, whatever the brand colour is.
- Define all colours as tokens in one CSS file and use only the tokens.
- Check every text and background pair for contrast: 4.5:1 for body text, 3:1
  for large text and UI borders. Check every state, including hover, disabled
  and the hidden state before a reveal. List the pairs that fail in the design doc.
- Blue backgrounds have been rejected before. Don't propose them without a reason.
- Green means on. Give a status the success colour only when the thing is
  switched on and working, and add a tick so the meaning doesn't rest on colour
  alone. Something present by default (built in, included) reads neutral. When
  everything is green, green says nothing.

## Corners

- Use one corner radius for every element: buttons, fields, cards, tables,
  modals, menus, alerts and tags. Define it once as a token and point every
  other radius at it, including a UI kit's own tokens and utility classes. Only
  round things (circle buttons, status dots, counters, switches) differ. A screen
  mixing 5, 8, 13 and 15 px corners and pill-shaped search bars was rejected.

## Type

- Use at most two families. Self-host them as woff2 subsets with
  `font-display: swap`. Preload the display face. Give the fallback font
  size-adjust so the swap causes no layout shift.
- A proven pair is Plus Jakarta Sans (display, 700–800) over Instrument Sans
  (body). Avoid serifs unless the owner asks, because they have read as "old".
- For Indic and other complex scripts, ship the whole script block (Noto Sans
  <Script>). Never subset these fonts to the page's text: vowel signs come
  apart, and any text added later has no glyphs. A rebuilt font needs a new file
  name, because caches and service workers keep the old file.
- Check every font's licence before shipping it. Demo builds of commercial
  fonts often stamp "DEMO" over digits or punctuation, so a heading with a
  price or a comma breaks. Until a licence is confirmed, keep variable text off
  that face.
- Use tabular numerals for prices, times, distances and counts. Keep prose to
  about 65–70 characters a line.

## Layout

- Design for phones first: 360 px, then 390, 768 and 1440 px. At 360 px nothing
  may be cut off. Shrink long names a little and shorten button labels
  ("Book a ring" becomes "Book") rather than truncating.
- The first screen says what the business offers, where, and for how much, and
  shows the main action. Nothing on it waits for an entrance animation.
- On phones, keep the main action within reach: a sticky bottom bar (Book,
  WhatsApp, Call) with safe-area padding, shown once the hero has scrolled past.
- Respect `env(safe-area-inset-*)`. The Android app draws edge to edge, under
  the status bar.
- Touch targets are at least 44 px; primary actions are 48 px or more.
- Prefer native elements: `<details>` for FAQs, `<dialog>` for sheets and
  lightboxes, real tables for timetables and prices.
- Header: solid white, or a floating glass pill if the owner chose that look.
  Keep the navigation small.

## Imagery

- Use real photos: the owner's own, or licensed ones (for example Wikimedia
  Commons CC, credited on a credits page). No stock, no AI-generated images, and
  no photos taken from Google Maps or from creators, unless the owner allows it
  for a specific case.
- When the owner allows AI-generated images, make them with Google Gemini's
  image model, never ChatGPT. They may look like photographs of places,
  vehicles, food or things, but never show a person, and are never evidence
  of results, a before/after or a review.
- Serve WebP or AVIF in several widths with `width` and `height` set. Preload the
  LCP image with `fetchpriority="high"`. Lazy-load everything below the fold.
- Use one inline SVG icon set with a consistent stroke. Decorative icons get
  `aria-hidden="true"`.
- A logo drawn for white backgrounds sits on a white plate on dark backgrounds.
- Hide animated or 3D showpieces on small or slow devices if they cost frames
  (`motion.md`).

## Brand assets from code

Generate icons, the 1200×630 share image, store graphics and store screenshots
with scripts committed to the repo (sharp, Playwright). When the brand changes,
re-run one command; never hand-edit a set of exported PNGs. Make the app icon,
favicon and store icon from a single source image.
