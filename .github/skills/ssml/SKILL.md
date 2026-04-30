---
name: ssml
description: >
  Convert raw text into correctly formatted Azure Speech SSML (Speech Synthesis Markup Language)
  for text-to-speech synthesis. Use this skill whenever the user wants to generate SSML, convert
  plain text to spoken audio markup, format text for Azure TTS, add prosody/emphasis/pronunciation
  controls, pick voices (including HD and Dragon HD Omni voices), apply speaking styles, or fix
  broken SSML. Triggers: "ssml", "text to speech", "tts", "speech synthesis", "azure speech",
  "voice markup", "prosody", "speak tag", "DragonHD", "pronunciation", "say-as", "phoneme",
  "convert text to ssml", "format for tts".
---

# Azure Speech SSML

Generate correctly structured SSML for Azure AI Speech text-to-speech.

## Core Principles

When converting raw text to SSML:

1. **Always produce valid XML** — escape `&` → `&amp;`, `<` → `&lt;`, `>` → `&gt;` in text content. Quote all attribute values.
2. **Wrap everything** in `<speak>` → `<voice>` at minimum. No bare text.
3. **Choose the right voice** for the language and use case (see Voice Selection below).
4. **Add structure** — use `<p>` and `<s>` for paragraphs and sentences when the text has clear structure.
5. **Apply prosody, breaks, and styles** where they improve naturalness — don't over-markup simple text.
6. **Use `say-as`** for dates, numbers, times, currencies, addresses, telephone numbers, and spelled-out content.
7. **Use `sub`** for abbreviations and acronyms that should be expanded when spoken.
8. **Use `phoneme`** only when default pronunciation is wrong — always keep readable text as fallback.

## Minimal SSML Template

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Ava:DragonHDLatestNeural">
        Your text here.
    </voice>
</speak>
```

When using Microsoft-specific extensions (styles, silence, viseme, background audio, etc.), add the `mstts` namespace:

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis"
       xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-Ava:DragonHDLatestNeural">
        Your text here.
    </voice>
</speak>
```

## Voice Selection

Pick the voice based on language, quality tier, and use case.

### Voice Name Formats

| Tier | Format | Example |
|------|--------|---------|
| Standard Neural | `{locale}-{Name}Neural` | `en-US-AvaNeural` |
| Multilingual Neural | `{locale}-{Name}MultilingualNeural` | `en-US-AvaMultilingualNeural` |
| DragonHD | `{locale}-{Name}:DragonHDLatestNeural` | `en-US-Ava:DragonHDLatestNeural` |
| Dragon HD Omni | `{locale}-{Name}:DragonHDOmniLatestNeural` | `en-US-Ava:DragonHDOmniLatestNeural` |
| Multi-talker | `en-US-MultiTalker-Ava-Andrew:DragonHDLatestNeural` | (dialogue between two speakers) |

### When to Use Which

- **DragonHD** — Best quality for supported voices. Use for enterprise, customer service, accessibility. Supports temperature parameter for variation control.
- **Dragon HD Omni** — 700+ voices, 100+ speaking styles, automatic emotion detection. Use for content creation, audiobooks, podcasts, diverse persona needs.
- **Standard Neural** — Full SSML support (all tags). Use when you need `<prosody>`, `<break>`, `<emphasis>`, or other tags unsupported by HD voices.
- **Multilingual Neural** — Auto-detects language. Use for multilingual content.

### Popular English Voices

| Voice | Gender | Notes |
|-------|--------|-------|
| `en-US-Ava:DragonHDLatestNeural` | Female | Top quality, GA |
| `en-US-Andrew:DragonHDLatestNeural` | Male | Top quality, GA |
| `en-US-Emma:DragonHDLatestNeural` | Female | GA |
| `en-US-Brian:DragonHDLatestNeural` | Male | GA |
| `en-US-Jenny:DragonHDLatestNeural` | Female | GA |
| `en-US-Ava:DragonHDOmniLatestNeural` | Female | 100+ styles |
| `en-US-Andrew:DragonHDOmniLatestNeural` | Male | 100+ styles |

