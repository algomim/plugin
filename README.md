# Algomim plugins

Official monorepo for Algomim integrations with AI coding and work clients.

## Packages

| Package | Client | Purpose |
| --- | --- | --- |
| [`plugins/algomim`](./plugins/algomim) | Codex | Ask Algomim for bounded CAD, BIM, design, and technical guidance. |

Each directory under `plugins/` is an independently installable plugin package. Shared service configuration and assets stay at the package root; client-specific manifests use dedicated directories such as `.codex-plugin/` and, in the future, `.claude-plugin/`.

## Install in Codex

### Requirements

- Codex must be installed.
- You need a valid Algomim Model API key.

### 1. Configure the API key

Choose the command for the user's operating system. Replace the placeholder with the user's own Algomim Model API key.

#### Windows

For Codex desktop, create a persistent user environment variable from PowerShell:

```powershell
setx ALGOMIM_API_KEY "your sk-... Algomim API key"
```

`setx` affects applications opened after the command runs. Fully close Codex and reopen it.

#### macOS

For a quick test or pilot setup, add the key to the current macOS login session:

```shell
launchctl setenv ALGOMIM_API_KEY "your sk-... Algomim API key"
```

Fully quit Codex with `Cmd+Q` and reopen it. The value may be cleared after signing out or restarting the Mac; run the command again when needed. Remove it with `launchctl unsetenv ALGOMIM_API_KEY`.

For session-only Windows CLI setup or persistent macOS setup, follow [`plugins/algomim/docs/AUTHENTICATION.md`](./plugins/algomim/docs/AUTHENTICATION.md).

The API key only authenticates requests to Algomim. It does not download or install the plugin. Never add the raw key to plugin files or source control. Commands containing a key may remain in shell history.

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
algomim@algomim  installed, enabled  0.3.0
```

### 5. Restart and test

Fully close Codex, reopen it, and start a new task so the MCP tool is loaded. Then try:

```text
@algomim Rhino’daki box scriptini incele.
```

The plugin connects to the hosted Algomim MCP server. It does not install the Algomim model on the user's computer.

When invoked, Codex states what it is asking, waits for one non-streaming MCP result, and presents the Algomim response first. Every result is terminal: `completed`, `truncated`, or `failed`. Codex does not poll or automatically repeat an unchanged request.

For action requests, Algomim supplies expert guidance and Codex separately performs any local Rhino, Revit, or other tool calls. If Algomim returns `truncated` or `failed`, Codex reports that outcome and asks before retrying or continuing without Algomim. A materially new question can use another Algomim call; there is no numeric round limit.

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
