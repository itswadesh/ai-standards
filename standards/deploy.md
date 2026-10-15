# Deploying

## Before pushing

- Run every check locally: type check, unit tests, e2e tests and a build.
- When dependencies change, reproduce the host's build in Docker with the
  host's Node version (for example `docker run node:22`). A lockfile written by
  a different npm or OS can be refused by `npm ci` on the host. On Cloudflare,
  generate the lockfile with Linux npm in Docker.
- Never delete the host's config files (`wrangler.jsonc`, `docker-compose.yml`,
  Dockerfiles) to "simplify". Their absence triggers auto-detection that fails.

## After pushing

- Watch the build: the GitHub check run, or the host's deploy log. If a
  Cloudflare build fails with no readable log and the same commit builds in
  Docker, retry once with an empty commit before digging.
- Load the live site with its real headers and confirm:
  - the new version is served;
  - 0 console errors and 0 CSP violations;
  - the main flow works.
- If deploys are manual (for example Dokploy without a webhook), tell the owner
  exactly what to redeploy. Don't report a change as live until it is.

## Hosting patterns

- **Static or content site:** Cloudflare Workers with static assets. Use
  `assets.not_found_handling: "404-page"` for real 404s, `_headers` for CSP and
  `X-Robots-Tag`, and `run_worker_first` only for the paths the Worker handles.
- **App with an API and a database:** Docker Compose with a reverse proxy in
  front (Traefik, Caddy). The web container proxies `/api` to a private API
  container. The database sits on a private network. Back up nightly with
  `pg_dump`.
- **Migrations:** append-only SQL files, embedded and run at start-up. Add
  columns with guarded `ALTER`s. Never edit a migration that has shipped.
- Public pages cache briefly (`max-age=60, stale-while-revalidate=600`). Purge
  or refresh them immediately when the owner changes data in admin.

## Rollback

- Every deploy is a commit. Roll back with `git revert` and a push. Never
  force-push the default branch.
- Database changes stay backward compatible for one release, so the previous
  build can still run.