For the full HD voice list, see `references/hd-voices.md`.

## SSML Elements Quick Reference

### Structure Elements

| Element | Purpose | Example |
|---------|---------|---------|
| `<speak>` | Root element (required) | `<speak version="1.0" xmlns="..." xml:lang="en-US">` |
| `<voice>` | Select voice (required, inside speak) | `<voice name="en-US-Ava:DragonHDLatestNeural">` |
| `<p>` | Paragraph | `<p>First paragraph.</p>` |
| `<s>` | Sentence | `<s>First sentence.</s>` |
| `<break>` | Pause | `<break time="500ms"/>` or `<break strength="strong"/>` |
| `<mstts:silence>` | Leading/trailing/boundary silence | `<mstts:silence type="Sentenceboundary" value="200ms"/>` |

### Voice & Style Elements

| Element | Purpose | Example |
|---------|---------|---------|
| `<mstts:express-as>` | Speaking style | `<mstts:express-as style="cheerful">` |
| `<lang xml:lang>` | Switch language | `<lang xml:lang="fr-FR">Bonjour!</lang>` |
| `<prosody>` | Pitch, rate, volume | `<prosody rate="+20%" pitch="high">` |
| `<emphasis>` | Word stress | `<emphasis level="strong">important</emphasis>` |

### Pronunciation Elements

| Element | Purpose | Example |
|---------|---------|---------|
| `<say-as>` | Interpret content type | `<say-as interpret-as="date" format="mdy">10/19/2024</say-as>` |
| `<sub>` | Substitute spoken text | `<sub alias="World Wide Web Consortium">W3C</sub>` |
| `<phoneme>` | Phonetic pronunciation | `<phoneme alphabet="ipa" ph="tə.ˈmeɪ.toʊ">tomato</phoneme>` |
| `<lexicon>` | Custom pronunciation dictionary | `<lexicon uri="https://example.com/lexicon.xml"/>` |

### Audio & Advanced Elements

| Element | Purpose | Example |
|---------|---------|---------|
| `<audio>` | Insert prerecorded audio | `<audio src="https://example.com/beep.wav"/>` |
| `<mstts:backgroundaudio>` | Background audio | `<mstts:backgroundaudio src="..." volume="0.7"/>` |
| `<mstts:audioduration>` | Control output duration | `<mstts:audioduration value="20s"/>` |
| `<bookmark>` | Mark position for SDK events | `<bookmark mark="section_1"/>` |
| `<mstts:viseme>` | Lip-sync data | `<mstts:viseme type="FacialExpression"/>` |

## Conversion Rules

When converting raw text to SSML, apply these rules in order:

### 1. Escape Special Characters
- `&` → `&amp;`
- `<` → `&lt;`
- `>` → `&gt;`
- Quotes in attribute values must be properly paired (use `"` or `'`)

### 2. Detect and Tag Content Types with `<say-as>`

