# Publishing checklist

This repository currently distributes Algomim through a public Git-backed Codex marketplace. The official OpenAI plugin directory is a separate release channel with its own review process.

## Git marketplace release

- Keep `.codex-plugin/plugin.json`, `.mcp.json`, MCP metadata, icons, legal links, and documentation in sync.
- Use a new semantic version for every user-visible release.
- Run the official plugin validator.
- Scan the release diff for API keys, bearer tokens, local paths, and private endpoints.
- Commit and push the release to the public `main` branch.
- Verify installation from a clean Codex profile, including the plugin card, the Analyze, Instructions, and Action starter prompts, explicit `@algomim` invocation, natural result-oriented invocation, purposeful consultation guardrails, MCP connection, and missing-key error.
- For `0.4.3`, verify the upgrade path with `codex plugin marketplace upgrade algomim`, plugin reinstall, a full Codex restart, and a new task.
- Verify a fresh MCP connection reports server version `0.4.3`, exposes only `call_algomim`, and describes each context as a self-contained current-request snapshot without server-side history.
- Verify Analyze presents findings without modifying the project; Instructions keeps the project unchanged and offers the appropriate show, adapt, or apply next step; Action acknowledges completed guidance once, continues with connected tools, verifies the real result, and summarizes it.
- Verify the host gives one short, honest waiting status without skill-loading narration, tool-catalog dumping, invented progress, or repeated filler updates.
- Verify the hosted request is recorded with `stream: true`, the private Responses SSE stream is folded into one terminal MCP result, and the listing does not promise live token rendering in Codex.
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
- Verify the server initialization instructions say that every call is independent and terminal; `@algomim` explicitly forces consultation; natural AEC requests can trigger purposeful consultation when specialist guidance is materially useful; mechanical actions, parameter-only changes, unchanged requests, and routine status checks do not; and local actions remain owned by the host agent.
- Verify context guidance defines a self-contained snapshot of the current request covering the request verbatim, interpreted intent, expected outcome, current target applications and project environments, connected systems, available capabilities or tool schemas, current project evidence, constraints, sources, actions, tool results, errors, failed verification, and the open question without sending conversation history, earlier independent responses, credentials, or unrelated private data.
- Verify completed action guidance is acknowledged once and then reused across several host tool rounds without being relabeled as another prompt.
- Verify a purposeful later consultation remains independent and reconstructs the current request from an updated self-contained snapshot rather than relying on server memory or a previous response.
- Prepare exactly five positive and three negative test prompts, covering missing authentication, insufficient balance, invalid input, timeout, truncated output, model failure, and empty-response behavior across the set.
- Include golden checks for the three intent flows; one honest waiting status; direct presentation of analysis findings; instruction generation without project modification and with the appropriate next-step choice; completed-guidance acknowledgement followed by apply-verify-summarize behavior for action requests; exact supplied-tool or script examples when useful; an independent current-request follow-up consultation; purposeful consultation for materially new AEC intent, missing specialist guidance, material uncertainty or a domain decision, execution error, and failed verification; no call for mechanical actions, parameter-only changes, unchanged requests, and routine status checks; no automatic retry after `truncated` or `failed`; and local action only after a successful Algomim response.
- Verify that the one internal empty-response or connection retry remains a single MCP invocation, records both attempts, and charges the user only for a successful attempt.
- Supply release notes, country availability, and any additional reviewer instructions.
- Test the exact production build before submission.

OAuth is optional for the current API-key-based Git distribution. If introduced later, follow `AUTHENTICATION.md` and update the listing only after the full production authorization flow is live.
