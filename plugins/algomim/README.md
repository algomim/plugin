# Algomim plugin

The Algomim plugin exposes the hosted Algomim model as one AEC expert-context tool for CAD, BIM, design, research, and technical work.

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

`message` is required, non-empty free text describing what the host agent needs Algomim to answer or advise on now. It is not an action that Algomim should claim to execute. `context` is optional, non-empty free text containing a self-contained snapshot of the current request. It can include:

- The current request verbatim, its interpreted intent, and the expected outcome.
- Current target applications, project environments, data sources, and connected-system status.
- Available capabilities and, when useful, exact tool names, descriptions, API surfaces, or input schemas.
- Current project state, constraints, sources, files, attachments, regulations, and evidence.
- Actions, tool results, verification findings, errors, and the current open question produced while fulfilling this request.

Codex supplies the information needed to reconstruct the current request, not a blind dump of conversation history, the machine, or the tool catalog. Credentials, secrets, unrelated private data, and responses from earlier independent consultations must not be included. Connected capabilities are tools available to Codex, not tools Algomim claims to run.

## Interaction

Use `@algomim` when Algomim must be consulted for the current request. The user can also state a natural, result-oriented AEC request without special syntax; Codex may consult Algomim when specialist guidance would materially improve the result. It does not consult for every mechanical action, parameter change, unchanged request, or routine status check.

The starter prompts demonstrate three distinct intents:

```text
@algomim Analyze the active project and report the most important design, coordination, and compliance risks.

@algomim Generate project-specific instructions for developing the selected facade detail. Do not modify the project yet.

@algomim Create a detailed facade in the active project and verify the result.
```

The same result-oriented requests can be written naturally without `@algomim`; the explicit mention is the force trigger. These examples are starters, not a fixed command language.

When runtime context is needed, Codex first gathers narrowly scoped, read-only facts and relevant capability information, then makes one terminal call. During the call it gives one short, honest status in the user's language, such as "I am getting the necessary guidance from Algomim..." It does not narrate skill loading, dump tool discovery, invent intermediate progress, or continue with filler status messages.

The hosted Model API consumes its private Inference Responses stream incrementally and folds it into one terminal MCP result. Codex does not necessarily render those private deltas live; token-by-token display remains client-dependent. Algomim reasons about the current intent and can return implementation guidance, review methods, decision and verification criteria, exact supplied-tool references, ready-to-adapt tool calls, scripts, API examples, or a direct answer. It can support AEC work across connected applications, APIs, tools, project environments, files, regulations, and other data sources. It does not execute host tools or claim to have inspected the project.

Every returned result is terminal:

- `completed`: a usable Algomim answer.
- `truncated`: the call ended with a partial or empty answer; it is not still running.
- `failed`: the call ended without an Algomim answer.

A completely empty completed response or connection failure is retried once inside the hosted service, so Codex still sees one MCP invocation. The failed first attempt is recorded with zero user charge; a successful retry is charged normally. `truncated` and `failed` results are not automatically retried.

Each call is independent: the hosted service does not retain a consultation session, conversation history, or previous response. A completed response can guide several host-tool rounds while Codex fulfills the current request. If another consultation becomes necessary, Codex constructs a new self-contained snapshot from the request's current state, evidence, capabilities, results, errors, and open question. It does not rely on server memory, poll, repeat an unchanged request, or re-consult for a mechanical action, parameter-only change, or routine status check. There is no plugin-defined numeric consultation limit.

Codex handles a completed result according to the user's intent:

- **Analyze:** gather the necessary read-only context, consult Algomim, and present the substantive findings. Do not change the project unless the user asks.
- **Instructions:** acknowledge that the instructions were received, keep the project unchanged, and present the requested artifact or ask whether to show, adapt, or apply it, according to the user's wording.
- **Action:** acknowledge receipt once, immediately continue with the relevant connected tools, verify the real result, and summarize the outcome. Do not stop after repeating Algomim's response as another prompt.

After a `truncated` or `failed` result, Codex reports the terminal outcome and asks before retrying or continuing without usable guidance.

The intended action flow is:

```text
User intent
    -> Codex gathers a current-request snapshot and relevant capabilities
    -> Codex shows one honest waiting status
    -> Algomim generates task-specific expert guidance
    -> Codex follows the Analyze, Instructions, or Action flow
    -> Codex verifies any real-world action
```

## Updating to 0.4.3

Version `0.4.3` keeps the `call_algomim({ message, context? })` contract, **Consult Algomim** title, private upstream streaming, and one terminal public MCP result. It clarifies the current-request-only context boundary and the Analyze, Instructions, and Action UX. Upgrade the marketplace and reinstall the plugin:

```shell
codex plugin marketplace upgrade algomim
codex plugin add algomim@algomim
```

Fully close and reopen Codex after the upgrade, then start a new task. Existing open tasks can retain the previous tool name and input schema.

## Release readiness

See [`docs/PUBLISHING.md`](./docs/PUBLISHING.md) for the Git marketplace release process and the additional work required before an official OpenAI directory submission. User-visible changes are recorded in [`CHANGELOG.md`](./CHANGELOG.md).

## Client compatibility

The MCP endpoint and service contract are client-neutral. Codex-specific presentation metadata lives in `.codex-plugin/`. A future Claude integration can add `.claude-plugin/` metadata without duplicating the hosted MCP configuration.
