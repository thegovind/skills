---
name: obsidian
description: >
  Interact with Obsidian vaults using the CLI, create Obsidian Flavored Markdown,
  build Bases (.base) databases, and author JSON Canvas (.canvas) files.
  Use when the user wants to read/write/search vault notes, manage tasks or properties,
  automate vault workflows, create .base or .canvas files, or write Obsidian-specific
  markdown with wikilinks, callouts, and embeds.
  Triggers: "obsidian", "vault", "daily note", "wikilink", "callout", ".base file",
  ".canvas file", "obsidian cli", "obsidian markdown", "obsidian bases".
---

# Obsidian

Skill for working with Obsidian vaults — CLI operations, Obsidian Flavored Markdown,
Bases databases, and JSON Canvas files.

## Obsidian CLI

The official CLI (v1.12+) controls a running Obsidian desktop instance via IPC.

| Requirement | Details |
|---|---|
| Obsidian Desktop | **v1.12.0+** |
| CLI enabled | Settings → General → Command line interface → ON |
| Obsidian running | Desktop app **must be running** (IPC) |

### Syntax

```bash
obsidian <command> [subcommand] [key=value ...] [flags]
```

- **Parameters**: `key=value` (quote values with spaces)
- **Flags**: boolean switches, no value (e.g., `open`, `overwrite`)
- **Vault targeting**: `vault="My Vault"` as first argument
- **File targeting**: `file=<name>` (wikilink-style) or `path=<vault-relative-path>`

### Core Commands

```bash
# Read & write notes
obsidian read path="folder/note.md"
obsidian create path="folder/note" content="# Title\n\nBody"
obsidian create path="folder/note" template="meeting-notes"
obsidian append path="folder/note.md" content="New paragraph"
obsidian prepend path="folder/note.md" content="Top content"
obsidian move path="old/note.md" to="new/note.md"
obsidian delete path="folder/note.md"

# Daily notes
obsidian daily                          # Open today's daily note
obsidian daily:read                     # Print content to stdout
obsidian daily:append content="- [ ] New task"

# Search
obsidian search query="project alpha"
obsidian search query="TODO" path="projects" limit=10
obsidian search:context query="meeting"  # Returns matching lines

# Properties & tags
obsidian properties path="note.md"
obsidian property:set path="note.md" name="status" value="active"
obsidian tags counts sort=count
obsidian tag name="project/alpha"

# Tasks
obsidian tasks todo                     # Incomplete tasks
obsidian tasks daily                    # Tasks in today's daily note
obsidian task path="note.md" line=12 toggle

# Links & graph
obsidian backlinks path="note.md"
obsidian orphans                        # Notes with no links
obsidian unresolved                     # Broken wikilinks

# Developer
obsidian eval code="app.vault.getFiles().length"
obsidian dev:screenshot path="screenshot.png"
obsidian plugin:reload id=my-plugin
obsidian dev:errors
```

### Command Groups (130+ commands)

| Group | Key Commands | Purpose |
|---|---|---|
| **files** | `read`, `create`, `append`, `prepend`, `move`, `rename`, `delete`, `files` | Note CRUD |
| **daily** | `daily`, `daily:read`, `daily:append`, `daily:prepend` | Daily notes |
| **search** | `search`, `search:context` | Full-text search |
| **properties** | `properties`, `property:set`, `property:remove`, `property:read` | Frontmatter |
| **tags** | `tags`, `tag` | Tag listing and filtering |
| **tasks** | `tasks`, `task` | Task querying and toggling |
| **links** | `backlinks`, `links`, `unresolved`, `orphans`, `deadends` | Graph analysis |
| **templates** | `templates`, `template:read`, `template:insert` | Template operations |
| **plugins** | `plugins`, `plugin:enable`, `plugin:install`, `plugin:reload` | Plugin management |
| **sync** | `sync`, `sync:status`, `sync:history`, `sync:restore` | Obsidian Sync |
| **bases** | `bases`, `base:query`, `base:views`, `base:create` | Bases database |
| **dev** | `eval`, `dev:screenshot`, `dev:console`, `dev:errors`, `dev:dom` | Developer tools |
| **vault** | `vault`, `vaults`, `version`, `reload`, `restart` | Vault/app control |

### Tips

1. **Paths are vault-relative** — use `folder/note.md`, not absolute paths.
2. **`create` omits `.md`** — extension is added automatically.
3. **`prepend` inserts after frontmatter**, not at byte 0.
4. **Pipe-friendly** — plain text output works with `grep`, `awk`, `jq`.
5. **`format=json`** on `search` returns a JSON array of file paths.
6. **`eval` requires single-line JS** — for multiline, write to a temp file and use `$(cat /tmp/obs.js)`.
7. **`property:set` stores values as strings** — for YAML arrays, edit frontmatter directly or use `eval`.

### Agent Patterns

