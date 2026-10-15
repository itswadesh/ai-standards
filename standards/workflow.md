# Workflow

## Starting work in a project

- Read the project's history before you propose anything: `CLAUDE.md`,
  `AGENTS.md`, `docs/decisions.md`, specs, `README.md` and `git log`. The
  options the owner rejected matter as much as the ones they chose.
- For anything bigger than a day's work, write a short spec, for example
  `docs/specs/<date>-<topic>.md`. Cover the intent, the architecture, the data
  model, the routes, what is out of scope and the owner's to-dos. Keep it current.
- Keep a decisions log in `docs/decisions.md`: numbered, dated sections, each
  with the owner's words when there are any, what changed, and why.
- The README covers how to run the project locally, how to run the tests, how to
  deploy, the owner's launch checklist, and how the key parts work.

## Stack defaults (when the project has none)

Choose fast, boring tools the owner can host cheaply. These have worked well:

- **Content and marketing site:** Astro with static output, plain CSS tokens or
  Tailwind. Host it on Cloudflare (Workers with static assets; D1 and KV when it
  needs a little data).
- **Booking or business app:** SvelteKit (adapter-node) for the web, a Go API
  (stdlib `net/http`, pgx) holding every business rule, and Postgres. Deploy
  with Docker Compose behind a reverse proxy (Dokploy, Traefik or Caddy).
- **Android app:** a Capacitor wrapper that loads the live site (`android-app.md`).
- Self-hosted fonts and inline SVG icons. No UI kit or component library unless
  the owner asks.

## Large UI work: new sites and redesigns

This sequence produced the best-received result:

1. **Read the history** of rejected looks, fonts and palettes.
2. **Offer 3–4 named directions** as ASCII mockups in one question. Each must
   respect the past rejections. Ask about stack and scope in the same set.
3. **Reach parity before you redesign.** When you change the stack, first rebuild
   the existing site exactly and diff the old and new output: titles, meta,
   canonical, hreflang, JSON-LD, headings, body text and links. Push that change
   on its own, then start the redesign.
4. **Research in parallel:**
   - pattern catalogues such as 21st.dev; rebuild the patterns natively rather
     than pasting them in;
   - a UI/UX rulebook, filtered through the owner's rules, with a written list of
     what you adopted and what the owner's decisions overrule;
   - a baseline of Lighthouse, axe and screenshots;
   - an inventory of everything the old JavaScript does.
5. **Write one design-system document** before building any section. It holds the
   hard rules, the tokens, each section's layout and DOM contract, the pattern
   map, and the accessibility and performance rules.
6. **Build the foundation yourself:** tokens, layout, header, footer, primitives.
   Screenshot it. Then build the sections on separate files (in parallel if you
   use subagents). After each section, an independent critic screenshots it at
   1440 px and 390 px (plus one non-Latin language if the site has one), judges
   it like an art director, and fixes what it finds.
7. **Verify after assembly, reporting only.** Measure performance against the
   baseline. Check accessibility with axe, a keyboard walk, and contrast measured
   over real photo pixels. Test behaviour with the CSP enforced and with the main
   script blocked. Do an art-director pass, and have an adversary check the
   owner's rules. Then fix the findings and verify again until clean.
8. **Ship with evidence** (`deploy.md`).

Each layer catches something the one before missed: clipped Indic text, contrast
over photos, a library ignoring a motion rule, a page that went blank while its
reveals waited.

## Owner feedback

- The owner steers with quick reactions such as "I hate blue", "looks like
  blood" or "sounds very AI". Treat each one as a standing rule: record it, and
  never bring the rejected thing back anywhere in the project.
- If the owner pastes findings from someone else (an audit, a review, another
  AI) with no instruction of their own, ask which ones to act on.
- If the owner reports something "missing" or "broken", reproduce it in their
  conditions first: their device class, their reduced-motion setting, the
  installed app rather than the browser.

## Code hygiene

- No mock data, invented copy or decorative filler in source.
- Delete markup you replace instead of commenting it out. Comments explain
  the current code; never leave ones that narrate what was removed ("X is
  gone", "used to").

## Code quality review

Before committing code, review the diff with two Claude Code plugins, then
apply the findings or say in the report why one was left:

- **ponytail** (`DietrichGebert/ponytail`): `/ponytail-review` lists what to
  delete or simplify (reinvented standard library, needless dependencies,
  speculative abstractions). Use `/ponytail-audit` for a whole repo.
- **pstack** (`michael-denyer/pstack-claude`): `/deslop` removes AI slop from
  the diff (needless comments, defensive checks, casts, deep nesting).
  `/blast-radius` finds what a change could break outside the diff.

Install with `/plugin marketplace add <repo>`, then `/plugin install
ponytail@ponytail` and `/plugin install pstack@pstack-claude`. Leave pstack's
thermo-nuclear reviews off: the owner removed them.

## Changes across many repos

- Finish and verify one repo before fanning out to the rest; the owner may
  pause the others to save cost, and the first repo shows what the brief got
  wrong.
- Give parallel agents one shared brief with the git rules, the ports each may
  use and a report format, and review their diffs before reporting.

## Questions worth asking

Ask about accounts, keys, money, approvals, and business facts such as prices,
hours, areas and who provides what. Ask before anything goes public under the
owner's name, such as a store submission or a post. Ask about the visual
direction of a redesign. Decide everything else yourself.

## Git

- Commit messages: `type(scope): plain-language subject`.
  - Types: feat, fix, perf, content, test, chore, docs, refactor.
  - Scopes: web, api, app, seo, store, admin, and so on.
  - The body explains why, and gives before/after numbers for performance work.
- Push to the default branch once the checks pass. Use pull requests only if the
  project already uses them.
- Add files by name and check `git status` first. Never commit secrets, signing
  keys, `.env` files, or personal photos and documents the owner dropped into the
  repo for you to look at.
- Chain shell steps with `&&` all the way to the commit, so that a failed step
  stops the commit.
- Mark a temporary diagnostic as TEMPORARY in its commit subject, and remove it
  in a later commit that says so.
