---
name: second-opinion
description: Delegate one bounded question or review task to Algomim when the user asks for an Algomim opinion, a second opinion, an independent review, or a comparison with another model.
---

# Algomim Second Opinion

Use the Algomim MCP `delegate_task` tool to obtain an independent response.

## Workflow

1. Convert the request into one concrete, bounded instruction.
2. Include all supporting context needed to answer without relying on the current conversation.
3. Exclude API keys, credentials, personal data, and unrelated repository content.
4. Call `delegate_task` once. Set `maxOutputTokens` only when a bounded response size is useful.
5. If the tool reports completion with empty text, retry once with a shorter, direct instruction.
6. Attribute the returned text to Algomim. Clearly separate it from your own assessment or comparison.
7. If the retry is also empty or incomplete, report the tool failure instead of inventing an Algomim response.

Do not delegate actions that modify external state. The tool is for analysis, review, and content generation.
