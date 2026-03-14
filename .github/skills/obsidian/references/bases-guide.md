# Obsidian Bases Guide

Bases (v1.12+) are `.base` files containing YAML that create database-like views of vault notes.

## Schema

```yaml
# Global filters — apply to ALL views
filters:
  and: []    # All conditions must be true
  or: []     # Any condition can be true
  not: []    # Exclude matching items

# Computed properties
formulas:
  formula_name: 'expression'

# Display name overrides
properties:
  property_name:
    displayName: "Display Name"
  formula.formula_name:
    displayName: "Formula Display Name"

# Custom summary formulas
summaries:
  custom_summary: 'values.mean().round(3)'

# One or more views
views:
  - type: table | cards | list | map
    name: "View Name"
    limit: 10                    # Optional: limit results
    groupBy:
      property: property_name
      direction: ASC | DESC
    filters:                     # View-specific filters
      and: []
    order:                       # Properties to display
      - file.name
      - property_name
      - formula.formula_name
    summaries:
      property_name: Average
```

## Filter Syntax

### Simple Filters

```yaml
filters: 'status == "done"'
```

### Compound Filters

```yaml
# AND — all must be true
filters:
  and:
    - 'status == "done"'
    - 'priority > 3'

# OR — any can be true
filters:
  or:
    - file.hasTag("book")
    - file.hasTag("article")

# NOT — exclude
filters:
  not:
    - file.hasTag("archived")

# Nested
filters:
  or:
    - file.hasTag("tag")
    - and:
        - file.hasTag("book")
        - file.hasLink("Textbook")
    - not:
        - file.hasTag("book")
        - file.inFolder("Required Reading")
```

### Filter Operators

| Operator | Description |
|----------|-------------|
| `==` | Equals |
| `!=` | Not equal |
| `>` `<` `>=` `<=` | Comparisons |
| `&&` | Logical and |
| `\|\|` | Logical or |
| `!` | Logical not |

## Properties

### Three Types

1. **Note properties** — From frontmatter: `author` or `note.author`
2. **File properties** — File metadata: `file.name`, `file.mtime`
3. **Formula properties** — Computed: `formula.my_formula`

### File Properties

| Property | Type | Description |
|----------|------|-------------|
| `file.name` | String | File name |
| `file.basename` | String | Name without extension |
| `file.path` | String | Full path |
| `file.folder` | String | Parent folder |
| `file.ext` | String | Extension |
| `file.size` | Number | Size in bytes |
| `file.ctime` | Date | Created time |
| `file.mtime` | Date | Modified time |
| `file.tags` | List | All tags |
| `file.links` | List | Internal links |
| `file.backlinks` | List | Files linking here |
| `file.embeds` | List | Embeds in note |
| `file.properties` | Object | All frontmatter |

## Formula Syntax

Formulas compute values from properties.

```yaml
formulas:
  # Arithmetic
  total: "price * quantity"

  # Conditional
  status_icon: 'if(done, "✅", "⏳")'

  # String formatting
  formatted_price: 'if(price, price.toFixed(2) + " dollars")'

  # Date formatting
  created: 'file.ctime.format("YYYY-MM-DD")'

  # Days since created
  days_old: '(now() - file.ctime).days'

  # Days until due (with null guard)
  days_until_due: 'if(due_date, (date(due_date) - today()).days, "")'
```

### Duration Type

Subtracting two dates returns a **Duration**, not a number.

**Duration fields:** `.days`, `.hours`, `.minutes`, `.seconds`, `.milliseconds`

```yaml
# CORRECT
"(date(due_date) - today()).days"              # Returns number
"(now() - file.ctime).days.round(0)"          # Rounded days

# WRONG — Duration doesn't support .round() directly
# "(now() - file.ctime).round(0)"
```

### Date Arithmetic

```yaml
"now() + \"1 day\""       # Tomorrow
"today() + \"7d\""        # Week from today
"now() - file.ctime"      # Returns Duration
```

