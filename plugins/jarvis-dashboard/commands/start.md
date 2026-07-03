---
description: Start the Jarvis Dashboard in dev mode (sandboxed + seeded with demo data) or live mode (tracks your real Claude Code sessions)
argument-hint: [dev|live]
---

Start the Jarvis Dashboard. Requested mode: $ARGUMENTS

**Determine the mode** (case-insensitive, trim whitespace):
- `dev` → sandboxed mode.
- `live` → live mode.
- Anything else (including empty) → ask the user to choose between `dev` and `live` before doing anything else. Briefly explain: **dev** is sandboxed and pre-populated with demo sessions/agents — including ones shown as currently active — so there's something to look at immediately without touching real data. **live** tracks the user's actual running Claude Code sessions on this machine and writes hook entries into their real `~/.claude/settings.json`.

**Then, regardless of mode:**

1. **Find/install the checkout.** Check `$JARVIS_DASHBOARD_HOME` (if set), then `~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins/app` (the persistent plugin data directory). If missing, clone and install automatically — don't ask, unless the clone itself fails:
   ```
   mkdir -p ~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins
   git clone --branch jarvis-reskin https://github.com/MrZarrar/Jarvis-Sub-Agent-Dashboard.git ~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins/app
   cd ~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins/app && npm install
   cd client && npm install
   ```
   If a checkout already exists but `node_modules` is missing (root and/or `client/`) or `package.json` changed, run `npm install` in the stale location(s) first.

2. **Pick the `CLAUDE_HOME` for this run**, based on mode:
   - `dev` → `~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins/dev-sandbox`. This is a stable path (not a one-off temp dir) so demo data persists across runs instead of resetting every time. The app derives its database, hooks, and settings entirely from this directory — nothing under it touches the user's real `~/.claude`.
   - `live` → don't set `CLAUDE_HOME` at all (falls back to the real `~/.claude`). This is what makes the dashboard register hooks in the user's actual `~/.claude/settings.json` and track their real sessions.

3. **If mode is `dev`**, seed demo data before starting the server, from inside the checkout:
   ```
   CLAUDE_HOME=~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins/dev-sandbox node scripts/seed.js
   ```
   This is additive and idempotent (safe to re-run — it no-ops if the fixtures already exist) and inserts two demo sessions, one a 9-agent depth-4 tree, both with `status: "active"` so the dashboard looks lively immediately. Don't pass `--full` — that adds unbounded random data on every re-run.

4. **Start the dev server** in that directory, in the background, with the mode's `CLAUDE_HOME` (or none, for live) set, redirecting output to a log file (e.g. `/tmp/jarvis-dashboard-dev.log`):
   ```
   # dev mode
   CLAUDE_HOME=~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins/dev-sandbox npm run dev > /tmp/jarvis-dashboard-dev.log 2>&1 &
   # live mode
   npm run dev > /tmp/jarvis-dashboard-dev.log 2>&1 &
   ```

5. **Get the real client URL, don't assume the default port.** Poll the log for a few seconds until Vite prints its ready line (`➜  Local:   http://localhost:XXXX/`) — both the server (default 4820) and client (default 5173) ports auto-shift if busy.

6. **Open it in the browser automatically**, best-effort, using whichever opener exists for the OS: `open <url>` (macOS), `xdg-open <url>` (Linux), or `start <url>` (Windows, via `cmd /c start`). Don't treat a failure here as fatal — headless/remote environments won't have a browser to open.

7. **Always** show the user a clickable Markdown link to the URL in chat, regardless of whether the auto-open worked. Also tell them which mode is running: for `live`, remind them it just wrote hook entries into their real `~/.claude/settings.json`; for `dev`, mention the demo data is sandboxed under `dev-sandbox` and won't affect their real Claude Code sessions.
