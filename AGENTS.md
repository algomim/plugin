# AGENTS.md

This repository is the provider-neutral Algomim plugin monorepo.

## Structure

- `plugins/<name>/` is one independently installable plugin package.
- `.agents/plugins/marketplace.json` is the Codex marketplace catalog for this repository.
- Client-specific manifests stay inside each plugin package, for example `.codex-plugin/` and `.claude-plugin/`.
- MCP configuration, reusable skills, scripts, and assets should remain client-neutral when their formats are shared.

## Rules

- Never commit credentials. Reference environment-variable names from MCP configuration.
- Keep the plugin folder name, marketplace entry name, and client manifest name aligned.
- Use strict semantic versions for released plugin manifests.
- Keep each delegated tool workflow bounded and include complete supporting context.
- Validate Codex plugins with the bundled `plugin-creator` validator before release.
- Add provider-specific files only when the target client requires them; do not duplicate shared instructions without a compatibility reason.
