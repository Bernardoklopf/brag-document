---
name: generate-excalidraw
description: Generate Excalidraw diagram files (.excalidraw) programmatically as JSON. Use this skill whenever the user asks to create diagrams, flowcharts, presentations, architecture diagrams, or any visual content in Excalidraw format. Also trigger on "draw a diagram", "create a flowchart", "visualize this", "make an excalidraw", or any request for visual/diagrammatic output that could be rendered in Excalidraw.
allowed-tools: Read, Write, Bash, Glob
---

# Generate Excalidraw Files

Prefer generating Excalidraw files via Python script using the bundled generator library. This avoids manual coordinate errors, guarantees unique IDs/indices, and makes diagrams easy to modify. For very small diagrams (< 10 elements), writing JSON directly is acceptable as a fallback.

## Workflow — Python Generator (Recommended)

### Generator library

Use the reusable helper at `.cursor/skills/generate-excalidraw/scripts/generator.py`. It provides:

| Function | Purpose |
|----------|---------|
| `make_rect(eid, x, y, w, h, bg, bound_elements)` | Rectangle with rounded corners |
| `make_ellipse(eid, x, y, w, h, bg, bound_elements)` | Ellipse/circle |
| `make_diamond(eid, x, y, w, h, bg, bound_elements)` | Diamond shape |
| `make_text(eid, x, y, w, h, text, font_size, ...)` | Text (standalone or bound) |
| `make_arrow(eid, x, y, points, start_binding, end_binding)` | Arrow connector |
| `make_line(eid, x, y, points)` | Line (no arrowheads) |
| `binding(element_id)` | Create arrow binding reference |
| `center_text_in_shape(sx, sy, sw, sh, tw, th)` | Calculate centered text position |
| `estimate_text_size(text, font_size)` | Estimate (width, height) for text |
| `build_document(elements)` | Build top-level Excalidraw JSON |
| `write_file(doc, filepath)` | Write file + assert unique IDs |

### Step-by-step

1. Create a Python script next to the target `.excalidraw` file
2. Import helpers from `generator.py`
3. Build elements list: shapes first, then bound text, then arrows
4. For bound text: set `container_id` on text AND `bound_elements` on shape
5. For arrows: set `start_binding`/`end_binding` AND add arrow to shapes' `bound_elements`
6. Call `build_document(elements)` and `write_file(doc, path)`
7. Run validation
8. **Keep the .py file** alongside the .excalidraw for future edits

### Example

```python
import sys, os
sys.path.insert(0, os.path.join(os.path.dirname(__file__), '.cursor/skills/generate-excalidraw/scripts'))
from generator import *

elements = []

# Shape with bound text
elements.append(make_rect("box1", 0, 0, 200, 80, bg="#a5d8ff",
    bound_elements=[{"type": "text", "id": "box1_text"}]))
tx, ty = center_text_in_shape(0, 0, 200, 80, 120, 25)
elements.append(make_text("box1_text", tx, ty, 120, 25, "Hello World",
    font_size=20, text_align="center", vertical_align="middle",
    container_id="box1"))

doc = build_document(elements)
write_file(doc, "output.excalidraw")
```

## Validation

After generating an `.excalidraw` file (either method), run:

```bash
python3 .cursor/skills/generate-excalidraw/scripts/validate.py <path-to-file.excalidraw>
```

The script checks: valid JSON structure, unique IDs/indices, no `isDeleted: true` on visible elements, bound text/shape cross-references consistency, and arrow binding/boundElements consistency.

**Always run validation after generating a file.** Fix any errors before delivering.

## Excalidraw Reference

For detailed element templates, properties, colors, fonts, and layout guidelines, read the reference files below. These contain the full JSON schema for each element type (rectangle, ellipse, diamond, text, arrow, line), binding mechanics, centering formulas, color palette, and font family values.

## Reference Files

| Purpose | Path |
|---------|------|
| Element templates (full JSON) | `.cursor/skills/generate-excalidraw/element-templates.md` |
| Generator library | `.cursor/skills/generate-excalidraw/scripts/generator.py` |
| Validation script | `.cursor/skills/generate-excalidraw/scripts/validate.py` |
