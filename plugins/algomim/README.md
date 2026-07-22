# Algomim plugin

The Algomim plugin exposes the hosted Algomim model as one MCP tool for bounded CAD, BIM, design, research, and technical guidance.

Its Codex listing includes Algomim branding, starter prompts, developer details, and links to the website, privacy policy, and terms of service. The MCP tool carries the Ask Algomim title, trigger guidance, and terminal result contract.

## Authentication

Set `ALGOMIM_API_KEY` to an Algomim Model API key before starting Codex. The plugin sends it as a bearer token to `https://api.algomim.com/mcp`.

Quick setup:

```powershell
# Windows user environment; restart Codex after running
setx ALGOMIM_API_KEY "your sk-... Algomim API key"
```

```shell
# macOS login session; quit Codex with Cmd+Q and reopen it
launchctl setenv ALGOMIM_API_KEY "your sk-... Algomim API key"
```

On Windows, `setx` persists for the user. On macOS, `launchctl setenv` is intended for quick test or pilot setup and may be cleared after sign-out or restart.

Never place the raw API key in `.mcp.json`, a shell script, or source control.

See [`docs/AUTHENTICATION.md`](./docs/AUTHENTICATION.md) for the current API-key flow and the boundary for a future OAuth migration.

## Included capability

- `delegate_task`: the compatibility-stable MCP tool ID, presented to users as **Ask Algomim**.

## Interaction

Invoke the plugin with `@algomim` or explicitly ask Codex to ask Algomim. Codex gives one short, topic-specific status message and waits for one non-streaming result. There is no skill-loading narration and no invented intermediate progress.

Every returned result is terminal:

- `completed`: a usable Algomim answer.
- `truncated`: the call ended with a partial or empty answer; it is not still running.
- `failed`: the call ended without an Algomim answer.

A completely empty completed response or connection failure is retried once inside the hosted service, so Codex still sees one MCP invocation. The failed first attempt is recorded with zero user charge; a successful retry is charged normally. Codex does not poll or automatically repeat an unchanged request. A materially new question may use another call; there is no numeric round limit.

The Algomim response is presented first. Codex adds its own comparison only when requested. For action requests, Algomim provides guidance while Codex separately runs and attributes local Rhino, Revit, or other tools. After a `truncated` or `failed` result, Codex asks before retrying or continuing without Algomim.

## Release readiness

See [`docs/PUBLISHING.md`](./docs/PUBLISHING.md) for the Git marketplace release process and the additional work required before an official OpenAI directory submission. User-visible changes are recorded in [`CHANGELOG.md`](./CHANGELOG.md).

## Client compatibility

The MCP endpoint and service contract are client-neutral. Codex-specific presentation metadata lives in `.codex-plugin/`. A future Claude integration can add `.claude-plugin/` metadata without duplicating the hosted MCP configuration.
