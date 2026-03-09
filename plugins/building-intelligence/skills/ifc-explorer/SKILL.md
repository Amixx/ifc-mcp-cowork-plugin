# IFC Explorer

Use this skill when the user wants to load, inspect, query, compare, or analyze IFC building model files.

## Trigger cues

- "Open this IFC"
- "Analyze this building model"
- "Find all walls/doors/windows"
- "How much floor area/material volume"
- "Show structure/storeys/spaces"

## Workflow

1. Check session state first:
   - `mcp__ifc_mcp__get_loaded_model` — also tells you if geometry is loaded (`geometry_loaded`)
2. If no model is loaded (or user wants another file), load one:
   - `mcp__ifc_mcp__load_model(file_path="/absolute/path/to/model.ifc")`
   - **Do NOT pass `with_geometry=True`** unless the task specifically needs bounding boxes or geometry-derived volumes. Loading without geometry is 10x+ faster.
3. Start orientation with:
   - `mcp__ifc_mcp__get_model_summary`
4. Then drill down with the right tool group.
5. `get_element_geometry_bounds` computes bounds on demand and caches results. Reload with `with_geometry=True` only for geometry-heavy volume workflows.

Most tools also accept optional `file_path` to run directly against a specific file without changing current session state.

## Geometry loading

Geometry is **off by default** for fast loading. Most tools work without it. Only reload with geometry when actually needed.

**Geometry-sensitive behavior:**

- `get_element_geometry_bounds` — computes bounds on demand and caches them; response includes `source` (`on_demand`/`cached`/`missing`).
- `get_quantities` / `get_material_summary` — volume values may be under-reported for elements lacking quantity sets unless model was loaded with geometry (bbox fallback then available).

**All other tools work fully without geometry** — spatial structure, search, properties, relationships, classifications, space summaries, etc.

**When to load geometry:**

- User needs many volume-heavy rollups where geometry bbox fallback should be available model-wide
- User asks repeated geometry queries across many elements and wants lower per-call latency
- Never load geometry "just in case" — it's expensive

## Tool group usage

### Session

- `load_model`: Set/switch active IFC model.
- `get_loaded_model`: Check active model and cache.
- `unload_model`: Clear active model.

### Spatial

Use when user asks where elements are, how storeys/spaces are organized, or what is in a room.

- `get_spatial_structure`
- `get_elements_in_space`

### Query

Use for element lookup and filtered search.

- `get_element_by_id`
- `search_elements`
- `get_element_properties`

### Relationships

Use for connectivity, containment, host chains, materials per element.

- `get_connected_elements`
- `get_contained_elements`
- `get_element_material`

### Quantities

Use for totals and rollups.

- `get_quantities`
- `get_material_summary`
- `get_space_summary`

### Analysis

Use for semantic filtering and classification/type context.

- `find_elements_by_property`
- `get_classification`
- `get_type_info`

### Meta

Use for model-wide orientation and discovery.

- `get_model_summary`
- `list_property_sets`
- `get_element_geometry_bounds`

## Common patterns

- "How many m2 of floor space?"
  - `get_space_summary` then sum/compare area by floor.
- "Find fire-rated walls"
  - `find_elements_by_property(property_name="FireRating", operator="exists"|"equals")`
  - then `search_elements(ifc_class="IfcWall")` as needed.
- "What is this element?"
  - `get_element_by_id` then `get_type_info` and `get_element_material`.
- "What is on Level 2?"
  - `search_elements(floor="Level 2")` and `get_space_summary(floor="Level 2")`.

## Response style

**Be concise. No filler. No AI slop.** Every sentence should carry information the user actually needs.

- Lead with the answer, not the process. Do not narrate what tools you called or explain your reasoning unless the user asks.
- Interpret results — never dump raw JSON or repeat tool output verbatim.
- Use compact tables for counts, areas, volumes, and type breakdowns.
- Round quantities to 2 decimals unless precision matters.
- Include GlobalIds only when listing specific elements for traceability.
- Flag missing data directly (e.g. "No spaces defined", "No classifications found").
- No filler phrases ("Let me analyze...", "Here's what I found...", "Based on the data..."). Just present the information.
- Write like a professional consultant's report: structured, factual, scannable.
