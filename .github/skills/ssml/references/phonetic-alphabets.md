# Phonetic Alphabets Reference

Use with the `<phoneme>` element for precise pronunciation control.

## IPA Suprasegmentals

These symbols are used across all locales:

| Symbol | IPA | Note |
|--------|-----|------|
| `ˈ` | Primary stress | Place BEFORE the stressed syllable. Don't use single quote. |
| `ˌ` | Secondary stress | Don't use comma. |
| `.` | Syllable boundary | |
| `ː` | Long vowel | Don't use colon. |
| `‿` | Linking | |

## Phoneme Element Usage

```xml
<phoneme alphabet="ipa" ph="tə.ˈmeɪ.toʊ">tomato</phoneme>
```

Supported alphabets:
- `ipa` — International Phonetic Alphabet (recommended, universal)
- `sapi` — Microsoft Speech API phoneset (en-US, fr-FR, de-DE, es-ES, ja-JP, zh-CN, zh-HK/yue-CN, zh-TW, en-CA, fr-CA, fr-BE, fr-CH, de-AT, de-CH)
- `ups` — Universal Phone Set
- `x-sampa` — Extended SAMPA

## en-US / en-CA Phonemes

### Vowels

| SAPI | IPA | Example |
|------|-----|---------|
| iy | `i` | **ea**t, f**ee**l, vall**ey** |
| ih | `ɪ` | **i**f, f**i**ll |
| ey | `eɪ` | **a**te, g**a**te, d**ay** |
| eh | `ɛ` | **e**very, p**e**t |
| ae | `æ` | **a**ctive, c**a**t |
| aa | `ɑ` | **o**bstinate, p**o**ppy |
| ao | `ɔ` | **o**range, c**au**se |
| uh | `ʊ` | b**oo**k |
| ow | `oʊ` | **o**ld, cl**o**ne, g**o** |
| uw | `u` | **U**ber, b**oo**st, t**oo** |
| ah | `ʌ` | **u**ncle, c**u**t |
| ay | `aɪ` | **i**ce, b**i**te, fl**y** |
| aw | `aʊ` | **ou**t, s**ou**th, c**ow** |
| oy | `ɔɪ` | **oi**l, j**oi**n, t**oy** |
| y uw | `ju` | **Yu**ma, h**u**man, f**ew** |
| ax | `ə` | **a**go, wom**a**n, are**a** |

### R-colored Vowels

| SAPI | IPA | Example |
|------|-----|---------|
| ih r | `ɪɹ` | **ear**s, n**ear** |
| eh r | `ɛɹ` | **air**plane, sc**ar**e |
| uh r | `ʊɹ` | c**ur**e |
| ay r | `aɪɹ` | **Ire**land, ch**oir** |
| aw r | `aʊɹ` | **hour**s, s**our** |
| ao r | `ɔɹ` | **or**ange, s**oar** |
| aa r | `ɑɹ` | **ar**tist, c**ar** |
| er r | `ɝ` | **ear**th, f**ur** |
| ax r | `ɚ` | all**er**gy, supp**er** |

### Consonants

| SAPI | IPA | Example |
|------|-----|---------|
| p | `p` | **p**ut, fla**p** |
| b | `b` | **b**ig, cra**b** |
| t | `t` | **t**alk, sough**t** |
| d | `d` | **d**ig, ro**d** |
| k | `k` | **c**ut, Ira**q** |
| g | `g` | **g**o, dra**g** |
| m | `m` | **m**at, roo**m** |
| n | `n` | **n**o, chicke**n** |
| ng | `ŋ` | li**n**k, si**ng** |
| f | `f` | **f**ork, hal**f** |
| v | `v` | **v**alue, lo**v**e |
| th | `θ` | **th**in, mon**th** |
| dh | `ð` | **th**en, smoo**th** |
| s | `s` | **s**it, fact**s** |
| z | `z` | **z**ap, kid**s** |
| sh | `ʃ` | **sh**e, ru**sh** |
| zh | `ʒ` | plea**s**ure, gara**g**e |
| h | `h` | **h**elp |
| ch | `tʃ` | **ch**in, atta**ch** |
| jh | `dʒ` | **j**oy, oran**g**e |
| l | `l` | **l**id, chi**ll** |
| r | `ɹ` | **r**ed, ta**r** |
| w | `w` | **w**ith |
| y | `j` | **y**ard |

### Stress Notation

SAPI places stress AFTER the vowel of the stressed syllable:
- Primary: `1` after vowel — `burger /b er 1 r - g ax r/`
- Secondary: `2` after vowel — `workforce /w er 1 r k - f ao 2 r s/`

IPA places stress BEFORE the syllable:
- Primary: `ˈ` before syllable — `tomato /tə.ˈmeɪ.toʊ/`

## Examples

```xml
<!-- IPA: stress on second syllable -->
<phoneme alphabet="ipa" ph="tə.ˈmeɪ.toʊ">tomato</phoneme>

<!-- SAPI -->
<phoneme alphabet="sapi" ph="iy eh n y uw eh s">en-US</phoneme>

<!-- UPS -->
<phoneme alphabet="ups" ph="JH AU">Zhou</phoneme>

<!-- X-SAMPA -->
<phoneme alphabet="x-sampa" ph='he."lou'>hello</phoneme>
```

## Important Notes

- Each locale supports a specific phone set. Invalid phones return HTTP 400.
- For IPA, use proper Unicode characters (e.g., `ˈ` not `'` for stress).
- For SAPI, syllable boundaries use `-` and stress is marked with `1` or `2` after the vowel.
- The `<phoneme>` element can only contain text — no nested elements.
- Always provide readable fallback text between the tags.
- `en-CA` does not support SAPI phones.
- DragonHD supports `<phoneme>`. Dragon HD Omni does NOT.
