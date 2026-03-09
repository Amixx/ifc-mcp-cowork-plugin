# /load-ifc

Load an IFC model into MCP session and return a concise overview.

Usage:

```text
/load-ifc /absolute/path/to/model.ifc
```

Execution steps:

1. Call `mcp__ifc_mcp__load_model(file_path="<path>")`
2. Call `mcp__ifc_mcp__get_model_summary()`
3. Call `mcp__ifc_mcp__get_spatial_structure()`

Response format:

Present a brief summary — no narration, no filler. Just the facts:

- File path
- Project/model label (if available)
- Schema version
- Top 5 element types by count (table)
- Storey list

On failure: show the error and suggest checking path/permissions. Nothing more.
