# HD Voices Reference

Complete list of Azure Speech HD voices and their capabilities.

## DragonHD Voices

Format: `{locale}-{Name}:DragonHDLatestNeural`

| Voice Name | Gender | Status | Notes |
|-----------|--------|--------|-------|
| de-DE-Florian:DragonHDLatestNeural | Male | GA | |
| de-DE-Seraphina:DragonHDLatestNeural | Female | GA | |
| en-US-Adam:DragonHDLatestNeural | Male | GA | |
| en-US-Alloy:DragonHDLatestNeural | Male | GA | |
| en-US-Andrew:DragonHDLatestNeural | Male | GA | |
| en-US-Andrew2:DragonHDLatestNeural | Male | GA | Conversational |
| en-US-Andrew3:DragonHDLatestNeural | Male | Preview | Podcast |
| en-US-Aria:DragonHDLatestNeural | Female | GA | |
| en-US-Ava:DragonHDLatestNeural | Female | GA | |
| en-US-Ava3:DragonHDLatestNeural | Female | Preview | Podcast |
| en-US-Brian:DragonHDLatestNeural | Male | GA | |
| en-US-Davis:DragonHDLatestNeural | Male | GA | |
| en-US-Emma:DragonHDLatestNeural | Female | GA | |
| en-US-Emma2:DragonHDLatestNeural | Female | GA | Conversational |
| en-US-Jenny:DragonHDLatestNeural | Female | GA | |
| en-US-MultiTalker-Ava-Andrew:DragonHDLatestNeural | Both | Preview | Multi-speaker dialogue |
| en-US-Nova:DragonHDLatestNeural | Female | GA | |
| en-US-Phoebe:DragonHDLatestNeural | Female | GA | |
| en-US-Serena:DragonHDLatestNeural | Female | GA | |
| en-US-Steffan:DragonHDLatestNeural | Male | GA | |
| es-ES-Tristan:DragonHDLatestNeural | Male | GA | |
| es-ES-Ximena:DragonHDLatestNeural | Female | GA | |
| fr-FR-Remy:DragonHDLatestNeural | Male | GA | |
| fr-FR-Vivienne:DragonHDLatestNeural | Female | GA | |
| ja-JP-Masaru:DragonHDLatestNeural | Male | GA | |
| ja-JP-Nanami:DragonHDLatestNeural | Female | GA | |
| zh-CN-Xiaochen:DragonHDLatestNeural | Female | GA | |
| zh-CN-Yunfan:DragonHDLatestNeural | Male | GA | |

### DragonHD Parameters

| Parameter | Default | Range | Purpose |
|-----------|---------|-------|---------|
| `temperature` | 1.0 | 0–1 | Controls randomness/variation |

Usage: `<voice name="en-US-Ava:DragonHDLatestNeural" parameters="temperature=0.8">`

### DragonHD SSML Support

Supported: `<voice>`, `<lang>`, `<say-as>`, `<sub>`, `<phoneme>`, `<lexicon>` (alias only), `<p>`, `<s>`

NOT supported: `<prosody>`, `<break>`, `<emphasis>`, `<audio>`, `<mstts:express-as>`, `<mstts:silence>`, `<mstts:backgroundaudio>`, `<mstts:audioduration>`, `<bookmark>`, `<mstts:viseme>`

## Dragon HD Omni Voices

Format: `{locale}-{Name}:DragonHDOmniLatestNeural`

Dragon HD Omni includes 700+ voices — any existing neural voice can be upgraded by appending `:DragonHDOmniLatestNeural`.

**Examples:**
| Standard Neural Voice | Omni Version |
|-----------------------|-------------|
| en-US-AvaNeural | en-US-Ava:DragonHDOmniLatestNeural |
| de-DE-ConradNeural | de-DE-Conrad:DragonHDOmniLatestNeural |
| ja-JP-NanamiNeural | ja-JP-Nanami:DragonHDOmniLatestNeural |

Full voice list: https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/Blog-Samples/Introducing-Dragon-HD-Omni/dragonhdomni_voice_list.json

### Dragon HD Omni Parameters

| Parameter | Default | Range | Purpose |
|-----------|---------|-------|---------|
| `temperature` | 0.7 | 0.3–1.0 | Creativity vs stability |
| `top_p` | 0.7 | 0.3–1.0 | Output diversity filtering |
| `top_k` | 22 | 1–50 | Limits options considered |
| `cfg_scale` | 1.4 | 1.0–2.0 | Relevance and speech speed |

Usage: `<voice name="en-US-Ava:DragonHDOmniLatestNeural" parameters="top_p=0.8;temperature=0.7">`

### Dragon HD Omni Styles (100+)

Available on `en-US-Ava` and `en-US-Andrew` via `<mstts:express-as style="...">`:

`angry`, `chill surfer`, `confused`, `curious`, `determined`, `disgusted`, `embarrassed`, `emo teenager`, `empathetic`, `encouraging`, `excited`, `fearful`, `friendly`, `grateful`, `joyful`, `mad scientist`, `meditative`, `narration`, `neutral`, `new yorker`, `news`, `reflective`, `regretful`, `relieved`, `sad`, `santa`, `shy`, `soft voice`, `surprised`

Style results are content-dependent: the model adapts style application based on semantic meaning.

### Dragon HD Omni SSML Support

Supported: `<voice>`, `<mstts:express-as>`, `<lang>`, `<say-as>`, `<sub>`, `<lexicon>` (alias only), `<p>`, `<s>`

NOT supported: `<prosody>`, `<break>`, `<emphasis>`, `<audio>`, `<phoneme>`, `<mstts:silence>`, `<mstts:backgroundaudio>`, `<mstts:audioduration>`, `<bookmark>`, `<mstts:viseme>`, `<mstts:ttsembedding>`

## Dragon HD Flash Voices

Optimized for Chinese and English. Available in `eastus`, `westeurope`, `southeastasia`, `chinaeast2`, `chinanorth2`, `chinanorth3`.

| Voice Name | Gender |
|-----------|--------|
| zh-CN-Xiaochen:DragonHDFlashLatestNeural | Female |
| zh-CN-Xiaoxiao:DragonHDFlashLatestNeural | Female |
| zh-CN-Xiaoxiao2:DragonHDFlashLatestNeural | Female |
| zh-CN-Yunxia:DragonHDFlashLatestNeural | Male |
| zh-CN-Yunxiao:DragonHDFlashLatestNeural | Male |
| zh-CN-Yunye:DragonHDFlashLatestNeural | Male |
| zh-CN-Yunyi:DragonHDFlashLatestNeural | Male |

## Choosing Between Models

| Consideration | DragonHD | Dragon HD Omni | Standard Neural |
|--------------|----------|---------------|----------------|
| Voice count | 30+ | 700+ | 500+ |
| SSML support | Partial | Partial | Full |
| Styles | ❌ | ✅ 100+ | ✅ (subset per voice) |
| Prosody control | ❌ | ❌ | ✅ |
| Break/pause control | ❌ | ❌ | ✅ |
| Phoneme support | ✅ | ❌ | ✅ |
| Temperature tuning | ✅ | ✅ (+ top_p, top_k, cfg_scale) | ❌ |
| Word boundary events | ❌ | ✅ | ✅ |
| Multi-talker | ✅ | ❌ | ❌ |
| Quality | Highest | High | Good |
| Latency | <300ms | <300ms | <300ms |
