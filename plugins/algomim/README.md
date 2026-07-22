# Algomim plugin

The Algomim plugin exposes the hosted Algomim model as an MCP tool and provides a reusable workflow for requesting an independent second opinion.

## Authentication

Set `ALGOMIM_API_KEY` to an Algomim Model API key before starting Codex. The plugin sends it as a bearer token to `https://api.algomim.com/mcp`.

Never place the raw API key in `.mcp.json`, a shell script, or source control.

## Included capability

- `delegate_task`: delegates one bounded task to Algomim.
- `second-opinion` skill: guides safe delegation, attribution, and empty-response handling.

## Client compatibility

The MCP endpoint and skill content are client-neutral. Codex-specific metadata lives in `.codex-plugin/`. A future Claude integration can add `.claude-plugin/` metadata without duplicating the shared MCP or skill content.
