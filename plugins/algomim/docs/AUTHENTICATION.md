# Authentication

## Current API-key flow

The plugin connects to the hosted MCP endpoint at `https://api.algomim.com/mcp`. Codex reads the user's Algomim Model API key from the `ALGOMIM_API_KEY` environment variable and sends it as a bearer token.

The plugin never stores or distributes the secret. Users must set the environment variable before starting Codex and restart Codex after changing it.

```text
Name:  ALGOMIM_API_KEY
Value: your Algomim Model API key
```

Keep `.mcp.json` limited to the environment-variable name. Never add a real key to the repository, screenshots, support logs, or installation scripts.

## Future OAuth flow

OAuth is not implemented by this plugin today. When the Algomim MCP server supports the standard MCP authorization flow, the migration should preserve the public endpoint and move identity handling to the server:

1. Publish the authorization-server metadata and dynamic client-registration behavior required by the supported MCP clients.
2. Register and verify the production domain in the OpenAI developer console.
3. Replace the API-key requirement in the install instructions with the client-provided sign-in flow.
4. Test first sign-in, consent, token refresh, reauthorization, revocation, logout, and cancelled-login behavior.
5. Decide whether API keys remain available as a documented fallback for CLI and automation clients.

Do not advertise OAuth in the plugin metadata until the production MCP endpoint and complete sign-in lifecycle are available.
