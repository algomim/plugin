# Publishing checklist

This repository currently distributes Algomim through a public Git-backed Codex marketplace. The official OpenAI plugin directory is a separate release channel with its own review process.

## Git marketplace release

- Keep `.codex-plugin/plugin.json`, `.mcp.json`, skill metadata, icons, legal links, and documentation in sync.
- Use a new semantic version for every user-visible release.
- Run the official plugin and skill validators.
- Scan the release diff for API keys, bearer tokens, local paths, and private endpoints.
- Commit and push the release to the public `main` branch.
- Verify installation from a clean Codex profile, including the plugin card, starter prompts, skill invocation, MCP connection, and missing-key error.
- Record user-visible changes in `CHANGELOG.md`.

## Official OpenAI directory readiness

Before submitting the hosted Algomim integration:

- Complete verified-developer and domain-verification requirements in OpenAI App Management.
- Provide the listing name, short and long descriptions, production logo, category, website, support email, privacy policy, and terms of service.
- Keep `https://api.algomim.com/mcp` publicly reachable and document its production authentication flow.
- Provide review credentials or an approved test path that does not expose production secrets.
- Add accurate MCP tool annotations to `delegate_task`. Because it starts a paid generation and records usage, the expected values are `readOnlyHint: false`, `destructiveHint: false`, and `openWorldHint: false` unless server behavior changes.
- Maintain a strict input schema and structured output schema.
- Prepare at least five positive and three negative test prompts, including missing authentication, insufficient balance, invalid input, timeout, and empty-response behavior.
- Supply release notes, country availability, and any additional reviewer instructions.
- Test the exact production build before submission.

OAuth is optional for the current API-key-based Git distribution. If introduced later, follow `AUTHENTICATION.md` and update the listing only after the full production authorization flow is live.