Duration units: `y/year/years`, `M/month/months`, `d/day/days`, `w/week/weeks`, `h/hour/hours`, `m/minute/minutes`, `s/second/seconds`.

## View Types

### Table

```yaml
views:
  - type: table
    name: "My Table"
    order:
      - file.name
      - status
      - due_date
    summaries:
      price: Sum
      count: Average
```

### Cards

```yaml
views:
  - type: cards
    name: "Gallery"
    order:
      - cover
      - file.name
      - author
```

### List

```yaml
views:
  - type: list
    name: "Simple List"
    order:
      - file.name
      - status
```

### Map

Requires latitude/longitude properties and the Maps community plugin.

## Default Summary Formulas

| Name | Input | Description |
|------|-------|-------------|
| `Average` | Number | Mean |
| `Min` / `Max` | Number | Extremes |
| `Sum` | Number | Total |
| `Range` | Number | Max - Min |
| `Median` | Number | Median |
| `Stddev` | Number | Standard deviation |
| `Earliest` / `Latest` | Date | Extremes |
| `Checked` / `Unchecked` | Boolean | Counts |
| `Empty` / `Filled` | Any | Presence counts |
| `Unique` | Any | Unique count |

## Complete Examples

### Task Tracker

```yaml
filters:
  and:
    - file.hasTag("task")
    - 'file.ext == "md"'

formulas:
  days_until_due: 'if(due, (date(due) - today()).days, "")'
  is_overdue: 'if(due, date(due) < today() && status != "done", false)'
  priority_label: 'if(priority == 1, "🔴 High", if(priority == 2, "🟡 Medium", "🟢 Low"))'

properties:
  formula.days_until_due:
    displayName: "Days Until Due"
  formula.priority_label:
    displayName: Priority

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
    groupBy:
      property: status
      direction: ASC
    summaries:
      formula.days_until_due: Average

  - type: table
    name: "Completed"
    filters:
      and:
        - 'status == "done"'
    order:
      - file.name
      - completed_date
```

### Reading List

```yaml
filters:
  or:
    - file.hasTag("book")
    - file.hasTag("article")

formulas:
  reading_time: 'if(pages, (pages * 2).toString() + " min", "")'
  status_icon: 'if(status == "reading", "📖", if(status == "done", "✅", "📚"))'

views:
  - type: cards
    name: "Library"
    order:
      - cover
      - file.name
      - author
      - formula.status_icon
    filters:
      not:
        - 'status == "dropped"'

  - type: table
    name: "Reading List"
    filters:
      and:
        - 'status == "to-read"'
    order:
      - file.name
      - author
      - pages
      - formula.reading_time
```

### Daily Notes Index

```yaml
filters:
  and:
    - file.inFolder("Daily Notes")
    - '/^\d{4}-\d{2}-\d{2}$/.matches(file.basename)'

formulas:
  word_estimate: '(file.size / 5).round(0)'
  day_of_week: 'date(file.basename).format("dddd")'

views:
  - type: table
    name: "Recent Notes"
    limit: 30
    order:
      - file.name
      - formula.day_of_week
      - formula.word_estimate
      - file.mtime
```

## Embedding Bases

```markdown
![[MyBase.base]]
![[MyBase.base#View Name]]
```

## YAML Quoting Rules

- Single quotes for formulas with double quotes: `'if(done, "Yes", "No")'`
- Double quotes for simple strings: `"My View Name"`
- Strings with `:`, `{`, `}`, `[`, `]`, `#`, `!`, `>`, `<`, `=` must be quoted

## Troubleshooting

| Problem | Cause | Fix |
|---|---|---|
| YAML syntax error | Unquoted special characters | Quote strings with `:`, `#`, etc. |
| Formula error | Duration math without field | Use `.days`, `.hours` before `.round()` |
| Empty column | Undefined formula reference | Ensure `formula.X` has matching `formulas:` entry |
| Wrong values | Missing null guard | Wrap with `if(prop, ..., "")` |
