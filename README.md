# AI standards for websites and mobile apps

Standing instructions for AI coding agents (Claude Code, Codex, Cursor and
others) that build websites, web apps and Android apps. Install them once and
every new project follows them, with no need to repeat them in each prompt.

They were written from real production work for an online jewellery store. 
Every rule is there because the obvious alternative was tried and rejected by the owner, or broke in production.

## What's inside

| File | Covers |
|---|---|
| [`AGENTS.md`](AGENTS.md) | The core: how to work, non-negotiables, and which topic file to read before which task. This is the file agents load. |
| [`standards/workflow.md`](standards/workflow.md) | Starting a project, decisions log, stack defaults, the redesign process, owner feedback, code quality review, git |
| [`standards/design.md`](standards/design.md) | Direction, colour, type, layout, imagery, brand assets from code |
| [`standards/motion.md`](standards/motion.md) | Page transitions, micro-interactions, reduced motion, frame-rate checks |
| [`standards/accessibility.md`](standards/accessibility.md) | Contrast, focus, semantics, forms, dialogs, checking |
| [`standards/performance.md`](standards/performance.md) | Targets, budgets, images, fonts, measuring |
| [`standards/content.md`](standards/content.md) | Voice, truth, formats, structure |
| [`standards/seo.md`](standards/seo.md) | Meta, JSON-LD, sitemaps, local SEO, AI answers, multilingual |
| [`standards/booking-and-forms.md`](standards/booking-and-forms.md) | Minimal forms, identity without logins, abuse protection, staff screens |
| [`standards/ecommerce.md`](standards/ecommerce.md) | Listings and pagination, product schema, policy values, cart and checkout events, shared packages |
| [`standards/india.md`](standards/india.md) | WhatsApp, +91 phones, UPI, DLT SMS, Indic languages, ₹ |
| [`standards/pwa.md`](standards/pwa.md) | Manifest, service worker, update on return to foreground |
| [`standards/android-app.md`](standards/android-app.md) | Capacitor wrapper of the live site, signing, testing |
| [`standards/play-store.md`](standards/play-store.md) | Listing copy (ASO), screenshots, releases |
| [`standards/analytics.md`](standards/analytics.md) | GA4 sales funnel, privacy |
| [`standards/testing.md`](standards/testing.md) | Unit, integration, e2e, visual checks, environment traps |
| [`standards/security.md`](standards/security.md) | Secrets, auth, headers, privacy |
| [`standards/deploy.md`](standards/deploy.md) | Before and after pushing, hosting patterns, rollback |
| [`standards/tooling-traps.md`](standards/tooling-traps.md) | Shell and browser-automation traps for agents |

`AGENTS.md` is kept short because it loads into every session. The topic files
are read only when a task needs them.

## Install

### Claude Code (every project, automatically)

```bash
git clone https://github.com/itswadesh/ai-standards ~/.claude/ai-standards
```

Then add this line to `~/.claude/CLAUDE.md` (create the file if it doesn't exist):

```markdown
@~/.claude/ai-standards/AGENTS.md
```

Claude Code loads `~/.claude/CLAUDE.md` in every session and follows the `@`
import. Run `/memory` in a session to check that the file is listed.

Update with `git -C ~/.claude/ai-standards pull`.

### Other agents

- **Codex CLI:** clone as above and copy or link `AGENTS.md` to
  `~/.codex/AGENTS.md`.
- **Cursor, Windsurf and others:** paste `AGENTS.md` into the global rules. Keep
  the clone at `~/.claude/ai-standards` so the topic-file paths resolve.
- **One project only:** copy `AGENTS.md` and `standards/` into the project root.

## Precedence

A project's own instructions (its `CLAUDE.md` / `AGENTS.md`,
`docs/decisions.md`) and the owner's messages override these standards. The
agent records any override in the project's decisions file.

## Adding a lesson

When feedback would hold for any project, add it to the matching topic file in
plain, general terms. Never add business names, phone numbers, account IDs, keys
or customer data: this repo is public. Agents using these standards are told to
do the same and push the change.
