# Pronunciation and language guide

Use this file when the request is about pronunciation, acronyms, dates and numbers, or mixed-language speech.

## Pick the lightest pronunciation tool

### Use `<sub>` for simple spoken aliases

Best when:

- the user wants a common spoken form
- a phonetic spelling would be overkill
- exact IPA is unnecessary

Example:

```xml
The <sub alias="Sequel">SQL</sub> migration finished successfully.
```

### Use `<phoneme>` for exact sounds

Best when:

- a word is repeatedly mispronounced
- brand names, people, or borrowed words need precision
- the user explicitly asks for IPA or phonetic control

Example:

```xml
<phoneme alphabet="ipa" ph="tə.ˈmeɪ.toʊ">tomato</phoneme>
```

Rules:

- `alphabet` must be lowercase.
- Supported values include `ipa`, `sapi`, `ups`, and `x-sampa`.
- Each locale supports a specific phone set. Invalid phones can cause Azure to return an HTTP 400 error.

Stress rules:

- In IPA, stress markers go before the stressed syllable.
- If you stress one syllable in IPA, mark the syllables clearly rather than leaving the rest ambiguous.

## Use custom lexicons when many terms repeat

Custom lexicons are better than repeating the same `<phoneme>` tags everywhere.

Key constraints:

- Use a public `.xml` or `.pls` file via `uri`.
- One lexicon is limited to one locale.
- Maximum size is 100 KB.
- Lexicon entries are case-sensitive.
- If both `alias` and `phoneme` exist for the same grapheme, `alias` wins.
- Azure caches a lexicon URI for up to 15 minutes.

Reference pattern:

```xml
<speak version="1.0"
       xmlns="http://www.w3.org/2001/10/synthesis"
       xml:lang="en-US">
  <voice name="en-US-AvaNeural">
    <lexicon uri="https://www.example.com/customlexicon.xml" />
    BTW, we will be there tomorrow morning.
  </voice>
</speak>
```

## Use `<say-as>` for structured text

`<say-as>` is usually better than spelling things out manually.

Common patterns:

```xml
<say-as interpret-as="cardinal">42</say-as>
<say-as interpret-as="ordinal">21</say-as>
<say-as interpret-as="date" format="mdy">05/12/2026</say-as>
<say-as interpret-as="time">3:45 PM</say-as>
<say-as interpret-as="telephone">8005550199</say-as>
<say-as interpret-as="characters">SSML</say-as>
<say-as interpret-as="spell-out">Govind</say-as>
```

Use `format` only when the content is ambiguous enough to need it.

## Multilingual guidance

Use `<lang xml:lang="...">` for short spans that change language or accent:

```xml
<voice name="en-US-AvaMultilingualNeural">
  Welcome back.
  <lang xml:lang="es-MX">Gracias por acompanarnos.</lang>
</voice>
```

Best practices:

- Keep the root `xml:lang` aligned with the dominant language.
- Wrap only the spans that change.
- Do not use `<lang>` on a voice that does not support language switching.
- If the voice does not support the requested language, Azure can fail to synthesize audio.

## Practical defaults

- If the user says "fix pronunciation", start with `<sub>` unless they clearly need exact phonetics.
- If they say "use IPA", switch to `<phoneme alphabet="ipa">`.
- If the same term appears many times, recommend a custom lexicon.
- If the request mixes languages, pick a multilingual voice first and then add `<lang>` spans.
