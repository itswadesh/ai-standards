# Online stores

Rules for storefronts: category listings, product pages, cart and checkout.
The general SEO, analytics and performance rules still apply (`seo.md`,
`analytics.md`, `performance.md`).

## Listings

- **Pagination is links.** Page numbers and Previous/Next are real
  `<a href="?page=N">` elements that keep the active filter and sort
  parameters, so crawlers can follow them. Client-side navigation sits on top
  as an enhancement. Page 1 drops `?page`, and nothing links past the last real
  page.
- Read the current page from the URL during server rendering, so the server
  HTML marks the right page as current.
- **Canonical on paginated pages** keeps `?page=N` when N is above 1, because
  page 2 lists different products. It still strips sort, filter and tracking
  parameters.
- If the search API stops returning results past some page, don't offer those
  pages, and make sure every product is still reachable through category pages
  and the sitemap.
- **Title, meta description and H1 come from the same record**: the current
  category, not its top-level parent and not the URL slug. Don't prefix "Shop"
  to a name that already starts with it.
- Every listing has an `h1`, including the all-products page that has no slug.
- **Empty and retired categories:** a category with zero products renders
  `noindex`. An unknown slug returns 404. A category the owner retires returns
  404 or 410 and leaves the sitemap.
- Homepage tiles and menus link to indexable category or landing pages, never
  to filter URLs that `robots.txt` blocks.

## Product pages

- **Product JSON-LD is in the first HTML response.** Build it from the page's
  load data. In Svelte 5, `$effect` never runs during server rendering, so
  schema built inside an effect only appears after JavaScript runs; use
  `$derived`. Escape `<` in the JSON so product text can't close the script tag.
- **Variants:** a `ProductGroup` with a `productGroupID` that is the same for
  every size and metal child, never the selected child's SKU. Give each
  `hasVariant` entry its real name, its own SKU, its own URL (for example
  `?variant_id=`), image and offer. Write `variesBy` only for axes the data
  actually carries, using Google's supported values.
- **Availability** follows the same stock logic as the add-to-cart button.
  Don't default everything to `InStock`.
- The schema image list holds images only. Filter out video files.
- Write a `VideoObject` only for a real video with its real upload date, never a
  date generated at render time.
- **Shipping and returns markup is complete and agrees with the page:**
  `addressCountry`, rate, and `handlingTime` plus `transitTime` that add up to
  the delivery window the page shows; `applicableCountry`,
  `returnPolicyCategory`, return method, return fees and the return days. Take
  the values from the store's shipping zones and the policy module, not from
  constants in the schema component.
- **One selected variant drives everything:** price, main image, size, delivery
  estimate, schema SKU, the cart line and the analytics payload.
- A record the API reports as missing (product, vendor, category) returns 404
  from the page's loader, never a 500.

## Policy values

- Return window, delivery window, signature threshold, exclusions and support
  phone numbers live in one module (for example `src/lib/config/policy.ts`).
  Meta descriptions, trust lines, the FAQ, policy pages and the schema all read
  from it.
- When the code holds two different values for the same policy, don't pick one.
  Put one value in the module, mark it pending and ask the owner.
- Policy pages kept in a CMS must state the same values as the code. List each
  mismatch for the owner; the agent can't edit CMS content safely.
- `grep -rnE "[0-9]+-[Dd]ay|[0-9]+ days" src` should match only the policy
  module and unrelated text.

## Cart and checkout

- **Add to cart succeeds only when the cart accepts the line.** Some cart
  libraries catch the error, show a toast and still resolve `true`. Check the
  cart state as well, and only then show the confirmation, send `add_to_cart`
  and navigate.
- Send analytics before a navigation that can unload the page.
- The analytics payload is the selected child: its SKU as `item_id`, the price
  shown, the quantity and the currency.
- **Payment SDKs load on the payment step only.** No preconnect or preload in
  the app shell. Some SDK packages inject their script as an import side effect
  (`@stripe/stripe-js` does); import the side-effect-free entry
  (`@stripe/stripe-js/pure`), or alias it in the bundler when a dependency
  imports the default entry.
- `purchase` is sent once per order. `sessionStorage` is per tab, so reopening
  the confirmation page in a new tab sends it again; key a `localStorage` flag
  by the order number, or flag it on the server.

## Shared storefront packages

- When sites consume a shared package from npm, a package fix reaches a site
  only after a release. Fix each site in its own files, never against an
  unreleased package API, and list the site workarounds that the release will
  let you remove.
- Publishing a package version needs the owner's go-ahead. Changes in
  behaviour (for example a failed add no longer showing "Added to cart") go in
  the changelog.

## Checks

```bash
curl -s "$SITE/products" | grep -o 'href="[^"]*page=2[^"]*"' | head -1        # a page-2 link
curl -s "$SITE/products?page=2" | grep -o '<link rel="canonical"[^>]*>'       # ends in ?page=2
curl -s "$SITE/<empty-category>" | grep -c 'name="robots" content="noindex'   # 1
curl -s "$SITE/products/<slug>" | grep -oE '"@type": ?"(Product|ProductGroup)"' | head -1
curl -s "$SITE/" | grep -c 'js.stripe.com'                                    # 0
curl -s "$SITE/" | grep -o 'href="[^"]*attributes\.[^"]*"' | head             # nothing
```
