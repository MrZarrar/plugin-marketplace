---
description: Start the local Jarvis Dashboard dev server (server on :4820, client on :5173)
---

Start the Jarvis Dashboard.

1. Find the local checkout: use `$JARVIS_DASHBOARD_HOME` if set, otherwise look for a directory named `jarvis-dashboard/app` under the user's common project roots (e.g. `~/ClaudeSkillsProject/jarvis-dashboard/app`). If you can't find one, ask the user for the path or offer to clone `https://github.com/MrZarrar/Jarvis-Sub-Agent-Dashboard` (branch `jarvis-reskin`).
2. In that directory, run `npm run dev` in the background. This starts the Express/SQLite/WebSocket server on port 4820 and the Vite client on port 5173.
3. Tell the user the dev server is running and remind them: on startup it auto-writes hook entries into `~/.claude/settings.json` so every Claude Code session reports events to it. If they want to develop against a sandbox instead, restart with `CLAUDE_HOME=/tmp/claude-sandbox node server/index.js` instead of `npm run dev`.
