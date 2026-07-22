# Changelog

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
