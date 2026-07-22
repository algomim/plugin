# Algomim plugins

Official monorepo for Algomim integrations with AI coding and work clients.

## Packages

| Package | Client | Purpose |
| --- | --- | --- |
| [`plugins/algomim`](./plugins/algomim) | Codex | Consult Algomim for task-specific AEC guidance across connected tools and project environments. |

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
algomim@algomim  installed, enabled  0.4.3
```

### 5. Restart and test

Fully close Codex, reopen it, and start a new task so the MCP tool is loaded. Then try:

```text
@algomim Analyze the active project and report the most important design, coordination, and compliance risks.

@algomim Generate project-specific instructions for developing the selected facade detail. Do not modify the project yet.

@algomim Create a detailed facade in the active project and verify the result.
```

The plugin connects to the hosted Algomim MCP server. It does not install the Algomim model on the user's computer.

`@algomim` explicitly selects Algomim for the current request. Users can also describe the desired AEC result naturally without special syntax; Codex may consult Algomim when specialist guidance would materially improve the outcome. The examples above are starter prompts, not a fixed command language.

For each consultation, Codex supplies `call_algomim` with a required free-text `message` and, when useful, a free-text `context`. The context is a self-contained snapshot of the current request: the request verbatim, interpreted intent, expected outcome, current applications and project environments, connected systems, relevant capabilities or tool schemas, project evidence, constraints, sources, actions, current results, errors, failed verification, and the open question. Codex sends only what is needed to reconstruct the current request, rather than conversation history or an exhaustive machine and tool dump, and excludes credentials, secrets, unrelated private data, and responses from earlier independent consultations.

Every public MCP call is independent and terminal: `completed`, `truncated`, or `failed`. The server does not retain a session, conversation history, or previous response. The hosted Model API consumes its private Inference Responses stream internally and returns one terminal MCP result; live token-by-token rendering in Codex remains client-dependent. A completed response can guide several tool rounds while Codex fulfills the current request. If another consultation is necessary, Codex builds another self-contained snapshot from the request's current state. It does not poll, automatically repeat an unchanged request, or call Algomim for every mechanical step, parameter change, unchanged request, or routine status check.

Codex gives one short, honest waiting status while Algomim is working. It does not narrate skill loading, dump tool discovery, invent intermediate progress, or fill the wait with repeated messages. Algomim can support any-to-any AEC work across connected applications, APIs, tools, files, regulations, data sources, and project environments, but execution remains under the host agent's control.

The completed-result UX depends on the request:

- **Analyze:** Codex gathers read-only current context, consults Algomim, and presents the findings without changing the project.
- **Instructions:** Codex confirms that the instructions were received, keeps the project unchanged, and presents them or asks whether to show, adapt, or apply them according to the request.
- **Action:** Codex confirms receipt once, continues immediately with the appropriate connected tools, verifies the real result, and summarizes the outcome.

If Algomim returns `truncated` or `failed`, Codex reports that terminal outcome and asks before retrying or continuing without usable guidance. There is no plugin-defined numeric consultation limit.

The Plugins screen should show the Algomim name, logo, description, developer, starter prompts, and legal links. If an older card is cached, run the update commands below and restart Codex.

### Update

Version `0.4.3` keeps `call_algomim({ message, context? })`, the **Consult Algomim** title, private upstream streaming, and one terminal public MCP result. It clarifies the current-request-only context boundary and the Analyze, Instructions, and Action UX. After upgrading, reinstall the plugin, fully restart Codex, and begin a new task so Codex loads the new instructions and metadata.

```shell
codex plugin marketplace upgrade algomim
codex plugin add algomim@algomim
```

## Security

Do not commit API keys or other credentials. Plugin configuration must reference environment-variable names, never raw secret values.

## Maintainers

Package metadata, authentication notes, release checks, and the changelog live under [`plugins/algomim`](./plugins/algomim). OAuth is a future server-backed authentication option; the current release uses `ALGOMIM_API_KEY`.
