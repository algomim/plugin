# Changelog

## 0.4.3

- Added distinct starter prompts and host UX for three user intents: analyze the current project, generate instructions without modifying it, and perform an action followed by verification.
- Clarified that `@algomim` is the explicit force trigger while natural, result-oriented AEC requests may still cause a purposeful consultation when specialist guidance is materially useful.
- Defined every consultation context as a self-contained snapshot of the current request; the hosted service retains no session, conversation history, prior response, or cross-request guidance.
- Expanded the any-to-any AEC description to cover connected applications, APIs, tools, project environments, files, regulations, and data sources without assigning host-tool execution to Algomim.
- Added one honest waiting-status rule and preserved private upstream `stream: true` processing with one terminal public MCP result; live token rendering remains client-dependent.

## 0.4.2

- Strengthened the MCP host instructions so Codex proactively consults Algomim for a materially new AEC intent, missing specialist guidance, material uncertainty or a domain decision, an execution error, or failed verification without requiring an explicit mention every time.
- Kept automatic consultation purposeful: existing guidance is reused across mechanical actions, parameter changes, unchanged requests, and routine status checks.
- Switched the hosted Model API-to-Inference request to `stream: true` and added bounded, validated Responses SSE consumption, including fragmented UTF-8 output, empty output, terminal errors, and malformed terminal-envelope handling.
- Preserved the single terminal MCP result and the public `call_algomim({ message, context? })` contract. Live token-by-token rendering in Codex remains client-dependent and is not promised by this release.

## 0.4.1

- Reframed completed Algomim answers as reusable working expert context for the host agent rather than delegated execution or a second user-facing prompt.
- Allowed purposeful automatic consultation when work enters a materially new AEC intent or existing guidance no longer covers a domain decision, while preserving explicit `@algomim` invocation.
- Expanded caller-carried context guidance to cover expected outcomes, target environments, connected systems, relevant available capabilities and tool schemas, files, actions, results, failed verification, and open questions.
- Directed Algomim to reason about the underlying intent, prefer supplied host-operable automation, and provide exact tool references, ready-to-adapt calls, scripts, API examples, fallbacks, and verification criteria when useful.
- Added the action handoff: acknowledge completed guidance once, immediately continue with connected tools, and verify the real result without relabeling the answer as another Algomim prompt.
- Changed the visible MCP title to **Consult Algomim** while keeping the stable `call_algomim` tool ID and `message + context?` contract.
- Replaced Rhino-specific and mixed-language starter prompts with English, provider-neutral examples for review, design, standards, and connected-tool workflows.

## 0.4.0

- Replaced the public `delegate_task` tool with the breaking `call_algomim({ message, context? })` contract and the **Call Algomim** title; no legacy alias is published.
- Reframed Algomim as a project-aware AEC consultation that can provide direct answers, analysis methods, adaptive instructions, decision criteria, verification guidance, or code examples.
- Added caller-carried context for relevant user intent, conversation history, project evidence, prior guidance, local tool results, and errors while keeping every call independent and terminal.
- Documented purposeful re-consultation after new evidence, errors, decision points, or failed verification without polling, unchanged-request repetition, or a numeric round limit.
- Clarified direct presentation for information and review requests and the apply, verify, then summarize flow for action requests.
- Added breaking-upgrade instructions requiring a marketplace upgrade, plugin reinstall, full Codex restart, and a new task.

## 0.3.0

- Changed the Codex package to an MCP-first integration and removed the separate Ask Algomim skill layer.
- Kept the compatibility-stable `delegate_task` tool ID while presenting it as **Ask Algomim**.
- Documented terminal `completed`, `truncated`, and `failed` results and removed automatic polling or unchanged-request repetition.
- Kept a deprecated structured `text` alias while clients migrate to the new `answer` field.
- Removed the model-selected output-token limit from the public tool workflow.
- Moved the single empty-response or connection retry inside the hosted call and made the failed first attempt non-chargeable to the user.
- Moved the Algomim logo to the plugin-level assets directory.
- Added copy-paste API-key setup instructions for Windows and macOS.
- Clarified which environment-variable commands are persistent, PowerShell-session-only, or macOS-login-session-only.
- Documented the persistent macOS Codex `.env` setup and restart requirements.

## 0.2.1

- Updated the compact listing description to `Built for consistent CAD and BIM results.`
- Standardized plugin and skill presentation on the official Algomim logo.

## 0.2.0

- Added plugin-card and skill-interface metadata for better discovery in Codex.
- Added Algomim brand artwork, color, legal links, and support contact metadata.
- Replaced the second-opinion workflow with the explicitly triggered Ask Algomim workflow.
- Added a topic-specific waiting message and Algomim-first result presentation without invented intermediate progress.
- Allowed purposeful Algomim calls in later agentic rounds without a plugin-defined round limit or separate iteration mode.
- Kept one automatic retry for completed calls that return empty text and clarified user-focused error handling.
- Documented API-key authentication and the future OAuth migration boundary.
- Added Git marketplace and official OpenAI directory publishing checklists.

## 0.1.0

- Added the Algomim MCP server and second-opinion skill.
