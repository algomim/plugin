# Authentication

## Current API-key flow

The plugin connects to the hosted MCP endpoint at `https://api.algomim.com/mcp`. Codex reads the user's Algomim Model API key from the `ALGOMIM_API_KEY` environment variable and sends it as a bearer token.

The plugin never stores or distributes the secret. Users must set the environment variable before starting Codex and restart Codex after changing it.

```text
Name:  ALGOMIM_API_KEY
Value: your Algomim Model API key
```

## Windows

### Persistent user environment for Codex desktop

Run from PowerShell:

```powershell
setx ALGOMIM_API_KEY "your sk-... Algomim API key"
```

`setx` stores the value for the Windows user and makes it available to applications opened afterward. Fully close Codex and reopen it. It does not update an already-running Codex or the current PowerShell process.

### Current PowerShell session only

For a temporary Codex CLI session:

```powershell
$env:ALGOMIM_API_KEY="your sk-... Algomim API key"
codex
```

The value is removed when that PowerShell session closes and is inherited only by processes launched from that window.

## macOS

### Current macOS login session

For a quick test or pilot setup:

```shell
launchctl setenv ALGOMIM_API_KEY "your sk-... Algomim API key"
```

Fully quit Codex with `Cmd+Q` and reopen it. Apps launched afterward can read the value. It may be cleared when the user signs out or restarts the Mac.

Remove the session value with:

```shell
launchctl unsetenv ALGOMIM_API_KEY
```

### Persistent Codex desktop setup

Create and open Codex's environment file:

```shell
mkdir -p ~/.codex
touch ~/.codex/.env
open -e ~/.codex/.env
```

Add this line in TextEdit, save the file, and close it:

```shell
export ALGOMIM_API_KEY="your sk-... Algomim API key"
```

Restrict the file to the current user, then fully restart Codex:

```shell
chmod 600 ~/.codex/.env
```

## Secret handling

Keep `.mcp.json` limited to the environment-variable name. Never add a real key to the repository, screenshots, support logs, or installation scripts.

Commands containing an API key may remain in shell history. The current commands are suitable for controlled API-key pilots; OAuth should replace manual key entry for the production consumer flow.

## Future OAuth flow

OAuth is not implemented by this plugin today. When the Algomim MCP server supports the standard MCP authorization flow, the migration should preserve the public endpoint and move identity handling to the server:

1. Publish the authorization-server metadata and dynamic client-registration behavior required by the supported MCP clients.
2. Register and verify the production domain in the OpenAI developer console.
3. Replace the API-key requirement in the install instructions with the client-provided sign-in flow.
4. Test first sign-in, consent, token refresh, reauthorization, revocation, logout, and cancelled-login behavior.
5. Decide whether API keys remain available as a documented fallback for CLI and automation clients.

Do not advertise OAuth in the plugin metadata until the production MCP endpoint and complete sign-in lifecycle are available.
