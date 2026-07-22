---
name: ask-algomim
description: Ask Algomim to answer or review a bounded task through the Algomim MCP when the user invokes @algomim or explicitly asks "Algomim'e sor", "Ask Algomim", or an equivalent request. Use it again in later agentic rounds when another Algomim response would materially help the same task.
---

# Ask Algomim

Use the Algomim MCP `delegate_task` tool to obtain an Algomim response.

## Workflow

1. Before every call, give one short, topic-specific status message in the user's language, such as `Rhino’daki box scriptini Algomim’e soruyorum…`.
2. Convert the current need into one concrete, bounded instruction.
3. Include all supporting context needed to answer without relying on hidden conversation state. When calling again in a later agentic round, include the relevant new context and prior conclusions.
4. Exclude API keys, credentials, personal data, and unrelated repository content.
5. Call `delegate_task`. Set `maxOutputTokens` only when a bounded response size is useful.
6. Call `delegate_task` again in later agentic rounds whenever another Algomim response would materially help. Do not impose a numeric round limit or create a separate iteration mode. Keep every call purposeful and do not repeat an already completed request unchanged.
7. If a call reports completion with empty text, retry that call once with a shorter, direct instruction.
8. Present the returned text first as `Algomim yanıtı`. Add your own assessment or a comparison only when the user asks for one.
9. If the retry is also empty or incomplete, report that Algomim did not return a usable response. If a call fails, explain the same failure in user-focused language while preserving any useful cause or next step. Never invent an Algomim response.

## Waiting state

Treat each invocation as one non-streaming tool call. While it runs, show only the truthful status for that call. Do not claim token streaming, internal reasoning steps, partial findings, or progress that the tool did not provide.

Do not delegate actions that modify external state. The tool is for analysis, review, and content generation.
