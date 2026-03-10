# IntelliFence — IFC MCP Cowork Plugin

## Project overview

IntelliFence is a Claude Cowork / Claude Code plugin for exploring IFC building models via natural language. It wraps the [`ifc-mcp`](https://github.com/amixx/ifc-mcp) MCP server.

## Repository structure

- `plugins/intellifence/` — the plugin itself (commands, skills, MCP config)
- `showcase/` — static HTML demos deployed to GitHub Pages
- `docs/` — installation guides and documentation

## Key conventions

- Plugin commands live in `plugins/intellifence/commands/` as Markdown files.
- Skills live in `plugins/intellifence/skills/` with a `SKILL.md` and optional `references/`.
- The MCP server config is in `plugins/intellifence/.mcp.json`.
- Showcase reports are self-contained single-file HTML pages (inline CSS/JS, no external assets beyond CDN libs).

## Development notes

- GitHub Pages deploys the `showcase/` directory via `.github/workflows/pages.yml`.
- IFC files (`*.ifc`) are gitignored — never commit them.
