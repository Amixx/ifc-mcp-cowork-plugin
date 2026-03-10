# /ifc-version

Show exactly which `ifc-mcp` server build is running in this session.

Usage:

```text
/ifc-version
```

Execution steps:

1. Call `mcp__ifc_mcp__get_server_info()`
2. Call `mcp__ifc_mcp__get_loaded_model()`

Response format:

- Server name
- Server version
- Python version
- Loaded model path (or none)
- Geometry loaded flag
- Cached model count

Keep it concise and factual. This command is for debugging version/config drift.