| Content Pattern | interpret-as | format | Example Input → SSML |
|----------------|-------------|--------|---------------------|
| Dates (MM/DD/YYYY) | `date` | `mdy` | `10/19/2024` → `<say-as interpret-as="date" format="mdy">10/19/2024</say-as>` |
| Dates (DD/MM/YYYY) | `date` | `dmy` | `19/10/2024` → `<say-as interpret-as="date" format="dmy">19/10/2024</say-as>` |
| Times | `time` | `hms12` / `hms24` | `4:00pm` → `<say-as interpret-as="time" format="hms12">4:00pm</say-as>` |
| Cardinal numbers | `cardinal` | — | `There are 10 items` → `There are <say-as interpret-as="cardinal">10</say-as> items` |
| Ordinals | `ordinal` | — | `1st place` → `<say-as interpret-as="ordinal">1st</say-as> place` |
| Phone numbers | `telephone` | — | `(888) 555-1212` → `<say-as interpret-as="telephone">(888) 555-1212</say-as>` |
| Currency | `currency` | — | `$99.90` → `<say-as interpret-as="currency">99.9 USD</say-as>` |
| Spelled out | `characters` | — | Spell "API" → `<say-as interpret-as="characters">API</say-as>` |
| Addresses | `address` | — | `150th CT NE, Redmond, WA` → `<say-as interpret-as="address">150th CT NE, Redmond, WA</say-as>` |
| Fractions | `fraction` | — | `3/8` → `<say-as interpret-as="fraction">3/8</say-as>` |
| Durations | `duration` | `hms` | `01:18:30` → `<say-as interpret-as="duration" format="hms">01:18:30</say-as>` |
| Units | `unit` | — | `10 m` → `<say-as interpret-as="unit">10 m</say-as>` |

### 3. Expand Abbreviations with `<sub>`

Common abbreviations should be wrapped:
```xml
<sub alias="for example">e.g.</sub>
<sub alias="that is">i.e.</sub>
<sub alias="versus">vs.</sub>
<sub alias="Doctor">Dr.</sub>
<sub alias="Mister">Mr.</sub>
```

### 4. Add Structural Markup

- Separate paragraphs with `<p>` tags
- Optionally mark sentences with `<s>` tags for fine control
- Insert `<break>` where a natural pause is needed beyond defaults

### 5. Apply Prosody Where Needed

```xml
<!-- Slow down for emphasis -->
<prosody rate="-20%">This is important.</prosody>

<!-- Speak louder -->
<prosody volume="+20%">Attention please!</prosody>

<!-- Raise pitch for excitement -->
<prosody pitch="+10%">Wow, that's amazing!</prosody>

<!-- Combine multiple -->
<prosody rate="slow" pitch="low" volume="soft">Let me explain carefully.</prosody>
```

**Prosody values:**
- **rate**: `x-slow`, `slow`, `medium`, `fast`, `x-fast`, or `±N%`, or multiplier (0.5–2)
- **pitch**: `x-low`, `low`, `medium`, `high`, `x-high`, or `±NHz`, `±Nst`, `±N%`
- **volume**: `silent`, `x-soft`, `soft`, `medium`, `loud`, `x-loud`, or `0`–`100`, or `±N`

### 6. Apply Speaking Styles (Dragon HD Omni or supported neural voices)

```xml
<mstts:express-as style="cheerful">
    That's wonderful news!
</mstts:express-as>

<!-- With intensity -->
<mstts:express-as style="sad" styledegree="2">
    I'm so sorry to hear that.
</mstts:express-as>

<!-- With role -->
<mstts:express-as role="YoungAdultFemale" style="calm">
    Everything will be fine.
</mstts:express-as>
```

**Common styles:** `cheerful`, `angry`, `sad`, `excited`, `friendly`, `empathetic`, `calm`, `chat`, `customerservice`, `narration-professional`, `narration-relaxed`, `newscast`, `whispering`, `shouting`, `hopeful`, `gentle`, `serious`, `embarrassed`, `fearful`, `depressed`.

**Dragon HD Omni adds 100+ styles** including: `chill surfer`, `confused`, `curious`, `determined`, `disgusted`, `emo teenager`, `encouraging`, `grateful`, `joyful`, `mad scientist`, `meditative`, `new yorker`, `news`, `reflective`, `regretful`, `relieved`, `santa`, `shy`, `soft voice`, `surprised`.

**Roles:** `Girl`, `Boy`, `YoungAdultFemale`, `YoungAdultMale`, `OlderAdultFemale`, `OlderAdultMale`, `SeniorFemale`, `SeniorMale`.

**styledegree:** `0.01` (very subtle) to `2` (double intensity). Default is `1`.

