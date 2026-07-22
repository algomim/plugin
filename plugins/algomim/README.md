# Algomim plugin

The Algomim plugin exposes the hosted Algomim model as one project-aware MCP consultation tool for CAD, BIM, design, research, and technical guidance.

Its Codex listing includes Algomim branding, starter prompts, developer details, and links to the website, privacy policy, and terms of service. The MCP tool carries the **Call Algomim** title, project-context guidance, and terminal result contract.

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

- `call_algomim({ message, context? })`: the single public MCP tool, presented to users as **Call Algomim**.

`message` is required, non-empty free text describing what guidance is needed now. `context` is optional, non-empty free text containing only the information relevant to that consultation. It can include:

- The user's goal and relevant conversation history.
- Current project state, constraints, sources, and evidence.
- Prior Algomim guidance and the local work performed from it.
- Tool results, verification findings, errors, and the reason for consulting again.

Codex does not blindly forward the entire conversation. Credentials and unrelated private data must not be included.

## Interaction

Invoke the plugin with `@algomim`, “Algomim'e sor,” or “Call Algomim.” When local project state is needed, Codex first gathers narrowly scoped, read-only context and then makes one non-streaming call. There is no skill-loading narration and no invented intermediate progress.

Every returned result is terminal:

- `completed`: a usable Algomim answer.
- `truncated`: the call ended with a partial or empty answer; it is not still running.
- `failed`: the call ended without an Algomim answer.

A completely empty completed response or connection failure is retried once inside the hosted service, so Codex still sees one MCP invocation. The failed first attempt is recorded with zero user charge; a successful retry is charged normally. `truncated` and `failed` results are not automatically retried.

Each call is independent: the hosted service does not retain a consultation session or previous response. One Algomim response can guide several local tool rounds. If new project evidence, an error, a decision point, or a failed verification makes more guidance useful, Codex calls `call_algomim` again and carries the relevant prior guidance and updated project context in that new call. Codex does not poll, repeat an unchanged request, or consult Algomim for every mechanical step. There is no plugin-defined numeric consultation limit.

For information and review requests, Codex presents the Algomim response directly. For action requests, Codex uses the guidance, runs the appropriate Rhino, Revit, AutoCAD, or other local tools, verifies the real project result, and then summarizes the outcome and Algomim's contribution. Algomim does not claim to see or modify the local project; local execution remains owned by Codex. After a `truncated` or `failed` result, Codex reports the terminal outcome and asks before retrying or continuing without Algomim.

## Updating to 0.4.0

Version `0.4.0` introduces the breaking `call_algomim({ message, context? })` tool contract. Upgrade the marketplace and reinstall the plugin:

```shell
codex plugin marketplace upgrade algomim
codex plugin add algomim@algomim
```

Fully close and reopen Codex after the upgrade, then start a new task. Existing open tasks can retain the previous tool name and input schema.

## Release readiness

See [`docs/PUBLISHING.md`](./docs/PUBLISHING.md) for the Git marketplace release process and the additional work required before an official OpenAI directory submission. User-visible changes are recorded in [`CHANGELOG.md`](./CHANGELOG.md).

## Client compatibility

The MCP endpoint and service contract are client-neutral. Codex-specific presentation metadata lives in `.codex-plugin/`. A future Claude integration can add `.claude-plugin/` metadata without duplicating the hosted MCP configuration.
