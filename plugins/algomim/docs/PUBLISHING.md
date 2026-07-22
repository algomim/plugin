# Publishing checklist

This repository currently distributes Algomim through a public Git-backed Codex marketplace. The official OpenAI plugin directory is a separate release channel with its own review process.

## Git marketplace release

- Keep `.codex-plugin/plugin.json`, `.mcp.json`, MCP metadata, icons, legal links, and documentation in sync.
- Use a new semantic version for every user-visible release.
- Run the official plugin validator.
- Scan the release diff for API keys, bearer tokens, local paths, and private endpoints.
- Commit and push the release to the public `main` branch.
- Verify installation from a clean Codex profile, including the plugin card, starter prompts, explicit `@algomim` invocation, MCP connection, and missing-key error.
- For `0.4.0`, verify the breaking upgrade path with `codex plugin marketplace upgrade algomim`, plugin reinstall, a full Codex restart, and a new task.
- Record user-visible changes in `CHANGELOG.md`.

## Official OpenAI directory readiness

Before submitting the hosted Algomim integration:

- Complete verified-developer and domain-verification requirements in OpenAI App Management.
- Provide the listing name, short and long descriptions, production logo, category, and HTTPS website, support, privacy-policy, and terms-of-service URLs.
- Keep the official-directory short description at 30 characters or fewer; Git marketplace card copy may differ.
- Keep `https://api.algomim.com/mcp` publicly reachable and document its production authentication flow.
- Provide review credentials or an approved test path that does not expose production secrets.
- Keep accurate MCP tool annotations on `call_algomim`. Because it starts a paid generation and records usage, the expected values are `readOnlyHint: false`, `destructiveHint: false`, `idempotentHint: false`, and `openWorldHint: false` unless server behavior changes.
- Maintain the strict `call_algomim({ message, context? })` input schema and a terminal structured output schema with `completed`, `truncated`, and `failed` states. Both inputs are free text; `message` is required and non-empty, while a supplied `context` must also be non-empty.
- Verify the server initialization instructions say that every call is independent and terminal, relevant history is carried by the caller, unchanged requests are not automatically repeated, and local actions remain owned by Codex.
- Verify context guidance covers relevant user intent, conversation history, current project evidence, prior Algomim guidance, local tool results, and errors without sending credentials or unrelated private data.
- Verify an Algomim response can guide several local tool rounds and that a purposeful later consultation includes the relevant prior guidance and updated evidence.
- Prepare exactly five positive and three negative test prompts, covering missing authentication, insufficient balance, invalid input, timeout, truncated output, model failure, and empty-response behavior across the set.
- Include golden checks for direct presentation of information and review responses, apply-verify-summarize behavior for action requests, a contextual follow-up consultation, a negative no-call prompt, no automatic retry after `truncated` or `failed`, and local action only after a successful Algomim response.
- Verify that the one internal empty-response or connection retry remains a single MCP invocation, records both attempts, and charges the user only for a successful attempt.
- Supply release notes, country availability, and any additional reviewer instructions.
- Test the exact production build before submission.

OAuth is optional for the current API-key-based Git distribution. If introduced later, follow `AUTHENTICATION.md` and update the listing only after the full production authorization flow is live.
