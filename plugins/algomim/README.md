# Algomim plugin

The Algomim plugin exposes the hosted Algomim model as an MCP tool and provides a reusable workflow for asking Algomim to answer or review a bounded task.

Its Codex listing includes Algomim branding, starter prompts, developer details, and links to the website, privacy policy, and terms of service. Skill-level metadata lets Codex present the Ask Algomim capability with its own name, description, and icons.

## Authentication

Set `ALGOMIM_API_KEY` to an Algomim Model API key before starting Codex. The plugin sends it as a bearer token to `https://api.algomim.com/mcp`.

Never place the raw API key in `.mcp.json`, a shell script, or source control.

See [`docs/AUTHENTICATION.md`](./docs/AUTHENTICATION.md) for the current API-key flow and the boundary for a future OAuth migration.

## Included capability

- `delegate_task`: delegates one bounded task to Algomim.
- `ask-algomim` skill: guides explicit invocation, safe delegation, attribution, repeated use, and empty-response handling.

## Interaction

Invoke the plugin with `@algomim` or explicitly ask Codex to ask Algomim. Before each tool call, Codex gives one topic-specific status message. Each invocation waits for one non-streaming result; it does not invent intermediate steps or partial progress.

Codex may ask Algomim again in later agentic rounds when another response would help the same task. There is no plugin-defined round counter or separate iteration mode, but every delegated instruction remains concrete and bounded. An empty completed response is retried once with a shorter instruction.

The result is presented first as the Algomim response. Codex adds its own comparison only when the user requests one. Tool failures are explained in user-focused language without inventing an Algomim answer.

## Release readiness

See [`docs/PUBLISHING.md`](./docs/PUBLISHING.md) for the Git marketplace release process and the additional work required before an official OpenAI directory submission. User-visible changes are recorded in [`CHANGELOG.md`](./CHANGELOG.md).

## Client compatibility

The MCP endpoint and skill content are client-neutral. Codex-specific metadata lives in `.codex-plugin/`. A future Claude integration can add `.claude-plugin/` metadata without duplicating the shared MCP or skill content.
