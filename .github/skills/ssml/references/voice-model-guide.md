# Azure Speech voice model guide

Use this guide to pick a voice family **before** you decide which SSML tags to emit.

## Quick rule

- If the user wants explicit pacing, prosody, pauses, emphasis, phonemes, or audio tags, prefer a **standard Azure neural voice**.
- If the user wants **HD voice quality plus phoneme-based pronunciation**, use **DragonHD** and avoid timing/prosody tags.
- If the user wants **HD voice quality plus styles, roles, or multilingual accent steering**, use **Dragon HD Omni** and avoid classic timing/prosody tags.

## Capability matrix

| Voice family | Best for | Supports well | Avoid these tags | Example voice |
|---|---|---|---|---|
| Standard neural | General Azure SSML authoring with the broadest control | `<prosody>`, `<break>`, `<emphasis>`, `<phoneme>`, `<say-as>`, `<sub>`, `<audio>`, `<p>`, `<s>` | Check voice-specific style support before assuming `mstts:express-as` works | `en-US-AvaNeural` |
| DragonHD | HD output when you still need pronunciation control | `<voice>`, `<lang>`, `<phoneme>`, `<lexicon>` (alias support), `<say-as>`, `<sub>`, `<p>`, `<s>` | `<mstts:express-as>`, `<prosody>`, `<emphasis>`, `<audio>`, `<break>`, `<mstts:silence>` | `en-US-Ava:DragonHDLatestNeural` |
| Dragon HD Omni | HD output with styles, roles, and multilingual behavior | `<voice>`, `<mstts:express-as>`, `<lang>`, `<lexicon>` (alias support), `<say-as>`, `<sub>`, `<p>`, `<s>` | `<prosody>`, `<emphasis>`, `<audio>`, `<break>`, `<mstts:silence>`, `<phoneme>` | `en-US-Ava:DragonHDOmniLatestNeural` |

## Style and role guidance

Use `mstts:express-as` when the user asks for things like:

- friendly
- calm
- documentary narration
- customer service
- excited
- sad
- angry
- gentle

Important constraints:

- `style` is required.
- `styledegree` is optional and ranges from `0.01` to `2`.
- `role` is optional and voice-specific.
- Style support is voice-specific. If support is uncertain, either:
  - choose a voice known to support styles, or
  - say the exact style depends on the selected voice.

## Multilingual guidance

Use `<lang xml:lang="...">` only when the selected voice family supports language switching.

Practical defaults:

- For mixed-language text without a strong voice preference, choose a multilingual standard neural voice if available.
- Dragon HD Omni voices support multilingual input and `<lang>`-based accent steering.
- If the chosen voice does not support the requested language, Azure can fail to produce audio.

## Conflict resolution

These requests need an explicit tradeoff:

### User wants Dragon HD Omni plus pauses and prosody

Problem: Dragon HD Omni supports styles, but not classic `<prosody>` or `<break>` tags.

Best response:

1. Explain the conflict in one sentence.
2. Offer two valid paths:
   - keep Dragon HD Omni and rely on wording plus `mstts:express-as`
   - switch to a standard neural voice to unlock `<prosody>` and `<break>`

### User wants DragonHD plus style tags

Problem: DragonHD does not support `mstts:express-as`.

Best response:

- either switch to Dragon HD Omni or a style-capable standard neural voice
- or keep DragonHD and simplify to neutral speech plus supported pronunciation tags

### User wants exact phoneme control on Dragon HD Omni

Problem: Dragon HD Omni does not support `<phoneme>`.

Best response:

- switch to a standard neural voice or DragonHD
- or use `<sub>` when a rough spoken alias is good enough

## Optional voice attributes

The `voice` element can also carry attributes such as `effect` for scenario tuning.

Example:

```xml
<voice name="en-US-AvaMultilingualNeural" effect="eq_car">
  Navigation will begin in ten seconds.
</voice>
```

Use effect attributes only when the user actually cares about that playback context.
