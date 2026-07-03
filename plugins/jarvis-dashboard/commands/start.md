---
description: Start the local Jarvis Dashboard dev server (server on :4820, client on :5173)
---

Start the Jarvis Dashboard. Do this automatically without asking the user for a path — only fall back to asking if every step below fails.

1. **Find the checkout.** Check, in order: `$JARVIS_DASHBOARD_HOME` (if set), then `~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins/app` (the persistent plugin data directory — survives plugin updates, this is where step 2 installs it).
2. **If no checkout exists yet**, install it automatically into the persistent data directory:
   ```
   mkdir -p ~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins
   git clone --branch jarvis-reskin https://github.com/MrZarrar/Jarvis-Sub-Agent-Dashboard.git ~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins/app
   cd ~/.claude/plugins/data/jarvis-dashboard-mushafzarrar-plugins/app && npm install
   ```
   Only ask the user for a different location or credentials if this clone fails (e.g. network/auth error).
3. **If a checkout already exists** but `node_modules` is missing or `package.json` has changed since the last install, run `npm install` in it before starting.
4. In that directory, run `npm run dev` in the background. This starts the Express/SQLite/WebSocket server on port 4820 and the Vite client on port 5173.
5. Tell the user the dev server is running, where it's installed, and remind them: on startup it auto-writes hook entries into `~/.claude/settings.json` so every Claude Code session reports events to it. If they want to develop against a sandbox instead, restart with `CLAUDE_HOME=/tmp/claude-sandbox node server/index.js` instead of `npm run dev`.
