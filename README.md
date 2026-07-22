# Algomim plugins

Official monorepo for Algomim integrations with AI coding and work clients.

## Packages

| Package | Client | Purpose |
| --- | --- | --- |
| [`plugins/algomim`](./plugins/algomim) | Codex | Consult Algomim for reusable AEC expert context across connected tools and projects. |

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
algomim@algomim  installed, enabled  0.4.2
```

### 5. Restart and test

Fully close Codex, reopen it, and start a new task so the MCP tool is loaded. Then try:

```text
@algomim Get the expert context needed for this design task, use the connected applications to complete it, and verify the result.
```

The plugin connects to the hosted Algomim MCP server. It does not install the Algomim model on the user's computer.

For each consultation, Codex supplies `call_algomim` with a required free-text `message` and, when useful, a free-text `context`. The context can carry the original request, interpreted intent, expected outcome, relevant conversation, target environments, connected systems, relevant available capabilities or tool schemas, project evidence, constraints, prior guidance, actions, results, errors, failed verification, and the current open question. Codex sends all task-relevant context rather than an exhaustive dump and excludes credentials, secrets, and unrelated private data.

Every public MCP call is independent and terminal: `completed`, `truncated`, or `failed`. The server does not retain consultation history. The hosted Model API consumes its private Inference Responses stream internally and returns one terminal MCP result; live token-by-token rendering in Codex remains client-dependent. One completed response becomes working expert context and can guide several rounds across connected tools, applications, and data sources. Codex consults proactively when a materially new AEC intent, missing specialist guidance, material uncertainty or a domain decision, an execution error, or failed verification requires expert context. It does not poll, automatically repeat an unchanged request, or call Algomim for every mechanical step, parameter change, unchanged request, or routine status check.

For information and review requests, Codex presents the substantive answer directly. For action requests, Codex acknowledges the completed guidance once, immediately continues with the appropriate connected tools, verifies the result in the real project, and then summarizes the outcome. Algomim may supply exact references to available tools, ready-to-adapt calls, scripts, API examples, fallbacks, and verification criteria, but local actions remain under Codex's control. If Algomim returns `truncated` or `failed`, Codex reports that terminal outcome and asks before retrying or continuing without usable guidance. There is no plugin-defined numeric consultation limit.

The Plugins screen should show the Algomim name, logo, description, developer, starter prompts, and legal links. If an older card is cached, run the update commands below and restart Codex.

### Update

Version `0.4.2` keeps `call_algomim({ message, context? })` and the **Consult Algomim** title. It strengthens proactive consultation guidance and enables private upstream streaming while preserving one terminal MCP result. After upgrading, reinstall the plugin, fully restart Codex, and begin a new task so Codex loads the new instructions and metadata.

```shell
codex plugin marketplace upgrade algomim
codex plugin add algomim@algomim
```

## Security

Do not commit API keys or other credentials. Plugin configuration must reference environment-variable names, never raw secret values.

## Maintainers

Package metadata, authentication notes, release checks, and the changelog live under [`plugins/algomim`](./plugins/algomim). OAuth is a future server-backed authentication option; the current release uses `ALGOMIM_API_KEY`.