```bash
# Daily journal automation
obsidian daily:append content="## $(date '+%H:%M') — Status Update\n- Done: merged PR\n- Next: code review"

# Create note from template with metadata
obsidian create path="projects/new-feature" template="project-template"
obsidian property:set path="projects/new-feature.md" name="status" value="planning"

# Vault analytics
obsidian files total
obsidian tags counts sort=count
obsidian orphans
obsidian unresolved

# Plugin dev/test cycle
obsidian plugin:reload id=my-plugin
obsidian dev:errors
obsidian dev:screenshot path=screenshot.png
```

---

## Obsidian Flavored Markdown

Obsidian extends CommonMark/GFM with wikilinks, embeds, callouts, properties, comments, and highlights.

### Internal Links (Wikilinks)

```markdown
[[Note Name]]                          Link to note
[[Note Name|Display Text]]             Custom display text
[[Note Name#Heading]]                  Link to heading
[[Note Name#^block-id]]                Link to block
```

### Embeds

```markdown
![[Note Name]]                         Embed full note
![[image.png]]                         Embed image
![[image.png|300]]                     Image with width
![[document.pdf#page=3]]               PDF page
```

### Callouts

```markdown
> [!note]
> Basic callout.

> [!warning] Custom Title
> Callout with custom title.

> [!faq]- Collapsed by default
> Foldable callout.
```

Types: `note`, `tip`, `warning`, `info`, `example`, `quote`, `bug`, `danger`, `success`, `failure`, `question`, `abstract`, `todo`.

### Properties (Frontmatter)

```yaml
---
title: My Note
date: 2024-01-15
tags:
  - project
  - active
aliases:
  - Alternative Name
---
```

### Other Syntax

```markdown
==Highlighted text==                   Highlight
%%Hidden comment%%                     Comment (hidden in reading view)
#tag #nested/tag                       Inline tags
$e^{i\pi} + 1 = 0$                    LaTeX math
```

---

## Obsidian Bases

Bases (v1.12+) are `.base` files containing YAML that create database views of vault notes.

### Workflow

1. Create a `.base` file with YAML content
2. Define `filters` to select notes (by tag, folder, property)
3. Add `formulas` for computed properties
4. Configure `views` (table, cards, list, map)
5. Validate YAML — check quoting, formula references

### Quick Example

```yaml
filters:
  and:
    - file.hasTag("task")
    - 'file.ext == "md"'

formulas:
  days_until_due: 'if(due, (date(due) - today()).days, "")'
  priority_label: 'if(priority == 1, "🔴 High", if(priority == 2, "🟡 Medium", "🟢 Low"))'

views:
  - type: table
    name: "Active Tasks"
    filters:
      and:
        - 'status != "done"'
    order:
      - file.name
      - status
      - formula.priority_label
      - due
      - formula.days_until_due
```

### Key Rules

- Use single quotes for formulas containing double quotes: `'if(done, "Yes", "No")'`
- Duration math requires field access: `(now() - file.ctime).days` not `(now() - file.ctime).round(0)`
- Guard nullable properties: `'if(due_date, (date(due_date) - today()).days, "")'`

---

## JSON Canvas

Canvas files (`.canvas`) follow the [JSON Canvas Spec 1.0](https://jsoncanvas.org/spec/1.0/).

### Structure

```json
{
  "nodes": [],
  "edges": []
}
```

### Node Types

| Type | Required Fields | Purpose |
|------|----------------|---------|
| `text` | `text` | Markdown content |
| `file` | `file` | Vault file embed |
| `link` | `url` | External URL |
| `group` | — | Visual container |

All nodes require: `id` (16-char hex), `type`, `x`, `y`, `width`, `height`.

### Edges

Connect nodes via `fromNode`/`toNode` IDs. Optional: `fromSide`/`toSide` (`top`, `right`, `bottom`, `left`), `label`, `color`.

### Validation

1. All `id` values unique across nodes and edges
2. Every `fromNode`/`toNode` references an existing node
3. Required fields present per node type
4. JSON is valid and parseable

---

## Reference Files

| File | Contents |
|------|----------|
| [references/cli-commands.md](references/cli-commands.md) | Full 130+ command reference with parameters |
| [references/bases-guide.md](references/bases-guide.md) | Bases schema, filters, views, complete examples |
| [references/bases-functions.md](references/bases-functions.md) | All Bases functions by type (Date, String, Number, List, File) |
| [references/json-canvas.md](references/json-canvas.md) | JSON Canvas spec, node/edge details, layout guidelines, examples |
| [references/markdown-syntax.md](references/markdown-syntax.md) | Full Obsidian Flavored Markdown reference |
| [references/callouts.md](references/callouts.md) | All callout types with aliases and CSS customization |
| [references/embeds.md](references/embeds.md) | Embed syntax for notes, images, audio, PDF, search |
| [references/properties.md](references/properties.md) | Frontmatter property types and tag rules |
