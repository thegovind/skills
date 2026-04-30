# Azure Speech SSML element guide

Use this file when you need a quick structure or syntax check.

## Canonical document templates

### Minimal W3C-style wrapper

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-US-AvaNeural">
    Hello world.
  </voice>
</speak>
```

### Azure-specific wrapper

```xml
<speak version="1.0"
       xmlns="http://www.w3.org/2001/10/synthesis"
       xmlns:mstts="https://www.w3.org/2001/mstts"
       xml:lang="en-US">
  <voice name="en-US-Ava:DragonHDOmniLatestNeural">
    <mstts:express-as style="friendly">
      Hello world.
    </mstts:express-as>
  </voice>
</speak>
```

## Escaping and XML hygiene

Always escape:

- `&` as `&amp;`
- `<` as `&lt;`
- `>` as `&gt;`

Always quote attribute values:

- valid: `<prosody volume="90">`
- invalid: `<prosody volume=90>`

If the user gives raw XML with broken attributes, fix the attributes first before changing anything else.

## High-value element reference

| Element | Use for | Key rules |
|---|---|---|
| `<speak>` | Required root | Needs `version`, `xmlns`, and `xml:lang` |
| `<voice>` | Voice selection | Must include `name`; every SSML document needs at least one |
| `<p>` | Paragraph boundaries | Good for long-form narration or multi-section scripts |
| `<s>` | Sentence boundaries | Useful when sentence-level styling or pronunciation changes matter |
| `<break>` | Inline pause anywhere in the text | Use `strength` or `time`; exact durations work well for user-requested pauses |
| `<mstts:silence>` | Leading, trailing, or sentence-boundary silence | Not a drop-in replacement for `<break>`; applies at boundaries |
| `<prosody>` | Pitch, rate, range, contour, volume | Prefer sentence-length spans; contour is a poor fit for single words |
| `<mstts:express-as>` | Style, role, scenario tone | Requires `style`; Azure-specific tag |
| `<phoneme>` | Exact pronunciation | Text only inside the element |
| `<sub>` | Spoken alias | Text only inside the element |
| `<say-as>` | Dates, times, numbers, spelling | Add `format` only when it actually clarifies ambiguity |
| `<lang>` | Accent or language switch | Use only with voices that support it |

## Break vs. silence

Use `<break>` when:

- the pause belongs at a precise point in the text
- the user explicitly asked for a pause like "wait half a second here"
- you are working with a standard neural voice that supports timing tags

Use `mstts:silence` when:

- you want leading or trailing silence
- you want sentence-boundary silence across an entire voice block
- you are shaping boundaries rather than a single inline pause

Do not stack punctuation, `<break>`, and `mstts:silence` everywhere. That usually makes speech worse rather than better.

## Prosody value reminders

### Rate

- named values: `x-slow`, `slow`, `medium`, `fast`, `x-fast`
- numeric multiplier: `0.5` to `2`
- percentage: `-20%`, `+30%`

### Pitch

- named values: `x-low`, `low`, `medium`, `high`, `x-high`
- relative values: `+80Hz`, `-2st`, `+20%`
- absolute values: `600Hz`

### Volume

- named values: `silent`, `x-soft`, `soft`, `medium`, `loud`, `x-loud`
- absolute values: `0` to `100`
- relative values: `+10`, `-5.5`, `+3%`

### Contour

Example:

```xml
<prosody contour="(0%,+20Hz) (40%,-2st) (100%,+10Hz)">
  We need to review the final numbers.
</prosody>
```

Use contour only on full sentences or longer phrases.

## Structure and nesting reminders

- `<phoneme>` can contain only text.
- `<sub>` can contain only text.
- `<say-as>` can contain only text.
- `<prosody>` can contain text plus elements like `break`, `phoneme`, `sub`, and `s`.
- `<s>` can contain text plus elements like `break`, `phoneme`, `prosody`, `say-as`, `sub`, and `mstts:express-as`.
- `<voice>` can contain most speaking elements, but not another `<speak>` root.

## Practical authoring hints

- Azure already handles many pauses from punctuation, so do not over-mark simple text.
- Use `<p>` and `<s>` to make long input cleaner, not to decorate short one-liners.
- If the user wants "better" audio but gives no specific acoustic goal, start with voice choice and sentence cleanup before heavy markup.
