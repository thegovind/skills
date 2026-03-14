# Properties (Frontmatter) Reference

Properties use YAML frontmatter at the start of a note:

```yaml
---
title: My Note Title
date: 2024-01-15
tags:
  - project
  - important
aliases:
  - My Note
  - Alternative Name
cssclasses:
  - custom-class
status: in-progress
rating: 4.5
completed: false
due: 2024-02-01T14:30:00
---
```

## Property Types

| Type | Example |
|------|---------|
| Text | `title: My Title` |
| Number | `rating: 4.5` |
| Checkbox | `completed: true` / `completed: false` |
| Date | `date: 2024-01-15` |
| Date & Time | `due: 2024-01-15T14:30:00` |
| List | `tags: [one, two]` or YAML list syntax |
| Links | `related: "[[Other Note]]"` |

## Default Properties

- **`tags`** — Searchable labels, shown in graph view and tag pane
- **`aliases`** — Alternative names for the note (used in link suggestions and search)
- **`cssclasses`** — CSS classes applied to the note in reading/editing view

## Tags

### Inline Tags

```markdown
#tag
#nested/tag
#tag-with-dashes
#tag_with_underscores
```

### Frontmatter Tags

```yaml
---
tags:
  - tag1
  - nested/tag2
---
```

### Tag Rules

Tags can contain:
- Letters (any language/script)
- Numbers (not as first character)
- Underscores `_`
- Hyphens `-`
- Forward slashes `/` (for nesting)

Tags **cannot** contain spaces or other special characters.
