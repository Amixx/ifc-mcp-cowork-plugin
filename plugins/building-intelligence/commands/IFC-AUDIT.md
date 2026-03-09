# /ifc-audit

Run a model quality audit on the currently loaded IFC model and produce a readable report.

Usage:

```text
/ifc-audit
```

Execution steps:

1. Call `mcp__ifc_mcp__get_loaded_model()`
2. If no model is loaded, stop and instruct user to run:
   - `/load-ifc /absolute/path/to/model.ifc`
3. Gather data (call in parallel where possible):
   - `mcp__ifc_mcp__get_model_summary()`
   - `mcp__ifc_mcp__get_spatial_structure()`
   - `mcp__ifc_mcp__get_space_summary()`
   - `mcp__ifc_mcp__get_material_summary()`
   - `mcp__ifc_mcp__list_property_sets()`
4. Run targeted checks:
   - `mcp__ifc_mcp__find_elements_by_property(property_name="FireRating", operator="exists")` — to count how many elements have fire ratings
   - `mcp__ifc_mcp__search_elements(ifc_class="IfcWall")` — check walls exist
   - `mcp__ifc_mcp__search_elements(ifc_class="IfcDoor")` — check doors exist
   - `mcp__ifc_mcp__search_elements(ifc_class="IfcWindow")` — check windows exist
   - `mcp__ifc_mcp__search_elements(ifc_class="IfcSpace")` — check spaces are defined

Report sections:

1. **Model Identity** — file name, schema version, project name, authoring app if available
2. **Element Overview** — total element count, top classes by count (table)
3. **Spatial Completeness** — are storeys defined? are spaces defined? do spaces have area/volume data?
4. **Material Coverage** — how many elements have materials assigned vs total
5. **Property Richness** — number of property sets, highlight if very few
6. **Fire Rating Check** — how many elements carry a FireRating property, flag if none
7. **Overall Health Score** — a simple 0–100 score based on:
   - Storeys defined: +15
   - Spaces defined: +15
   - Spaces have area data: +15
   - Materials assigned to >50% of elements: +15
   - Fire ratings present on any element: +10
   - More than 5 property sets: +10
   - Model has walls, doors, and windows: +10
   - At least 2 storeys: +10

Style:

- Use a clear header: "🏗️ IFC Model Audit Report"
- Use tables for element counts and space summaries
- Use ✅ / ⚠️ / ❌ icons for each check
- Round quantities to 2 decimals
- End with 1–2 sentence plain-language summary of model quality
