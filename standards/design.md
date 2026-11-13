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
- When an owner says a site "looks AI-made", ask which parts (photos, copy,
  a motif) before changing anything, and change only those. A full redesign
  made on that comment alone was rejected, and the owner kept the original.
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
- Sentence case everywhere: buttons, labels, eyebrows, status pills, nav. No
  all-caps styling (`uppercase`, `text-transform: uppercase`) and no wide
  caps letter-spacing. The owner rejected tracked all-caps labels. When you
  remove caps, recheck the copy: text typed in Title Case was hidden under
  them. Names of documents, places and brands keep their capitals.

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
- Keep shopping screens compact on a phone, in the density of the big Indian
  shopping apps: about a 16 px page gutter, 24 px between sections, 8–14 px
  between product cards and a 14 px body. A spacious desktop look that keeps
  its 24 px gutters and 16 px text on a phone was rejected as "everything is
  big". Get the density from spacing and type, not from smaller touch targets:
  let neighbouring 44 px targets touch rather than shrink them. Don't repeat a
  control the screen already shows, such as filter chips above a grid that
  already has a sticky Filter bar.
- Prefer native elements: `<details>` for FAQs, `<dialog>` for sheets and
  lightboxes, real tables for timetables and prices.
- Except selects, in an app or admin screen with its own theme and dark mode:
  use one styled select everywhere (shadcn's select, or the project's own
  equivalent), never a native `<select>`. The browser draws a native select's
  list, so it does not follow the page's colours or dark mode, and its box sits
  differently from the fields beside it. Keep the keyboard behaviour (arrows,
  Enter, Escape, type-ahead), and add a test that fails if a native one returns.
- Header: solid white, or a floating glass pill if the owner chose that look.
  Keep the navigation small.

## App screens

The owner held up a small social scheduler as the model of a simple app layout.
What made it simple:

- One shell. On desktop, a fixed sidebar about 240 px wide holds the logo, one
  primary action button at the top, five places (an icon and one or two words
  each) and the account at the bottom. There is no top bar on desktop. Content
  sits in one centred column, at most about 72rem wide, with a 16 px gutter on
  phones and 32 px on desktop.
- On phones, the sidebar becomes a bottom tab bar with the places and the
  primary action as a round button in the middle. The top bar holds only the
  logo and one or two icon buttons.
- Five places, no more. Anything else lives inside one of them or in a dialog,
  not in a second row of tabs or a toolbar of status chips.
- Create and edit in a dialog, never on a new page: a bottom sheet on phones,
  with the form on the left and a live preview on the right. A sticky footer
  puts the destructive action alone at the far left and Save draft and the
  primary action at the right. Closing it returns to the same screen.
- A small, fixed size scale: 32 px small buttons, 36 px nav rows, 40 px fields
  and buttons, 44 px for the main action, 56–64 px bars. Text comes in two
  sizes (14 px body, 12 px meta) and at most four weights.
- Greys for everything. The brand colour marks only the primary action, the
  active place and the icons inside action buttons. Areas are separated by
  1 px borders, with no card shadows.
- Every state of a control is a ready block swapped in place, so nothing jumps.
  An upload is a dashed drop area with one line of help, then a spinner with
  the file name, then a thumbnail with its name, Replace and Remove.
- Helper actions sit as a row of small outlined buttons under the field they
  change, each an icon and a verb ("Improve", "Shorten", "Add hashtags").
- One toast at the bottom confirms an action, above the phone tab bar.
- Theme is light, dark or system, applied before the first paint.
- A status view (agents, jobs, runs) is graphical, not a wall of text. Give each
  item an icon in a circle coloured by its status, with a small status mark.
  Show counts per status at the top that also act as filters. Put what needs
  the owner first, with a count badge. Clamp each text line to one or two lines
  and show the full text on tap. Every status has an icon, a colour and a label,
  never colour alone. An owner rejected a text-first board ("display this in a
  graphical way using icons and status indicators and color codes").

Don't copy its weaker parts: the all-caps field labels, the mix of corner radii,
dark mode built by overriding each utility class, and focus outlines turned off.
The rules above (one radius, sentence case) still hold.

## Imagery

- Use real photos: the owner's own, or licensed ones (for example Wikimedia
  Commons CC, credited on a credits page). No stock, no AI-generated images, and
  no photos taken from Google Maps or from creators, unless the owner allows it
  for a specific case.
- When the owner does allow generated images or film frames, they must be
  hyper-realistic: indistinguishable from a real photograph, with natural skin
  texture, lighting and imperfection. A plain, smooth face that "looks like an AI
  image" was rejected ("we always need hyper realistic images").
- Sample or demo content inside a product, such as a sample shop a merchant
  starts with, may use free-licence photos (for example under the Unsplash
  License), credited in the docs, and sample reviews. It must be marked as a
  sample, erasable in one action, and named on screen with a prompt to erase it
  before going live. Offer the choice first: free-licence photos, drawn
  illustrations, or Gemini images.
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
