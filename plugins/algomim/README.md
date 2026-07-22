# Algomim plugin

The Algomim plugin exposes the hosted Algomim model as one on-demand AEC expert-context tool for CAD, BIM, design, research, and technical work.

Its Codex listing includes Algomim branding, starter prompts, developer details, and links to the website, privacy policy, and terms of service. The MCP tool carries the **Consult Algomim** title, runtime-context guidance, and terminal result contract.

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

- `call_algomim({ message, context? })`: the single public MCP tool, presented to users as **Consult Algomim**.

`message` is required, non-empty free text describing what the host agent needs Algomim to answer or advise on now. It is not an action that Algomim should claim to execute. `context` is optional, non-empty free text containing only the runtime information relevant to that consultation. It can include:

- The original request, interpreted intent, and expected outcome.
- Relevant conversation, target environments, and connected-system status.
- Available capabilities and, when useful, exact tool names, descriptions, or input schemas.
- Current project state, constraints, sources, files, attachments, and evidence.
- Prior Algomim guidance and the work performed from it.
- Tool results, verification findings, errors, and the current open question.

Codex supplies all task-relevant context, not a blind dump of the conversation, machine, or tool catalog. Credentials, secrets, and unrelated private data must not be included. Connected capabilities are tools available to Codex, not tools Algomim claims to run.

## Interaction

Invoke the plugin with `@algomim`, “Algomim'e sor,” or “Consult Algomim.” Codex can also consult automatically when the work enters a materially new AEC intent, existing guidance no longer covers the task, or a domain decision would materially benefit from expert context. It does not consult for every mechanical action or parameter change.

When runtime context is needed, Codex first gathers narrowly scoped, read-only facts and relevant capability information, then makes one non-streaming call. There is no skill-loading narration and no invented intermediate progress. Algomim reasons about the underlying intent and can return reusable implementation guidance, review methods, decision and verification criteria, exact supplied-tool references, ready-to-adapt tool calls, scripts, API examples, or a direct answer. It does not execute local tools or claim to have inspected the project.

Every returned result is terminal:

- `completed`: a usable Algomim answer.
- `truncated`: the call ended with a partial or empty answer; it is not still running.
- `failed`: the call ended without an Algomim answer.

A completely empty completed response or connection failure is retried once inside the hosted service, so Codex still sees one MCP invocation. The failed first attempt is recorded with zero user charge; a successful retry is charged normally. `truncated` and `failed` results are not automatically retried.

Each call is independent: the hosted service does not retain a consultation session or previous response. A completed response becomes working expert context for the current intent and can guide several local tool rounds. Codex consults again only when a material intent or environment change, new evidence, an error, a decision point, or a failed verification invalidates or extends that guidance. It carries the relevant prior guidance and updated runtime context in the new call. Codex does not poll or repeat an unchanged request. There is no plugin-defined numeric consultation limit.

For information and review requests, Codex presents the substantive Algomim response directly. For action requests, Codex briefly acknowledges that it received the necessary guidance, immediately continues its normal agentic loop with the connected tools and applications, verifies the real result, and then summarizes the outcome. It does not stop to rewrite the guidance as another prompt. If the user explicitly asks for a prompt, instruction artifact, or reusable procedure, Codex presents that artifact instead of executing it. After a `truncated` or `failed` result, Codex reports the terminal outcome and asks before retrying or continuing without usable guidance.

The intended action flow is:

```text
User intent
    -> Codex gathers relevant runtime context and capabilities
    -> Algomim generates reusable expert guidance
    -> Codex acknowledges receipt once
    -> Codex continues with connected tools
    -> Codex verifies the real result
    -> Codex consults again only when the guidance needs revision
```

## Updating to 0.4.1

Version `0.4.1` keeps the `call_algomim({ message, context? })` contract and updates the orchestration behavior, visible title, and starter prompts. Upgrade the marketplace and reinstall the plugin:

```shell
codex plugin marketplace upgrade algomim
codex plugin add algomim@algomim
```

Fully close and reopen Codex after the upgrade, then start a new task. Existing open tasks can retain the previous tool name and input schema.

## Release readiness

See [`docs/PUBLISHING.md`](./docs/PUBLISHING.md) for the Git marketplace release process and the additional work required before an official OpenAI directory submission. User-visible changes are recorded in [`CHANGELOG.md`](./CHANGELOG.md).

## Client compatibility

The MCP endpoint and service contract are client-neutral. Codex-specific presentation metadata lives in `.codex-plugin/`. A future Claude integration can add `.claude-plugin/` metadata without duplicating the hosted MCP configuration.
