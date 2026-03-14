# Acceptance Criteria: obsidian

**Skill**: `obsidian`
**Source**: Obsidian Help Docs (https://help.obsidian.md/cli), JSON Canvas Spec, Obsidian Bases Docs
**Purpose**: Skill testing acceptance criteria

---

## 1. CLI Command Patterns

### 1.1 ✅ CORRECT: Read a note

```bash
obsidian read path="folder/note.md"
```

### 1.2 ✅ CORRECT: Create with template

```bash
obsidian create path="projects/new-feature" template="project-template"
```

### 1.3 ❌ INCORRECT: Create with .md extension

```bash
obsidian create path="projects/new-feature.md" content="# Title"
# Wrong — create appends .md automatically
```

### 1.4 ✅ CORRECT: Daily note append

```bash
obsidian daily:append content="- [ ] New task"
```

### 1.5 ❌ INCORRECT: Absolute filesystem path

```bash
obsidian read path="/Users/me/vault/note.md"
# Wrong — paths must be vault-relative
```

### 1.6 ✅ CORRECT: Search with JSON output

```bash
obsidian search query="meeting" format=json
```

### 1.7 ✅ CORRECT: Property set

```bash
obsidian property:set path="note.md" name="status" value="active"
```

### 1.8 ❌ INCORRECT: Property set for array values

```bash
obsidian property:set path="note.md" name="tags" value="[tag1, tag2]"
# Wrong approach for arrays — stores literal string, not YAML array
# Use: edit frontmatter directly or use eval
```

---

## 2. Obsidian Markdown Patterns

### 2.1 ✅ CORRECT: Wikilinks

```markdown
[[Note Name]]
[[Note Name|Display Text]]
[[Note Name#Heading]]
```

### 2.2 ❌ INCORRECT: Standard links for internal notes

```markdown
[Note Name](Note%20Name.md)
<!-- Wrong — use wikilinks for internal vault notes -->
```

### 2.3 ✅ CORRECT: Image embed with width

```markdown
![[image.png|300]]
```

### 2.4 ❌ INCORRECT: Image embed with wrong syntax

```markdown
![[image.png|width=300]]
<!-- Wrong — use pipe then number: ![[image.png|300]] -->
```

### 2.5 ✅ CORRECT: Callout syntax

```markdown
> [!warning] Important
> This is a warning callout.
```

### 2.6 ❌ INCORRECT: Callout without bracket syntax

```markdown
> **Warning:** This is not a proper callout.
<!-- Wrong — must use > [!type] syntax -->
```

### 2.7 ✅ CORRECT: Foldable callout

```markdown
> [!faq]- Click to expand
> Hidden content.
```

### 2.8 ✅ CORRECT: Frontmatter properties

```yaml
---
title: My Note
tags:
  - project
  - active
aliases:
  - Alt Name
---
```

### 2.9 ❌ INCORRECT: Tags with spaces

```markdown
#my tag
<!-- Wrong — tags cannot contain spaces. Use #my-tag or #my_tag -->
```

### 2.10 ✅ CORRECT: Comment syntax

```markdown
%%This is hidden in reading view%%
```

---

## 3. Bases Patterns

### 3.1 ✅ CORRECT: Base file with filters and formulas

```yaml
filters:
  and:
    - file.hasTag("task")
    - 'file.ext == "md"'

formulas:
  days_until_due: 'if(due, (date(due) - today()).days, "")'

views:
  - type: table
    name: "Tasks"
    order:
      - file.name
      - formula.days_until_due
```

### 3.2 ❌ INCORRECT: Duration without field access

```yaml
formulas:
  age: '(now() - file.ctime).round(0)'
  # Wrong — Duration doesn't support .round() directly
  # Correct: '(now() - file.ctime).days.round(0)'
```

### 3.3 ❌ INCORRECT: Missing null guard

```yaml
formulas:
  days_left: '(date(due_date) - today()).days'
  # Wrong — crashes if due_date is empty
  # Correct: 'if(due_date, (date(due_date) - today()).days, "")'
```

### 3.4 ❌ INCORRECT: Double quotes inside double quotes

```yaml
formulas:
  label: "if(done, "Yes", "No")"
  # YAML error — use single quotes wrapping double quotes
  # Correct: 'if(done, "Yes", "No")'
```

### 3.5 ✅ CORRECT: Referencing formula in views

```yaml
formulas:
  total: "price * quantity"

views:
  - type: table
    order:
      - formula.total
```

### 3.6 ❌ INCORRECT: Referencing undefined formula

```yaml
views:
  - type: table
    order:
      - formula.total
# Wrong — formula.total not defined in formulas section
```

---

## 4. JSON Canvas Patterns

### 4.1 ✅ CORRECT: Canvas structure

```json
{
  "nodes": [
    {"id": "6f0ad84f44ce9c17", "type": "text", "x": 0, "y": 0, "width": 300, "height": 150, "text": "# Hello"}
  ],
  "edges": []
}
```

### 4.2 ❌ INCORRECT: Missing required fields

```json
{
  "nodes": [
    {"type": "text", "text": "Hello"}
  ]
}
```
Missing: `id`, `x`, `y`, `width`, `height`.

### 4.3 ❌ INCORRECT: Dangling edge reference

```json
{
  "nodes": [
    {"id": "aaa", "type": "text", "x": 0, "y": 0, "width": 100, "height": 100, "text": "A"}
  ],
  "edges": [
    {"id": "bbb", "fromNode": "aaa", "toNode": "nonexistent"}
  ]
}
```
`toNode` references a node that doesn't exist.

### 4.4 ❌ INCORRECT: Literal backslash-n in text

```json
{"text": "Line 1\\nLine 2"}
```
Wrong — renders as literal `\n`. Use `\n` (single backslash in JSON string).

### 4.5 ✅ CORRECT: Edge with label and sides

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
