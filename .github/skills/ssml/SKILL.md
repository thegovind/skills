---
name: ssml
description: >
  Convert raw text, speaking notes, or broken XML into correctly formatted Azure Speech SSML.
  Use this whenever the user wants Azure TTS output, SSML, text to speech markup, prosody,
  pronunciation control, speaking styles, multilingual voice output, or needs help repairing
  and validating SSML for Azure Speech. Also use it when the user starts from plain text and
  wants it to sound warmer, clearer, slower, more dramatic, or more natural in Azure Speech.
  Triggers: "ssml", "azure tts", "text to speech", "speech synthesis", "voice markup",
  "prosody", "phoneme", "say-as", "azure speech", "mstts:express-as", "fix ssml",
  "raw text to ssml".
---

# Azure Speech SSML

Turn plain text or rough speaking directions into valid, useful Azure Speech SSML.

## Default approach

1. Identify the real goal: minimal valid SSML, more expressive delivery, pronunciation repair, multilingual speech, or SSML debugging.
2. Choose the voice family before adding markup. Azure voice families do **not** support the same tags.
3. Use the lightest markup that achieves the goal. Azure already handles basic punctuation and sentence rhythm well.
4. Return copy-paste-ready XML unless the user asks for explanation.

## Choose the right voice family first

Use this decision rule before writing tags:

| Need | Preferred voice family | Why |
|---|---|---|
| Full control with `<prosody>`, `<break>`, `<emphasis>`, `<phoneme>`, or `<audio>` | Standard Azure neural voice | Safest choice for classic Azure SSML controls |
| HD voice with exact pronunciation tags | DragonHD | Supports `<phoneme>`, `<say-as>`, `<sub>`, and `<lang>`, but not expressive timing tags |
| HD voice with styles, roles, and multilingual output | Dragon HD Omni | Supports `<mstts:express-as>` and `<lang>`, but not `<prosody>`, `<break>`, or `<phoneme>` |

Important implications:

- Do **not** emit unsupported tags for the chosen voice family just because they look helpful.
- If the user asks for a conflicting combo such as "Dragon HD Omni plus `<prosody>` and `<break>`", call out the conflict and either:
  - switch to a standard neural voice, or
  - keep the requested HD voice and simplify the markup.
- If the user does not specify a voice, prefer a standard neural voice in the target locale because it gives the most markup flexibility.

See [references/voice-model-guide.md](references/voice-model-guide.md) for the compatibility matrix and fallback rules.

## Canonical wrappers

Use a minimal wrapper when the request only needs W3C SSML tags:

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-US-AvaNeural">
    Text here.
  </voice>
</speak>
```

Use the Azure namespace when any `mstts:*` element appears:

```xml
<speak version="1.0"
       xmlns="http://www.w3.org/2001/10/synthesis"
       xmlns:mstts="https://www.w3.org/2001/mstts"
       xml:lang="en-US">
  <voice name="en-US-Ava:DragonHDOmniLatestNeural">
    <mstts:express-as style="friendly">
      Hello there.
    </mstts:express-as>
  </voice>
</speak>
```

Do not forget:

- `version="1.0"`
- `xmlns="http://www.w3.org/2001/10/synthesis"`
- `xml:lang="..."`
- at least one `<voice name="...">`
- `xmlns:mstts` whenever any `mstts:*` element is present

## Raw text to SSML authoring rules

Apply these rules in order:

1. Preserve the user's words unless they explicitly ask for a rewrite.
2. Match `xml:lang` to the dominant language or locale of the output.
3. Use `<p>` and `<s>` when they clarify structure, long-form pacing, or mixed markup. Do not wrap every tiny sentence just for ceremony.
4. Use punctuation first. Add `<break>` only when the user wants a pause stronger or more precise than normal punctuation.
5. Use `<prosody>` for explicit pace, pitch, or volume changes only on compatible voices.
6. Use `<mstts:express-as>` when the user asks for mood, scenario, or role-play and the voice family supports it.
7. Use `<sub alias="...">` for simple pronunciation fixes. Use `<phoneme>` only when exact pronunciation really matters.
8. Use `<say-as>` for dates, times, numbers, phone numbers, characters, spelling, and similar content types.
9. Use `<lang xml:lang="...">` only when the voice supports language switching or accent steering.
10. Escape XML special characters and quote every attribute value.

## Pattern cookbook

### 1. Minimal cleanup from raw text

When the user mainly wants valid Azure SSML and not a dramatic performance, keep it simple:

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-US-AvaNeural">
    <p>
      <s>Welcome to the launch.</s>
      <s>We start in <say-as interpret-as="cardinal">3</say-as> minutes.</s>
    </p>
  </voice>
</speak>
```

