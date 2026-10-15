# SEO, local search and AI answers

## Every public page

- A unique `<title>` (main keyword plus place, about 60 characters), a meta
  description (about 150 characters, written as an answer), a canonical URL,
  and Open Graph and Twitter tags with a 1200×630 share image.
- One `h1`, headings in order, descriptive link text, and semantic HTML.
- Server-rendered or static HTML. Content is never injected only by client
  JavaScript. Tabs and accordions keep every panel in the HTML (`<details>`, or
  panels hidden by CSS).
- App routes (booking, account, staff, admin, payment, booking links) are
  `noindex`, sent as both the meta tag and an `X-Robots-Tag` header. They are
  disallowed in `robots.txt` and left out of the sitemap.
- Structured data and content count only if they are in the first HTML
  response. Check with `curl`, not in the browser.
- Status codes are honest. An unknown slug, or a record the API says doesn't
  exist, returns 404, never 500 or an empty 200. A route file with no page
  behind it is deleted.
- Never link internally to URLs that `robots.txt` blocks; link to the indexable
  page instead.

## Structured data (JSON-LD)

- `LocalBusiness`, or the closest subtype (`AutoWash`, `TravelAgency`…), with
  name, telephone, url, address or `areaServed`, `openingHoursSpecification`,
  `priceRange`, and `hasOfferCatalog` holding the real services and prices.
- Per page, as it fits: `Service` with `Offer` (INR), `FAQPage`,
  `BreadcrumbList` on every page below home, and `BlogPosting`.
- `AggregateRating` and `Review` only when real, approved reviews exist. Never
  fabricate them. A `PriceSpecification` with a currency and no price claims
  nothing false. Use that, not an invented price.
- Generate JSON-LD from the same config the page renders, and parse every block
  after each build.
- Keep the name, address and phone (NAP) identical across the site, the schema,
  Google Business Profile and the app listing.

## Sitemap and robots

- Build the sitemap from the real routes and data. `<lastmod>` must not lie:
  store a content hash per page and move the date only when that page's own
  source changes. The same date can feed a visible "Updated" line.
- A route that proxies a sitemap generated elsewhere sets its own
  `Cache-Control` (for example `public, max-age=3600`) and `no-store` on errors,
  instead of forwarding the upstream header, which can be a year long.
- An optional XSL stylesheet makes the sitemap readable for people who open it.
  Crawlers still read the XML.
- `robots.txt` allows search and AI crawlers (Googlebot, Bingbot, GPTBot,
  OAI-SearchBot, ClaudeBot, PerplexityBot, Google-Extended) and disallows app
  routes.
- `llms.txt`: a plain summary of the business, services, prices, areas and key
  links. It is optional, and Google ignores it.

## Pages that rank

- Use the searchers' own words. Read Search Console queries and copy their
  phrasing ("earrings for women" rather than "ear piercing jewelry", and
  local spellings of place names).
- One page per real intent: a service, an area, a route, a question. Each page
  has distinct, useful content. No doorway pages built by swapping the city
  name.
- Area or location pages need their own intro and facts. A thin one is
  `noindex` and stays out of the sitemap until it has real content.
- Link services, areas, pricing and the FAQ to each other.
- Distances, timetables and prices carry the date they were checked
  (`content.md`).

## Writing for AI answers (GEO)

- Open each page with a one-sentence answer.
- Use question-style `h2`s, each followed by a self-contained passage of up to
  three sentences that still makes sense when quoted alone.
- State concrete facts in text, not only in images: price per ring, duration,
  what is included, how to pay, and "no sign-up needed".
- Being cited in AI Overviews brings impressions without clicks. That is
  expected, not a bug.

## Dead links

Crawl every internal link of each page type before shipping: no 5xx, and no
404s from the header, footer or menus. A link to a page that doesn't exist is
repointed to the page that covers it or removed; the owner decides which,
since it may be a page they plan to write.

## Redesigns and migrations

The SEO surface is frozen during a redesign. Titles, meta, canonicals,
hreflang, JSON-LD, headings, body text and internal links stay the same unless
the change is deliberate. Diff the old and new build with a script. Content may
move and be restyled, but it must not disappear from the HTML.

## Multilingual sites

- Give each language its own path prefix (`/hi/`, `/or/`) and add `hreflang`
  alternates, including `x-default`.
- Every link on a translated page stays in that language. An untranslated page
  must not quietly fall back to English. Either translate it or leave it out of
  that language.
- A structure checker compares each translation with the master (keys, array
  lengths, placeholders, links, times). It can't judge meaning, so a native
  speaker still reviews the copy.

## After launch (owner's jobs; list them in the report)

Verify Search Console and submit the sitemap. Set up the Google Business Profile
with the same NAP, and request reviews from real customers, never in exchange
for a discount. Use Bing Webmaster Tools.