## Break and Silence Reference

### `<break>`

| Usage | Result |
|-------|--------|
| `<break/>` | Default 750ms (medium) |
| `<break strength="x-weak"/>` | 250ms |
| `<break strength="weak"/>` | 500ms |
| `<break strength="medium"/>` | 750ms |
| `<break strength="strong"/>` | 1000ms |
| `<break strength="x-strong"/>` | 1250ms |
| `<break time="2s"/>` | Exactly 2 seconds |
| `<break time="500ms"/>` | Exactly 500ms |

Max break time: 20000ms. `time` overrides `strength`.

### `<mstts:silence>`

Types: `Leading`, `Leading-exact`, `Tailing`, `Tailing-exact`, `Sentenceboundary`, `Sentenceboundary-exact`, `Comma-exact`, `Semicolon-exact`, `Enumerationcomma-exact`.

The `-exact` suffix replaces natural silence entirely; without it, the value is added to natural silence.

## Multi-talker Dialogue

For natural conversations between two speakers:

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis"
       xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-MultiTalker-Ava-Andrew:DragonHDLatestNeural">
        <mstts:dialog>
            <mstts:turn speaker="ava">Hello Andrew! How are you?</mstts:turn>
            <mstts:turn speaker="andrew">I'm great, thanks for asking!</mstts:turn>
        </mstts:dialog>
    </voice>
</speak>
```

## HD Voice Parameters

DragonHD and Dragon HD Omni voices support a `parameters` attribute on `<voice>`:

```xml
<voice name="en-US-Ava:DragonHDLatestNeural" parameters="temperature=0.8">
    Hello!
</voice>
```

### Dragon HD Omni Parameters

| Parameter | Default | Range | Purpose |
|-----------|---------|-------|---------|
| `temperature` | 0.7 | 0.3–1.0 | Creativity vs stability |
| `top_p` | 0.7 | 0.3–1.0 | Output diversity filtering |
| `top_k` | 22 | 1–50 | Limits options considered |
| `cfg_scale` | 1.4 | 1.0–2.0 | Relevance and speech speed |

Combine with semicolons: `parameters="top_p=0.8;temperature=0.7;cfg_scale=1.2"`

**Tuning tips:**
- More expressive → increase `temperature`, `top_p`, `top_k` together
- More stable → lower `temperature` first, then `top_p`
- Faster & more relevant → increase `cfg_scale`
- Keep `top_p` equal to `temperature` for best results

## HD Voice SSML Compatibility

Not all SSML elements work with HD voices. Check compatibility before using:

| Element | DragonHD | Dragon HD Omni | Standard Neural |
|---------|----------|---------------|----------------|
| `<voice>` | ✅ | ✅ | ✅ |
| `<mstts:express-as>` | ❌ | ✅ | ✅ (subset) |
| `<lang xml:lang>` | ✅ | ✅ | ✅ (multilingual) |
| `<prosody>` | ❌ | ❌ | ✅ |
| `<emphasis>` | ❌ | ❌ | ✅ (select voices) |
| `<break>` | ❌ | ❌ | ✅ |
| `<mstts:silence>` | ❌ | ❌ | ✅ |
| `<say-as>` | ✅ | ✅ | ✅ |
| `<sub>` | ✅ | ✅ | ✅ |
| `<phoneme>` | ✅ | ❌ | ✅ |
| `<lexicon>` | ✅ (alias only) | ✅ (alias only) | ✅ |
| `<p>`, `<s>` | ✅ | ✅ | ✅ |
| `<audio>` | ❌ | ❌ | ✅ |
| `<bookmark>` | ❌ | ❌ | ✅ |
| `<mstts:backgroundaudio>` | ❌ | ❌ | ✅ |
| `<mstts:audioduration>` | ❌ | ❌ | ✅ |
| `<mstts:viseme>` | ❌ | ❌ | ✅ |

If you need `<prosody>`, `<break>`, `<emphasis>`, or `<audio>`, use a standard neural voice instead of HD.

## Complete Examples

### Example 1: Newsletter with Dates, Numbers, and Pauses

**Input:** "On 10/15/2024, our Q3 revenue hit $2.5M, a 15% increase. Dr. Smith said 'This is incredible growth.' Contact us at (555) 123-4567."

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-Ava:DragonHDLatestNeural">
        <p>
            <s>On <say-as interpret-as="date" format="mdy">10/15/2024</say-as>, our <sub alias="third quarter">Q3</sub> revenue hit <say-as interpret-as="currency">2.5M USD</say-as>, a <say-as interpret-as="cardinal">15</say-as> percent increase.</s>
            <s><sub alias="Doctor">Dr.</sub> Smith said, This is incredible growth.</s>
            <s>Contact us at <say-as interpret-as="telephone">(555) 123-4567</say-as>.</s>
        </p>
    </voice>
</speak>
```

