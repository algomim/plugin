# Algomim plugins

Official monorepo for Algomim integrations with AI coding and work clients.

## Packages

| Package | Client | Purpose |
| --- | --- | --- |
| [`plugins/algomim`](./plugins/algomim) | Codex | Ask Algomim to answer or review bounded tasks. |

Each directory under `plugins/` is an independently installable plugin package. Shared MCP and skill content stays at the package root; client-specific manifests use dedicated directories such as `.codex-plugin/` and, in the future, `.claude-plugin/`.

## Install in Codex

### Requirements

- Codex must be installed.
- You need a valid Algomim Model API key.

### 1. Configure the API key

Create a user environment variable:

```text
Name:  ALGOMIM_API_KEY
Value: your sk-... Algomim API key
```

The API key only authenticates requests to Algomim. It does not download or install the plugin. Never add the raw key to plugin files or source control.

### 2. Add the Algomim marketplace

Run this command from PowerShell or another terminal:

```shell
codex plugin marketplace add https://github.com/algomim/plugin.git
```

This downloads the public repository and registers its plugin catalog as the `algomim` marketplace.

### 3. Install the plugin

```shell
codex plugin add algomim@algomim
```

The first `algomim` is the plugin name; the second is the marketplace name.

### 4. Verify the installation

```shell
codex plugin list
```

The output should include:

```text
algomim@algomim  installed, enabled  0.2.0
```

### 5. Restart and test

Fully close Codex, reopen it, and start a new task so the MCP tool and skill are loaded. Then try:

```text
@algomim Rhino’daki box scriptini incele.
```

The plugin connects to the hosted Algomim MCP server. It does not install the Algomim model on the user's computer.

When invoked, Codex states what it is asking, waits for the single non-streaming tool result, and presents the Algomim response first. Codex can ask Algomim again in later agentic rounds when the task benefits from another response.

The Plugins screen should show the Algomim name, logo, description, developer, starter prompts, and legal links. If an older card is cached, run the update commands below and restart Codex.

### Update

```shell
codex plugin marketplace upgrade algomim
codex plugin add algomim@algomim
```

## Security

Do not commit API keys or other credentials. Plugin configuration must reference environment-variable names, never raw secret values.

## Maintainers

Package metadata, authentication notes, release checks, and the changelog live under [`plugins/algomim`](./plugins/algomim). OAuth is a future server-backed authentication option; the current release uses `ALGOMIM_API_KEY`.
