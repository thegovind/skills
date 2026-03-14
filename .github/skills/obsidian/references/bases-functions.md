# Bases Functions Reference

Complete reference for all Obsidian Bases formula functions.

## Global Functions

| Function | Signature | Description |
|----------|-----------|-------------|
| `date()` | `date(string): date` | Parse string to date (`YYYY-MM-DD HH:mm:ss`) |
| `duration()` | `duration(string): duration` | Parse duration string |
| `now()` | `now(): date` | Current date and time |
| `today()` | `today(): date` | Current date (time = 00:00:00) |
| `if()` | `if(condition, trueResult, falseResult?)` | Conditional |
| `min()` | `min(n1, n2, ...): number` | Smallest number |
| `max()` | `max(n1, n2, ...): number` | Largest number |
| `number()` | `number(any): number` | Convert to number |
| `link()` | `link(path, display?): Link` | Create a link |
| `list()` | `list(element): List` | Wrap in list if not already |
| `file()` | `file(path): file` | Get file object |
| `image()` | `image(path): image` | Create image for rendering |
| `icon()` | `icon(name): icon` | Lucide icon by name |
| `html()` | `html(string): html` | Render as HTML |
| `escapeHTML()` | `escapeHTML(string): string` | Escape HTML characters |

## Any Type

| Function | Signature | Description |
|----------|-----------|-------------|
| `isTruthy()` | `any.isTruthy(): boolean` | Coerce to boolean |
| `isType()` | `any.isType(type): boolean` | Check type |
| `toString()` | `any.toString(): string` | Convert to string |

## Date

**Fields:** `date.year`, `date.month`, `date.day`, `date.hour`, `date.minute`, `date.second`, `date.millisecond`

| Function | Signature | Description |
|----------|-----------|-------------|
| `date()` | `date.date(): date` | Remove time portion |
| `format()` | `date.format(string): string` | Format with Moment.js pattern |
| `time()` | `date.time(): string` | Get time as string |
| `relative()` | `date.relative(): string` | Human-readable relative time |
| `isEmpty()` | `date.isEmpty(): boolean` | Always false for dates |

## Duration

Subtracting two dates returns a Duration type.

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `duration.days` | Number | Total days |
| `duration.hours` | Number | Total hours |
| `duration.minutes` | Number | Total minutes |
| `duration.seconds` | Number | Total seconds |
| `duration.milliseconds` | Number | Total milliseconds |

**IMPORTANT:** Duration does NOT support `.round()`, `.floor()`, `.ceil()` directly. Access a numeric field first.

```yaml
# CORRECT
"(date(due_date) - today()).days"
"(now() - file.ctime).days.round(0)"

# WRONG
# "(now() - file.ctime).round(0)"
```

### Date Arithmetic

```yaml
"now() + \"1 day\""       # Tomorrow
"today() + \"7d\""        # Week from today
"now() - file.ctime"      # Returns Duration
"(now() - file.ctime).days"  # Get days as number
```

Duration units: `y/year/years`, `M/month/months`, `d/day/days`, `w/week/weeks`, `h/hour/hours`, `m/minute/minutes`, `s/second/seconds`.

## String

**Field:** `string.length`

| Function | Signature | Description |
|----------|-----------|-------------|
| `contains()` | `string.contains(value): boolean` | Check substring |
| `containsAll()` | `string.containsAll(...values): boolean` | All substrings |
| `containsAny()` | `string.containsAny(...values): boolean` | Any substring |
| `startsWith()` | `string.startsWith(query): boolean` | Starts with |
| `endsWith()` | `string.endsWith(query): boolean` | Ends with |
| `isEmpty()` | `string.isEmpty(): boolean` | Empty or not present |
| `lower()` | `string.lower(): string` | Lowercase |
| `title()` | `string.title(): string` | Title Case |
| `trim()` | `string.trim(): string` | Remove whitespace |
| `replace()` | `string.replace(pattern, replacement): string` | Replace |
| `repeat()` | `string.repeat(count): string` | Repeat |
| `reverse()` | `string.reverse(): string` | Reverse |
| `slice()` | `string.slice(start, end?): string` | Substring |
| `split()` | `string.split(separator, n?): list` | Split to list |

## Number

| Function | Signature | Description |
|----------|-----------|-------------|
| `abs()` | `number.abs(): number` | Absolute value |
| `ceil()` | `number.ceil(): number` | Round up |
| `floor()` | `number.floor(): number` | Round down |
| `round()` | `number.round(digits?): number` | Round to digits |
| `toFixed()` | `number.toFixed(precision): string` | Fixed-point notation |
| `isEmpty()` | `number.isEmpty(): boolean` | Not present |

## List

**Field:** `list.length`

| Function | Signature | Description |
|----------|-----------|-------------|
| `contains()` | `list.contains(value): boolean` | Element exists |
| `containsAll()` | `list.containsAll(...values): boolean` | All exist |
| `containsAny()` | `list.containsAny(...values): boolean` | Any exists |
| `filter()` | `list.filter(expression): list` | Filter (uses `value`, `index`) |
| `map()` | `list.map(expression): list` | Transform (uses `value`, `index`) |
| `reduce()` | `list.reduce(expression, initial): any` | Reduce (uses `value`, `index`, `acc`) |
| `flat()` | `list.flat(): list` | Flatten nested |
| `join()` | `list.join(separator): string` | Join to string |
| `reverse()` | `list.reverse(): list` | Reverse order |
| `slice()` | `list.slice(start, end?): list` | Sublist |
| `sort()` | `list.sort(): list` | Sort ascending |
| `unique()` | `list.unique(): list` | Remove duplicates |
| `isEmpty()` | `list.isEmpty(): boolean` | No elements |

## File

| Function | Signature | Description |
|----------|-----------|-------------|
| `asLink()` | `file.asLink(display?): Link` | Convert to link |
| `hasLink()` | `file.hasLink(otherFile): boolean` | Has link to file |
| `hasTag()` | `file.hasTag(...tags): boolean` | Has any of the tags |
| `hasProperty()` | `file.hasProperty(name): boolean` | Has property |
| `inFolder()` | `file.inFolder(folder): boolean` | In folder or subfolder |

## Link

| Function | Signature | Description |
|----------|-----------|-------------|
| `asFile()` | `link.asFile(): file` | Get file object |
| `linksTo()` | `link.linksTo(file): boolean` | Links to file |

## Object

| Function | Signature | Description |
|----------|-----------|-------------|
| `isEmpty()` | `object.isEmpty(): boolean` | No properties |
| `keys()` | `object.keys(): list` | List of keys |
| `values()` | `object.values(): list` | List of values |

## RegExp

| Function | Signature | Description |
|----------|-----------|-------------|
| `matches()` | `regexp.matches(string): boolean` | Test if matches |
