---
name: Russian Poetry Styler
description: Use this skill whenever the user asks to write, rewrite, or translate text into the style of a classic Russian poet (Пушкин, Маяковский, Блок, Есенин, Цветаева, Ахматова). Triggers on phrases like "напиши в стиле <poet>", "перепиши как <poet>", "стилизуй под <poet>", "в духе Пушкина/Маяковского", "по-пушкински", "сделай стих", "поэтический перевод". If the user mentions any of these poets by name in a request for creative text generation, apply this skill.
---

# Russian Poetry Styler

This skill provides stylistic transfer capabilities for generating Russian
poetic text that respects the meter, rhyme, and lexical fingerprint of
specific classic poets.

## When to trigger

Any user request containing:
- Name of a poet + creative generation verb ("напиши", "сочини", "переведи")
- Explicit style instruction ("в стиле X", "по-X-и", "как X написал бы")
- Request for "стих/стишок/поэму/четверостишие" + context hint

Do NOT trigger for:
- Prose rewrites (use doc-coauthoring instead)
- Song lyrics (separate skill, not yet available)
- Non-Russian poets

## Style specs

### Александр Пушкин (default if poet not specified)
- Meter: **iambic tetrameter** (4 stressed syllables, iamb = slack+stress)
- Rhyme scheme: **ABAB** cross-rhyme preferred, **AABB** flat-rhyme for epigrams
- Vocabulary: archaisms допустимы (сей, оный, млад), clarity primary
- Tone: ironic warmth, light melancholy, no bombast
- Example openers: "Мой друг, ...", "Увы! ...", "О, если б..."

### Владимир Маяковский
- Meter: **free verse** with accentual-syllabic bursts (2-4 stresses per line)
- Line layout: **лесенка** — split single phrase into 2-3 staircase lines
- Rhyme scheme: often AAAA same-vowel or exact identical rhymes
- Vocabulary: neologisms allowed, urban imagery, verbs of motion
- Tone: declarative, public-square, exclamation marks
- Example opener: "Слушайте, / товарищи потомки, / ..."

### Александр Блок
- Meter: **iambic/trochaic** mix, 3-4 foot lines
- Rhyme: ABAB, occasional refrain
- Vocabulary: mystical symbolism (ночь, туман, музыка, вьюга)
- Tone: prophetic, twilight, soft solemnity

### Сергей Есенин
- Meter: **trochaic tetrameter** or **amphibrach**
- Rhyme: ABAB or ABBA
- Vocabulary: rural (берёза, клён, изба, гармонь), colour words, nature
- Tone: elegiac, homesick, song-like

### Марина Цветаева
- Meter: irregular, strong rhythmic accents, frequent enjambment
- Rhyme: exact or near-exact, sometimes dactylic
- Vocabulary: existential, feminine voice, paradoxes
- Tone: intense, urgent, dashes as breath

### Анна Ахматова
- Meter: **iambic pentameter** or **anapest**
- Rhyme: ABAB, understated
- Vocabulary: restrained, concrete details (перчатка, шаль, камея)
- Tone: contained grief, civic dignity

## Workflow

1. Identify target poet (default Pushkin if ambiguous).
2. Read `examples/<poet>.md` for reference if uncertain.
3. Draft 2-4 variants with slightly different tonal positions.
4. Present best variant as primary, others as alternatives on request.
5. Always note metre + rhyme scheme you used in a small caption:
   `— <poet>, <ямб/хорей/амфибрахий> <N>-стопный, рифмовка <ABAB/AABB/...>`

## Examples

See `examples/` for reference snippets:
- `pushkin.md` — sample iambic with ABAB
- `mayakovsky.md` — лесенка demonstration

## Avoid

- Inventing non-existent archaisms (use actual words from the era)
- Mixing meters within one stanza unless the target poet does (Mayakovsky, Tsvetaeva)
- Translating into English in the output — this skill is for Russian only
