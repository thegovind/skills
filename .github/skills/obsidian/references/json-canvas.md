# JSON Canvas Reference

Canvas files (`.canvas`) follow the [JSON Canvas Spec 1.0](https://jsoncanvas.org/spec/1.0/).

## File Structure

```json
{
  "nodes": [],
  "edges": []
}
```

## Nodes

Array order determines z-index: first = bottom, last = top.

### Common Attributes (All Nodes)

| Attribute | Required | Type | Description |
|-----------|----------|------|-------------|
| `id` | Yes | string | Unique 16-char hex identifier |
| `type` | Yes | string | `text`, `file`, `link`, or `group` |
| `x` | Yes | integer | X position in pixels |
| `y` | Yes | integer | Y position in pixels |
| `width` | Yes | integer | Width in pixels |
| `height` | Yes | integer | Height in pixels |
| `color` | No | canvasColor | Preset `"1"`-`"6"` or hex `"#FF0000"` |

### Text Nodes

| Attribute | Required | Description |
|-----------|----------|-------------|
| `text` | Yes | Plain text with Markdown syntax |

```json
{
  "id": "6f0ad84f44ce9c17",
  "type": "text",
  "x": 0, "y": 0, "width": 400, "height": 200,
  "text": "# Hello World\n\nThis is **Markdown** content."
}
```

Use `\n` for line breaks. Do **not** use literal `\\n`.

### File Nodes

| Attribute | Required | Description |
|-----------|----------|-------------|
| `file` | Yes | Path to file within vault |
| `subpath` | No | Link to heading or block (`#heading`) |

```json
{
  "id": "a1b2c3d4e5f67890",
  "type": "file",
  "x": 500, "y": 0, "width": 400, "height": 300,
  "file": "Attachments/diagram.png"
}
```

### Link Nodes

| Attribute | Required | Description |
|-----------|----------|-------------|
| `url` | Yes | External URL |

```json
{
  "id": "c3d4e5f678901234",
  "type": "link",
  "x": 1000, "y": 0, "width": 400, "height": 200,
  "url": "https://obsidian.md"
}
```

### Group Nodes

Visual containers for organizing other nodes. Position children inside bounds.

| Attribute | Required | Description |
|-----------|----------|-------------|
| `label` | No | Text label |
| `background` | No | Path to background image |
| `backgroundStyle` | No | `cover`, `ratio`, or `repeat` |

```json
{
  "id": "d4e5f6789012345a",
  "type": "group",
  "x": -50, "y": -50, "width": 1000, "height": 600,
  "label": "Project Overview",
  "color": "4"
}
```

## Edges

Connect nodes via `fromNode` and `toNode` IDs.

| Attribute | Required | Type | Default | Description |
|-----------|----------|------|---------|-------------|
| `id` | Yes | string | — | Unique identifier |
| `fromNode` | Yes | string | — | Source node ID |
| `fromSide` | No | string | — | `top`, `right`, `bottom`, `left` |
| `fromEnd` | No | string | `none` | `none` or `arrow` |
| `toNode` | Yes | string | — | Target node ID |
| `toSide` | No | string | — | `top`, `right`, `bottom`, `left` |
| `toEnd` | No | string | `arrow` | `none` or `arrow` |
| `color` | No | canvasColor | — | Line color |
| `label` | No | string | — | Text label |

```json
{
  "id": "0123456789abcdef",
  "fromNode": "6f0ad84f44ce9c17",
  "fromSide": "right",
  "toNode": "a1b2c3d4e5f67890",
  "toSide": "left",
  "toEnd": "arrow",
  "label": "leads to"
}
```

## Colors

| Preset | Color |
|--------|-------|
| `"1"` | Red |
| `"2"` | Orange |
| `"3"` | Yellow |
| `"4"` | Green |
| `"5"` | Cyan |
| `"6"` | Purple |

Also accepts hex strings: `"#FF0000"`.

## ID Generation

16-character lowercase hexadecimal strings (64-bit random):

```
"6f0ad84f44ce9c17"
"a3b2c1d0e9f8a7b6"
```

## Layout Guidelines

- Coordinates can be negative (canvas extends infinitely)
- `x` increases right, `y` increases down; position is top-left corner
- Space nodes 50-100px apart; 20-50px padding inside groups
- Align to multiples of 10 or 20

| Node Type | Width | Height |
|-----------|-------|--------|
| Small text | 200-300 | 80-150 |
| Medium text | 300-450 | 150-300 |
| Large text | 400-600 | 300-500 |
| File preview | 300-500 | 200-400 |
| Link preview | 250-400 | 100-200 |

## Workflows

### Create a New Canvas

1. Create `.canvas` file with `{"nodes": [], "edges": []}`
2. Generate unique 16-char hex IDs for each node
3. Add nodes with required fields
4. Add edges referencing valid node IDs
5. Validate: parse JSON, verify all edge references

### Add Node to Existing Canvas

1. Read and parse existing `.canvas`
2. Generate unique ID (no collisions)
3. Position to avoid overlaps (50-100px spacing)
4. Append to `nodes` array
5. Optionally add connecting edges
6. Validate all IDs and references

### Connect Two Nodes

1. Identify source and target node IDs
2. Generate unique edge ID
3. Set `fromNode`, `toNode`, and optionally `fromSide`/`toSide`
4. Append to `edges` array

## Validation Checklist

1. All `id` values unique across nodes and edges
2. Every `fromNode`/`toNode` references existing node
3. Required fields present per node type
4. `type` is `text`, `file`, `link`, or `group`
5. `fromSide`/`toSide` is `top`, `right`, `bottom`, or `left`
6. `fromEnd`/`toEnd` is `none` or `arrow`
7. Colors are `"1"`-`"6"` or valid hex
8. JSON is valid and parseable

## Examples

### Simple Mind Map

```json
{
  "nodes": [
    {"id": "8a9b0c1d2e3f4a5b", "type": "text", "x": 0, "y": 0, "width": 300, "height": 150, "text": "# Main Idea\n\nCentral concept."},
    {"id": "1a2b3c4d5e6f7a8b", "type": "text", "x": 400, "y": -100, "width": 250, "height": 100, "text": "## Point A\n\nDetails."},
    {"id": "2b3c4d5e6f7a8b9c", "type": "text", "x": 400, "y": 100, "width": 250, "height": 100, "text": "## Point B\n\nMore details."}
  ],
  "edges": [
    {"id": "3c4d5e6f7a8b9c0d", "fromNode": "8a9b0c1d2e3f4a5b", "fromSide": "right", "toNode": "1a2b3c4d5e6f7a8b", "toSide": "left"},
    {"id": "4d5e6f7a8b9c0d1e", "fromNode": "8a9b0c1d2e3f4a5b", "fromSide": "right", "toNode": "2b3c4d5e6f7a8b9c", "toSide": "left"}
  ]
}
```

### Project Board with Groups

```json
{
  "nodes": [
    {"id": "5e6f7a8b9c0d1e2f", "type": "group", "x": 0, "y": 0, "width": 300, "height": 500, "label": "To Do", "color": "1"},
    {"id": "6f7a8b9c0d1e2f3a", "type": "group", "x": 350, "y": 0, "width": 300, "height": 500, "label": "In Progress", "color": "3"},
    {"id": "7a8b9c0d1e2f3a4b", "type": "group", "x": 700, "y": 0, "width": 300, "height": 500, "label": "Done", "color": "4"},
    {"id": "8b9c0d1e2f3a4b5c", "type": "text", "x": 20, "y": 50, "width": 260, "height": 80, "text": "## Task 1\n\nImplement feature X"},
    {"id": "9c0d1e2f3a4b5c6d", "type": "text", "x": 370, "y": 50, "width": 260, "height": 80, "text": "## Task 2\n\nReview PR #123", "color": "2"},
    {"id": "0d1e2f3a4b5c6d7e", "type": "text", "x": 720, "y": 50, "width": 260, "height": 80, "text": "## Task 3\n\n~~Setup CI/CD~~"}
  ],
  "edges": []
}
```