### 2. Style and tone

If the user asks for a mood such as friendly, calm, angry, documentary, or customer service, prefer `mstts:express-as` over aggressive prosody tweaks:

```xml
<speak version="1.0"
       xmlns="http://www.w3.org/2001/10/synthesis"
       xmlns:mstts="https://www.w3.org/2001/mstts"
       xml:lang="en-US">
  <voice name="en-US-Ava:DragonHDOmniLatestNeural">
    <mstts:express-as style="friendly">
      Hello! I'm ready when you are.
    </mstts:express-as>
  </voice>
</speak>
```

Notes:

- `style` is required when you use `mstts:express-as`.
- `styledegree` is optional and ranges from `0.01` to `2`.
- `role` is optional and voice-specific.

### 3. Pacing and emphasis

If the request is "slow this down", "make this punchier", or "pause here", use standard neural voices and explicit timing tags:

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-US-AvaNeural">
    <prosody rate="slow" pitch="medium">
      Let me explain the next step.
    </prosody>
    <break time="500ms" />
    <prosody rate="-10%">
      Please wait for the confirmation message.
    </prosody>
  </voice>
</speak>
```

Use `contour` only on full sentences or longer phrases. Do not use it on single words.

### 4. Pronunciation repair

Use the lightest tool that works:

- `<sub alias="SQL">Sequel</sub>` for common spoken aliases
- `<phoneme alphabet="ipa" ph="tə.ˈmeɪ.toʊ">tomato</phoneme>` when exact sounds matter
- custom lexicons when many repeated terms need consistent pronunciation

Example:

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-US-AvaNeural">
    The recipe uses <phoneme alphabet="ipa" ph="tə.ˈmeɪ.toʊ">tomato</phoneme> paste.
    Query the <sub alias="Sequel">SQL</sub> database after lunch.
  </voice>
</speak>
```

### 5. Multilingual passages

When the user mixes languages or wants accent steering, wrap only the spans that change language:

```xml
<speak version="1.0"
       xmlns="http://www.w3.org/2001/10/synthesis"
       xml:lang="en-US">
  <voice name="en-US-AvaMultilingualNeural">
    Welcome to our cafe.
    <lang xml:lang="fr-FR">Bonjour et merci d'etre ici.</lang>
  </voice>
</speak>
```

Do not use `<lang>` on voices that do not support language switching.

### 6. Fixing broken SSML

When the user gives broken SSML, preserve intent but repair structure:

- add missing root attributes and namespaces
- quote attribute values
- escape `&`, `<`, and `>`
- fix invalid nesting
- remove unsupported tags for the selected voice family

If the user's requested voice family makes the markup impossible, say so briefly and provide the closest valid version.

## Validation checklist

Before you return SSML, verify all of the following:

- The document has a `<speak>` root with `version`, `xmlns`, and `xml:lang`.
- There is at least one `<voice name="...">`.
- Every attribute value is quoted.
- `&`, `<`, and `>` are escaped in text and attributes.
- `mstts:*` tags only appear when `xmlns:mstts` is present.
- Voice family and tags are compatible.
- `<phoneme>`, `<say-as>`, and `<sub>` only contain text.
- `<break>` and `<mstts:silence>` are used intentionally and not piled on top of normal punctuation without reason.
- If you used `<lang>`, the target voice actually supports that language or accent.

## Output format

Use the format that best matches the user's request:

- If the user asks for SSML, return a single copy-paste-ready XML block first.
- If you had to change voice family, remove unsupported tags, or make another non-obvious tradeoff, add a short note after the XML.
- If the user asked to debug SSML, list the concrete issues and then show the corrected XML.
- If the user asked for explanation, keep it short and tied to the exact tags you chose.

Never return pseudo-XML, placeholder ellipses inside required structure, or invalid attribute syntax.

## Reference files

Use these files when you need more detail:

| File | Use when |
|---|---|
| [references/voice-model-guide.md](references/voice-model-guide.md) | Choosing between standard neural, DragonHD, and Dragon HD Omni |
| [references/element-guide.md](references/element-guide.md) | Checking element structure, escaping, pauses, and nesting |
| [references/pronunciation-and-language.md](references/pronunciation-and-language.md) | Working with phonemes, `say-as`, lexicons, and multilingual spans |
