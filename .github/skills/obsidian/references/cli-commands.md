# Obsidian CLI — Full Command Reference

Complete reference for all official Obsidian CLI commands (v1.12+).

**Syntax**: `obsidian [vault] <command> [subcommand] [key=value ...] [flags]`

All parameters use `key=value` syntax. Quote values containing spaces: `content="hello world"`.

---

## Table of Contents

1. [Files](#files)
2. [Daily Notes](#daily-notes)
3. [Search](#search)
4. [Properties](#properties)
5. [Tags](#tags)
6. [Tasks](#tasks)
7. [Links](#links)
8. [Bookmarks](#bookmarks)
9. [Templates](#templates)
10. [Plugins](#plugins)
11. [Sync](#sync)
12. [Themes](#themes)
13. [CSS Snippets](#css-snippets)
14. [Commands & Hotkeys](#commands--hotkeys)
15. [Obsidian Bases](#obsidian-bases)
16. [History](#history)
17. [Workspace & Tabs](#workspace--tabs)
18. [Diff](#diff)
19. [Developer](#developer)
20. [Vault & System](#vault--system)
21. [Output Formatting](#output-formatting)

---

## Files

### Reading Notes

```bash
obsidian read path="folder/note.md"
obsidian read file="Recipe"          # Wikilink-style resolution
```

### Creating Notes

```bash
obsidian create path="folder/note" content="# Title\n\nBody text"
obsidian create path="folder/note" template="template-name"
obsidian create name="Note" content="Hello" open overwrite
```

- Path should **not** include `.md` — appended automatically.
- Use `template=` to create from a template file.
- Flags: `overwrite`, `open`, `newtab`.

### Appending & Prepending

```bash
obsidian append path="folder/note.md" content="Appended text"
obsidian prepend path="folder/note.md" content="Prepended text"
```

- `prepend` adds content **after frontmatter**, not at byte 0.
- Flag: `inline` (append without newline).

### Moving & Renaming

```bash
obsidian move path="old/path/note.md" to="new/path/note.md"
obsidian rename path="folder/note.md" name="new-name"
```

- `move` requires full vault-relative target path including `.md`.
- `rename` takes just the new filename (no path, no extension).

### Deleting

```bash
obsidian delete path="folder/note.md"           # Moves to trash
obsidian delete path="folder/note.md" permanent  # Permanent
```

### File Discovery

```bash
obsidian files                       # List all files
obsidian files ext=md                # Filter by extension
obsidian files folder="subfolder"    # Files in folder
obsidian files total                 # File count
obsidian folders                     # List all folders
obsidian file path="folder/note.md"  # File info (size, created, modified)
obsidian random                      # Open random note
obsidian random:read                 # Print random note content
```

---

## Daily Notes

Requires Daily Notes core plugin enabled.

```bash
obsidian daily                           # Open today's note
obsidian daily:read                      # Print content to stdout
obsidian daily:append content="text"     # Append to today's note
obsidian daily:prepend content="text"    # Prepend (after frontmatter)
obsidian daily:path                      # Print vault-relative path
```

- If today's note doesn't exist, `daily` creates it (using configured template).
- `daily:append`/`daily:prepend` accept `inline` and `open` flags.

---

## Search

```bash
obsidian search query="search text"
obsidian search query="text" path="folder"         # Scope to folder
obsidian search query="text" limit=10               # Limit results
obsidian search query="text" format=json            # JSON array of file paths
obsidian search query="text" case                   # Case-sensitive
obsidian search:context query="text"                # Returns matching lines
obsidian search:open query="text"                   # Open search panel in UI
```

---

## Properties

```bash
obsidian properties path="note.md"                              # All properties
obsidian property:read path="note.md" name="status"             # Read single
obsidian property:set path="note.md" name="status" value="active"  # Set value
obsidian property:remove path="note.md" name="draft"            # Remove
obsidian aliases path="note.md"                                 # List aliases
```

> **Note:** `property:set` stores values as strings. `value="[a, b]"` writes a literal string, not a YAML array. For array properties, edit frontmatter directly or use `eval`.

---

## Tags

```bash
obsidian tags                          # List all tags
obsidian tags counts                   # With usage counts
obsidian tags counts sort=count        # Sorted by frequency
obsidian tags path="note.md"           # Tags in a specific file
obsidian tag name="project/alpha"      # Notes with a specific tag
```

---

## Tasks

### Querying

```bash
obsidian tasks                         # All tasks (done + todo)
obsidian tasks todo                    # Incomplete only
obsidian tasks done                    # Completed only
obsidian tasks daily                   # Tasks in today's daily note
obsidian tasks path="note.md"          # Tasks in specific file
obsidian tasks verbose                 # Group by file with line numbers
```

### Toggling

```bash
obsidian task path="note.md" line=12 toggle
obsidian task ref="note.md:8" toggle
obsidian task daily line=3 done
obsidian task file=Recipe line=8 status=-   # Custom status character
```

---

## Links

```bash
obsidian backlinks path="note.md"         # Notes linking TO this note
obsidian backlinks path="note.md" counts  # With link counts
obsidian links path="note.md"             # Outgoing links FROM this note
obsidian unresolved                        # All unresolved wikilinks
obsidian unresolved verbose                # With source files
obsidian orphans                           # Notes with no incoming links
obsidian deadends                          # Notes with no outgoing links
```

---

## Bookmarks

```bash
obsidian bookmarks                                      # List all
obsidian bookmark file="folder/note.md"                 # Bookmark a note
obsidian bookmark file="note.md" subpath="#Heading"     # Bookmark a heading
obsidian bookmark folder="projects"                     # Bookmark a folder
obsidian bookmark search="query" title="My Search"      # Bookmark a search
obsidian bookmark url="https://example.com" title="Link" # Bookmark a URL
```

---

## Templates

```bash
obsidian templates                                      # List templates
obsidian template:read name="weekly-review"             # Read template
obsidian template:read name="weekly" resolve title="My Note"  # Render with variables
obsidian template:insert name="weekly-review"           # Insert into active file
```

> **Note:** `template:insert` inserts into the currently active file in the UI. It does not accept `path=`. To create a new file from a template, use `obsidian create path="..." template="..."`.

---

## Plugins

```bash
obsidian plugins                         # List all plugins
obsidian plugins:enabled                 # Enabled only
obsidian plugins versions                # With version numbers
obsidian plugin id="dataview"            # Plugin info
obsidian plugin:enable id="canvas"
obsidian plugin:disable id="canvas"
obsidian plugin:install id="dataview"
obsidian plugin:uninstall id="dataview"
obsidian plugin:reload id="my-plugin"    # Reload (dev)
obsidian plugins:restrict on|off         # Toggle restricted mode
```

---

## Sync

Requires active Obsidian Sync subscription.

```bash
obsidian sync                                   # Status summary
obsidian sync on|off                            # Resume/pause
obsidian sync:status                            # Detailed status
obsidian sync:history path="note.md"            # Version history
obsidian sync:read path="note.md" version=3     # Read version
obsidian sync:restore path="note.md" version=3  # Restore version
obsidian sync:deleted                           # Deleted files
obsidian sync:open                              # Open Sync UI
```

---

## Themes

```bash
obsidian themes                            # List installed
obsidian themes versions                   # With versions
obsidian theme                             # Active theme
obsidian theme:set name="Minimal"          # Switch theme
obsidian theme:set name=""                 # Default theme
obsidian theme:install name="Minimal"
obsidian theme:install name="Minimal" enable
obsidian theme:uninstall name="Minimal"
```

---

## CSS Snippets

```bash
obsidian snippets                          # List all
obsidian snippets:enabled                  # Enabled only
obsidian snippet:enable name="my-style"
obsidian snippet:disable name="my-style"
```

---

## Commands & Hotkeys

```bash
obsidian commands                          # List all command IDs
obsidian commands filter="canvas"          # Filter by prefix
obsidian command id="app:reload"           # Execute a command
obsidian hotkeys                           # List all hotkeys
obsidian hotkey id="app:open-settings"     # Get hotkey for command
```

---

## Obsidian Bases

```bash
obsidian bases                                    # List .base files
obsidian base:query file="tasks" format=json      # Query default view
obsidian base:query file="tasks" view="Kanban"    # Query specific view
obsidian base:views file="tasks"                  # List views
obsidian base:create file="tasks" title="Buy milk"  # Add item
```

Supported output formats: `json` (default), `csv`, `tsv`, `md`, `paths`.

---

## History

File version history (File Recovery core plugin).

```bash
obsidian history:list                             # Files with history
obsidian history path="folder/note.md"            # List versions
obsidian history:read path="note.md"              # Latest version
obsidian history:read path="note.md" version=3    # Specific version
obsidian history:restore path="note.md" version=3
obsidian history:open path="note.md"              # Open recovery UI
```

---

## Workspace & Tabs

```bash
obsidian workspace                                # Workspace tree
obsidian tabs                                     # Open tabs
obsidian tab:open file="folder/note.md"           # Open in new tab
obsidian tab:open view="graph"                    # Open view type
obsidian recents                                  # Recently opened
```

---

## Diff

Compare local and sync versions.

```bash
obsidian diff path="note.md"               # List versions
obsidian diff path="note.md" from=1 to=2   # Compare two versions
obsidian diff path="note.md" filter=local   # Local only
obsidian diff path="note.md" filter=sync    # Sync only
```

---

## Developer

### Screenshots

```bash
obsidian dev:screenshot path="folder/screenshot.png"
```

Path must be **vault-relative**.

### JavaScript Evaluation

```bash
obsidian eval code="app.vault.getFiles().length"
obsidian eval code="app.vault.getMarkdownFiles().map(f => f.path).join('\n')"
```

> **Multiline JS:** Write to temp file, then `obsidian eval code="$(cat /tmp/obs.js)"`.

### Console & Errors

```bash
obsidian dev:debug on              # Start capturing (required before dev:console)
obsidian dev:debug off
obsidian dev:console limit=20      # Recent console output
obsidian dev:errors                # Recent errors
```

### DOM & CSS Inspection

```bash
obsidian dev:dom selector=".view-content"             # outerHTML
obsidian dev:dom selector=".view-content" text         # Text content
obsidian dev:dom selector=".view-content" total        # Count
obsidian dev:css selector=".view-content"              # CSS with sources
obsidian dev:css selector=".view-content" prop=color   # Specific property
```

### Other Developer Tools

```bash
obsidian devtools                   # Toggle Electron DevTools
obsidian dev:mobile on|off          # Mobile emulation
obsidian dev:cdp method="Runtime.evaluate" params='{"expression":"1+1"}'
```

---

## Vault & System

```bash
obsidian vault                         # Name, path, file/folder counts
obsidian vaults                        # All known vaults
obsidian version                       # Version info
obsidian outline path="note.md"        # Heading structure
obsidian wordcount path="note.md"      # Word/character count
obsidian reload                        # Reload vault
obsidian restart                       # Restart app
```

---

## Output Formatting

| Format | Description | Best for |
|---|---|---|
| `text` | Plain text (default) | Piping to grep/awk/sed |
| `json` | JSON array or object | Processing with jq, AI agents |
| `csv` | Comma-separated | Spreadsheet import |
| `tsv` | Tab-separated | Shell parsing |
| `yaml` | YAML output | Config-style |
| `md` | Markdown table | Embedding in notes |
| `paths` | One path per line | Batch file operations |
| `tree` | Tree view | Visual hierarchy |

Not all formats are supported by every command. Use `text` or `json` when in doubt.

### Piping Examples

```bash
obsidian files folder="projects" | wc -l
obsidian tag name="urgent" | while read -r note; do obsidian read path="$note"; done
obsidian search query="meeting" format=json | jq '.[]'
obsidian base:query file="tasks" format=csv
```

---

## Troubleshooting

| Problem | Cause | Fix |
|---|---|---|
| Empty output / hangs | Obsidian not running | Start Obsidian |
| Command not found | CLI not in PATH | Re-enable CLI in Settings; restart terminal |
| Wrong vault targeted | Multi-vault ambiguity | Pass vault name as first arg |
| Colon+params exit 127 (Windows) | Missing `Obsidian.com` | Reinstall latest from obsidian.md/download |
| Colon+params exit 127 (Git Bash) | Bash resolves to `.exe` not `.com` | Create wrapper script |
| IPC socket not found (Linux) | `PrivateTmp=true` in systemd | Set `PrivateTmp=false` |
| Snap confinement | Snap restricts IPC | Use `.deb` package |
| `property:set` list is string | CLI stores value as-is | Edit frontmatter directly or use `eval` |

### Platform Notes

- **macOS**: Binary registered in PATH via `~/.zprofile`. For non-zsh shells, add manually.
- **Windows**: Requires `Obsidian.com` redirector alongside `Obsidian.exe`. Must use normal-privilege terminal.
- **Linux**: Symlink at `/usr/local/bin/obsidian`. Use `.deb` for headless (not snap). Run under `xvfb`.
