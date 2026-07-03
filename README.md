# Mushaf Zarrar's Claude Code Plugin Marketplace

A single marketplace for installing all of my Claude Code plugins.

## Plugins

- **[lie-detector](https://github.com/MrZarrar/lie-detector)** — verifies that an AI coding agent's self-reported summary matches its actual git diff.
- **jarvis-dashboard** — launches the [Jarvis Dashboard](https://github.com/MrZarrar/Jarvis-Sub-Agent-Dashboard) (a Jarvis-style HUD reskin of the Claude Code Agent Monitor) via `/jarvis-dashboard:start`.

## Usage

Add the marketplace:

```
/plugin marketplace add MrZarrar/plugin-marketplace
```

Install a plugin:

```
/plugin install lie-detector@mushafzarrar-plugins
/plugin install jarvis-dashboard@mushafzarrar-plugins
```

## Adding a new plugin

1. Build the plugin (its own repo, or a thin wrapper under `plugins/<name>/` in this repo).
2. Add an entry to `.claude-plugin/marketplace.json`'s `plugins` array — either a `github` source pointing at the plugin's own repo, or a `./plugins/<name>` relative path for wrapper plugins that live in this repo.
3. Run `claude plugin validate .` before pushing.
