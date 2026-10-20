# Tooling traps (for AI agents)

Traps that cost real time on the owner's Windows machine and in browser
automation. Check this file before you script anything non-trivial.

## Shell

- Don't put Node or Python scripts in heredocs or `node -e "…"`. Backticks run
  as commands, `\n` becomes a real newline, and regex backslashes get eaten.
  Write the script to a file in the scratchpad and run `node file.mjs`. For regex
  in generated code, use `String.raw` or the Edit tool.
- Chain with `&&` all the way. `a && b; git commit` commits even when `a` fails.
- `cmd | grep …` hides the exit code of `cmd`. Capture `$?` or use `set -o
  pipefail`.
- Line endings:
  - Repos may mix CRLF and LF (`core.autocrlf=true`).
  - Patch scripts should normalise to LF, match, then restore the file's own
    endings.
  - Python on Windows writes CRLF unless you pass `newline=''`.
  - In Git Bash, `grep -c $'\r$'` can report 0 on a CRLF file. Check the
    bytes (`od -c file | grep -c '\\r'`) before deciding a file's style.
- PowerShell 5.1 has no `&&`, `||`, ternary or `??`. Use the Bash tool for
  POSIX scripts.
- Long-running servers survive a stopped task. Kill the process by its PID, and
  check the port is free before restarting.
- Docker Desktop is available here: use it for tools the machine lacks (Linux
  npm, keytool, Python, CLIs).

## Browser automation

- The extension's tab can be hidden or throttled (see `testing.md`). Prefer
  headless Chrome or Playwright for anything timing-related.
- Headless Chromium draws WebGL in software, and one frame can hold the page's
  thread for hundreds of milliseconds. Timing background work by its round trip
  from the page then measures the renderer. Time the work where it runs, such
  as inside the Web Worker.
- DevTools CPU throttling (`Emulation.setCPUThrottlingRate`) slows the page's
  thread but not Web Workers. To approximate a slower phone, run the code on the
  page's thread while throttled, and report it as an approximation.
- Before you call a missing animation or a black image a site bug, force the end
  state and check again.
- A form field may miss the first typed input. Focus it with JavaScript first,
  then read the value back with JavaScript before submitting.
- A coordinate click can miss in custom listboxes. Click the element's reference
  instead. Re-find references after any list re-renders.
- Fetching `127.0.0.1` from a public page can hang on Chrome's local-network
  permission prompt. Move data by navigating to a local page with it in the URL
  hash.
- The browser extension may run on the owner's machine, not the agent's, so it
  can't open the agent's `localhost` dev server. Use headless Playwright for
  local pages.
- Never trigger `alert`, `confirm` or `prompt` dialogs in the automated tab; they
  block the session.
- Auth on some portals is per tab. The owner may need to sign in inside the
  agent's own tab.

## Builds

- A tool's dev server may drop query strings on prerendered pages. Preview pages
  that read `?lang=` need to opt out of prerendering, and must be deleted before
  the build.
- CSS minifiers can drop one of a prefixed and unprefixed pair. Put the
  unprefixed property (`backdrop-filter`) last and check the output.
- `:is(#id)` takes ID specificity. Overrides of it need more weight.
- An ancestor with `transform`, `filter` or `will-change: transform` becomes
  the containing block for its `position: fixed` children. A modal backdrop
  inside a condensing header then covers only the header. Portal overlays to
  `<body>`, keeping the `{#if}` inside the portal so the close transition
  still plays.
- Compare a new build against a clean build of the same commit, not the
  committed output. Chunk names drift with the toolchain.