### Example 2: Emotional Dialogue with Styles (Dragon HD Omni)

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis"
       xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-Ava:DragonHDOmniLatestNeural">
        <mstts:express-as style="excited">
            I just got the promotion! Can you believe it?
        </mstts:express-as>
        <mstts:express-as style="grateful">
            Thank you so much for all your support along the way.
        </mstts:express-as>
    </voice>
</speak>
```

### Example 3: Multilingual Content

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis"
       xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-Ava:DragonHDOmniLatestNeural">
        Welcome to our international conference!
        <lang xml:lang="fr-FR">
            Bienvenue à notre conférence internationale!
        </lang>
        <lang xml:lang="de-DE">
            Willkommen zu unserer internationalen Konferenz!
        </lang>
        <lang xml:lang="es-ES">
            ¡Bienvenidos a nuestra conferencia internacional!
        </lang>
    </voice>
</speak>
```

### Example 4: Detailed Prosody Control (Standard Neural only)

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
    <voice name="en-US-AvaMultilingualNeural">
        <prosody rate="slow" pitch="low" volume="soft">
            In the quiet of the evening, the old clock ticked softly.
        </prosody>
        <break time="1s"/>
        <prosody rate="fast" pitch="high" volume="loud">
            Suddenly, the alarm went off!
        </prosody>
    </voice>
</speak>
```

### Example 5: Mathematical Expression

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis"
       xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
    <voice name="en-US-AvaMultilingualNeural">
        <mstts:prompt domain="Math"/>
        The quadratic formula is: x = (-b ± √(b² - 4ac)) / 2a
    </voice>
</speak>
```

## Common Mistakes to Avoid

1. **Unescaped `&` in text** — Always use `&amp;`. `"green & yellow"` must be `"green &amp; yellow"`.
2. **Missing `<voice>` inside `<speak>`** — `<speak>` must contain at least one `<voice>`.
3. **Using `<prosody>` or `<break>` with HD voices** — These are unsupported. Use standard neural voices for prosody control, or rely on HD voices' natural prosody.
4. **Unquoted attribute values** — `<prosody volume=90>` is invalid. Use `<prosody volume="90">`.
5. **Break time exceeding 20s** — Maximum is `20000ms`. Values above this are clamped.
6. **Forgetting mstts namespace** — When using `<mstts:express-as>`, `<mstts:silence>`, or other mstts elements, the namespace `xmlns:mstts` must be declared on `<speak>`.
7. **Phoneme with unrecognized phones** — Returns HTTP 400. Validate phonetic strings against the locale's phone set.

## Reference Files

For detailed information, read these reference files:

- **`references/hd-voices.md`** — Complete list of DragonHD and Dragon HD Omni voices, naming conventions, and capabilities.
- **`references/phonetic-alphabets.md`** — IPA, SAPI, and X-SAMPA phonetic alphabet tables for en-US and other locales. Read this when you need to generate `<phoneme>` elements.
