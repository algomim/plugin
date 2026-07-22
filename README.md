# Algomim plugins

Official monorepo for Algomim integrations with AI coding and work clients.

## Packages

| Package | Client | Purpose |
| --- | --- | --- |
| [`plugins/algomim`](./plugins/algomim) | Codex | Delegate bounded tasks to Algomim for a second opinion. |

Each directory under `plugins/` is an independently installable plugin package. Shared MCP and skill content stays at the package root; client-specific manifests use dedicated directories such as `.codex-plugin/` and, in the future, `.claude-plugin/`.

## Install in Codex

Add the repository as a marketplace source:

```shell
codex plugin marketplace add algomim/plugin
```

Install the plugin:

```shell
codex plugin add algomim@algomim
```

Set `ALGOMIM_API_KEY` to your Algomim Model API key, restart Codex, and start a new task so the MCP tool and skill are loaded.

## Security

Do not commit API keys or other credentials. Plugin configuration must reference environment-variable names, never raw secret values.
