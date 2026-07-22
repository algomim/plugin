# Algomim plugins

Official monorepo for Algomim integrations with AI coding and work clients.

## Packages

| Package | Client | Purpose |
| --- | --- | --- |
| [`plugins/algomim`](./plugins/algomim) | Codex | Delegate bounded tasks to Algomim for a second opinion. |

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
algomim@algomim  installed, enabled  0.1.0
```

### 5. Restart and test

Fully close Codex, reopen it, and start a new task so the MCP tool and skill are loaded. Then try:

```text
Ask Algomim for a second opinion on this approach.
```

The plugin connects to the hosted Algomim MCP server. It does not install the Algomim model on the user's computer.

### Update

```shell
codex plugin marketplace upgrade algomim
codex plugin add algomim@algomim
```

## Security

Do not commit API keys or other credentials. Plugin configuration must reference environment-variable names, never raw secret values.
