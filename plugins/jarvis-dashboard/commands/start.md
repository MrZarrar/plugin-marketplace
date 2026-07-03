---
description: Start the local Jarvis Dashboard dev server (server on :4820, client on :5173)
---

Start the Jarvis Dashboard. Do this automatically without asking the user for a path — only fall back to asking if every step below fails.

1. **Find the checkout.** Check, in order: `$JARVIS_DASHBOARD_HOME` (if set), then `~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins/app` (the persistent plugin data directory — survives plugin updates, this is where step 2 installs it).
2. **If no checkout exists yet**, install it automatically into the persistent data directory. This is a two-package repo (root + `client/`) — both need their own `npm install`:
   ```
   mkdir -p ~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins
   git clone --branch jarvis-reskin https://github.com/MrZarrar/Jarvis-Sub-Agent-Dashboard.git ~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins/app
   cd ~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins/app && npm install
   cd client && npm install
   ```
   Only ask the user for a different location or credentials if this clone fails (e.g. network/auth error).
3. **If a checkout already exists** but `node_modules` is missing (root and/or `client/`) or `package.json` has changed since the last install, run `npm install` in the missing/stale location(s) before starting.
4. In that directory, run `npm run dev` in the background, redirecting output to a log file (e.g. `/tmp/jarvis-dashboard-dev.log`). This starts the Express/SQLite/WebSocket server (default port 4820, but `scripts/dev.js` auto-picks another port if it's busy) and the Vite client (default port 5173, same auto-pick-another-port behavior).
5. **Get the real client URL, don't assume the default port.** Poll the log for a few seconds until Vite prints its ready line (`➜  Local:   http://localhost:XXXX/`) — that's the browser-facing URL to use, not the server's port. Both ports can shift if something else is already listening on the default.
6. **Open it in the browser automatically**, best-effort, using whichever opener exists for the OS: `open <url>` (macOS), `xdg-open <url>` (Linux), or `start <url>` (Windows, via `cmd /c start`). Don't treat a failure here as fatal — headless/remote environments won't have a browser to open.
7. **Always** show the user a clickable Markdown link to the URL in chat (e.g. `[Open Jarvis Dashboard](http://localhost:5173)`), regardless of whether the auto-open worked — this is the fallback, not an alternative only shown on failure. Also mention: on startup it auto-writes hook entries into `~/.claude/settings.json` so every Claude Code session reports events to it. If they want to develop against a sandbox instead, restart with `CLAUDE_HOME=/tmp/claude-sandbox node server/index.js` instead of `npm run dev`.
