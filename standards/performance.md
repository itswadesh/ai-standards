# Performance

## Targets

- **Lighthouse:** 100 for accessibility, best practices and SEO on every page;
  desktop performance 100; mobile performance 90 or more.
- **CLS:** 0 on every page.
- **Budgets for a marketing page:** under 50 KB of JavaScript gzipped (zero is
  better), under 20 KB of CSS gzipped, and no third-party scripts unless there is
  a reason.

## How

- **Ship zero JavaScript where possible.** Marketing pages work as plain HTML.
  Small progressive scripts enhance links that already work without them (for
  example a "Share" link that is a `wa.me` link until the script takes over).
- **Island hydration:** load heavy widgets only when they scroll into view, and
  only on the pages that use them. A dynamic import with side effects at import
  time can pull a framework chunk onto every page. Keep entry modules free of
  side effects.
- **Third-party loaders** (chat, email-marketing, review widgets) load on the
  first interaction or after consent, not at page load. Payment SDKs load on
  the payment step only. Watch for packages that inject a script when merely
  imported; use their side-effect-free entry (`ecommerce.md`).
- **The LCP image** is preloaded with `fetchpriority="high"`, and it is the only
  image with priority.
- **Reserve every box.** Give images `width` and `height` or `aspect-ratio`.
  Give embeds a fixed height. Fonts get a size-adjusted fallback.
- **Fonts:** one or two self-hosted woff2 subsets, the display face preloaded,
  `font-display: swap` (`design.md` covers Indic fonts).
- **Images:** AVIF or WebP at 480, 960 and 1600 widths through `srcset`, with a
  JPEG fallback when needed. Resize with sharp in a script. Never serve the
  camera original.
- When the LCP image is preloaded and has a `srcset`, give the preload a
  matching `imagesrcset` and `imagesizes`, or the phone downloads it twice.
- **Caching:** hashed assets get `Cache-Control: public, max-age=31536000,
  immutable`. Public HTML gets a short max-age with `stale-while-revalidate`.
- **`content-visibility: auto`** helps long pages. Give it
  `contain-intrinsic-height`, not `contain-intrinsic-size`, which reserves the
  width too and can widen a phone column. Re-check anchor links afterwards,
  because skipped content can break scrolling to a `#hash`.
- **Lazy images inside cards** can re-lay out the card. Contain the card
  (`contain: strict`, with the image absolutely positioned) or reserve its size.
- **Compositor cost:** see `motion.md`. Blur, blend modes and filters cost time
  on every scroll frame.

## Measuring

- Measure before and after every performance change, and put the numbers in the
  commit message.
- A busy machine gives noisy Lighthouse scores. Alternate baseline and new runs in
  the same batch, and compare medians.
- Test with the production build, never the dev server.
- Find the cause before fixing. Take a performance trace, then remove one suspect
  at a time (`motion.md`).
- Pages in Indic scripts carry bigger fonts. Measure them separately; they are
  usually the slowest pages.
