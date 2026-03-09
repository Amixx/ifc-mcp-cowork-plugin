# ifc-mcp-cowork-plugin

A [Claude Code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview) plugin that lets you explore and query IFC building models using natural language — powered by the [`ifc-mcp`](https://github.com/amixx/ifc-mcp) MCP server.

## Features

- **Load & inspect** IFC models directly from your Claude Code session
- **Spatial queries** — storeys, spaces, containment, connectivity
- **Element search** — by class, property, classification, or type
- **Quantity takeoffs** — areas, volumes, material breakdowns
- **Slash commands** — `/load-ifc` and `/ifc-summary` for quick workflows
- **IFC Explorer skill** — guides Claude through multi-step analysis tasks

## Prerequisites

- [uv](https://docs.astral.sh/uv/getting-started/installation/) — the MCP server is run via `uvx`, so no manual install of `ifc-mcp` is needed.

## Installation

Install the plugin from the Claude Code marketplace or add it manually:

```bash
claude plugin add amixx/ifc-mcp-cowork-plugin
```

## Usage

Load a model and start exploring:

```
/load-ifc /path/to/model.ifc
```

Get a full report of the loaded model:

```
/ifc-summary
```

Or just ask questions naturally — the IFC Explorer skill activates automatically:

- *"How much floor area is on Level 2?"*
- *"Find all fire-rated walls"*
- *"What materials are used in this building?"*

```

## License

[MIT](LICENSE)
