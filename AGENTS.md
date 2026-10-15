# Standards for websites and apps

These are standing instructions for every website, web app and mobile app built
for this owner. They apply without being repeated in a prompt. They come from
real projects for an online jewellery store. Every rule is there because the
obvious alternative was tried and rejected by the owner, or broke in production.

A project's own instructions (its `CLAUDE.md` / `AGENTS.md`, `docs/decisions.md`)
and the owner's messages override these. When they do, record the override in
the project's decisions file.

## How to work

- **Decide and continue.** When a choice has a sensible default, pick it, say in
  one line what you chose and why, keep it easy to reverse, and carry on. Ask
  only for what only the owner can supply: accounts, keys, money, approvals and
  facts about the business. The exception is the visual direction of a new site
  or a redesign: offer 3–4 named directions with ASCII mockups in one question.
- **No plan-review gates.** For large work, write a short spec, then build,
  verify and ship. Don't stop for approval of a plan. If the owner answers a
  question with "continue" or "go ahead", take your recommended option and say so.
- **Done means verified.** Run the type check, unit tests and end-to-end tests.
  Look at screenshots at 360–390 px and 1440 px. Check the browser console. A UI
  change also needs its motion (see below). Review the diff for code quality
  with the ponytail and pstack plugins before committing (`workflow.md`).
- **Ship it.** In the owner's own repositories, commit and push to the default
  branch once checks pass, without asking. Never push failing work. One concern
  per commit, with a plain-language subject (`feat(web): …`) and a body that says why.
- **Report plainly.** Say what shipped, with the evidence (test counts, Lighthouse
  numbers, the commit). List what is left for the owner, such as dashboard
  settings, secrets, store review clicks and approvals. Say what you could not
  verify, such as a real device or the live site.
- **Record decisions.** Each owner decision goes into the project's
  `docs/decisions.md` (or its existing equivalent) with the date and the reason.
  Rejections like "no blue" or "no OTP" count too. Read the file before you
  propose anything. Changing a settled decision needs the owner.

## Non-negotiables

### Honesty
- Never publish a price, review, rating, testimonial, count, date or claim that
  nobody verified. No "best in town". Structured data asserts nothing false.
- No placeholder copy ("coming soon", "to be added", lorem ipsum). A missing
  asset is simply absent, not announced.
- Never say the business does something it doesn't. Confirm operational facts
  with the owner: who brings what, which areas are served, hours, what is included.
- Features waiting on the owner ship dark. Prices, analytics IDs, payment IDs and
  reviews sit behind a config value that is `null`, and a `null` value renders
  nothing. Filling the value turns the feature on.
- Each fact lives once in config and is rendered everywhere. Never retype a
  phone number, price or opening hours by hand.

### Motion is part of done
A UI change is finished only when it ships with:
- **Page transitions** between pages or views: cross-document View Transitions
  (`@view-transition { navigation: auto; }`) on a multi-page site, or the
  framework's route transitions in an app.
- **Micro-interactions** on every interactive element. Hover, press and focus,
  and every state change (loading, progress, success, error, open and close),
  answer with short, visible feedback of 150–300 ms.
- **A `prefers-reduced-motion` path** that removes the movement and keeps the
  feedback.

Animate `transform` and `opacity`. Confirm that scrolling and the animations hold
their frame rate before calling the work done.

### Phone first, fast, accessible
- Most visitors use mid-range Android phones on mobile data. Design and test at
  360 px first. Nothing is cut off at 360 px.
- Aim for Lighthouse 100 in accessibility, best practices and SEO, CLS 0, and
  mobile performance of 90 or more. No third-party script without a reason.
- Use real photos only: the owner's own, or licensed ones with credit. No stock
  photos, and no AI images unless the owner asks. When the owner does want AI
  images, make them with Google Gemini, never ChatGPT.

### Secrets
- Secrets live in env files kept out of git or in the host's secret store, never
  in git, logs or chat. Signing keys live outside the repo.
- Never type passwords, API keys or payment details into a web form for the
  owner. The owner does that.
- Public outputs never carry personal data: commits, store listings, this repo.

## Topic files: read before the matching work

The topic files are in `standards/` next to this file (installed at
`~/.claude/ai-standards/standards/`). Each is short. Read the ones that match
the task before you start, not after.

| Before you… | Read |
|---|---|
| start a new project, a large feature or a redesign | `workflow.md` |
| design or restyle any UI | `design.md`, `motion.md`, `accessibility.md` |
| add pages, scripts, images or fonts, or chase speed | `performance.md` |
| write or edit any visible text | `content.md` |
| build pages meant to rank, or touch titles, meta, schema or sitemaps | `seo.md` |
| build a form, a booking flow, an admin or staff screen | `booking-and-forms.md` |
| build a store: listings, product pages, cart or checkout | `ecommerce.md` |
| serve Indian customers: UPI, +91 phones, SMS, Indic languages, ₹ | `india.md` |
| make a site installable, offline-capable or self-updating | `pwa.md` |
| wrap a site as an Android app | `android-app.md` |
| work on a Google Play listing, screenshots or a release | `play-store.md` |
| add analytics or conversion tracking | `analytics.md` |
| write tests or verify UI | `testing.md` |
| handle logins, tokens, cookies, headers or secrets | `security.md` |
| deploy, or change dependencies or hosting | `deploy.md` |
| run shell scripts, browser automation or screenshots | `tooling-traps.md` |

## Keeping these standards current

When the owner gives feedback that would hold for any project, add it to the
right file here, then commit and push. Examples: a rejected pattern, a rule about
copy, a better way to verify. Mention in your report that you did. This repo is
**public**: write the lesson in general terms, with no business names, phone
numbers, account IDs, keys or customer data. Project-specific facts belong in
that project's own docs.
