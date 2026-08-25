# Verse content

Editorial text that accompanies scripture in the Angelus app. Four kinds hang
off a reference:

- **detail** — what the passage is: who wrote it, what it answers, how it is
  built.
- **spiritual** — how it is read in the light of the whole of scripture.
- **modern** — what it says to the world the reader lives in today.
- **meditation** — what to do with it now.

They fill the four sections of the app's verse screen and chapter reader, in
that order.

## Format

One CSV, `verse-content.csv`:

```csv
book,chapter,verse,language,detail,spiritual,modern,meditation
1,1,1,fr,"Premier verset de la Torah…","La Bible s'ouvre sur…","Dans un monde…","Seigneur, donne-moi…"
19,23,0,fr,"Psaume attribué à David…",,,"Relis-le lentement…"
```

| Column | Meaning |
| --- | --- |
| `book` | 1-66, Genesis to Revelation, in the BibleSuperSearch numbering |
| `chapter` | 1-based |
| `verse` | the verse, or **0 for the whole chapter** |
| `language` | ISO 639-1 code — `fr`, `en`, `es`, `pt`, or any other |
| `detail` | may be empty |
| `spiritual` | may be empty |
| `modern` | may be empty |
| `meditation` | may be empty |

One row carries all four columns for one reference in one language, and any of
them may be blank — but not all four. Rows are keyed by reference alone, never
by edition: a note about Psalm 23 is equally true of Segond and of the World
English Bible.

Each text exists **per language**, with no obligation to cover them all. The app
uses the reader's language when it has been written, English otherwise, and
draws **no heading at all** when neither exists — an empty section is worse than
an absent one. The fallback is per section, so a half-translated reference
degrades one paragraph at a time.

## Status

This file is a **seed, not the finished corpus**. Its meditations are, for the
most part, the app's twelve thematic ones spread over 127 curated references; a
few references carry a hand-written detail; and Genesis 1 carries, in French,
its two visions across all 31 verses, as a worked example of the shape expected.
It is meant to be replaced wholesale by the authored CSV.

## How the app consumes it

Two ways, from the same file.

`npm run db:build` in the app repository loads it into the `verse_notes` table
of the bundled database — that is the seed a fresh install opens with.

`node scripts/build-packs.mjs --content-only` turns it into one **content pack**
per language (`content-fr.db`, ~85 kB), published to this repository's releases
alongside the bible packs. An installed app compares the published hash with its
own and replaces the language wholesale when they differ, so **rewriting this
file does not need a new app build** — only a republication.

A malformed row stops the build rather than silently shipping a blank section —
a reference outside books 1-66, an invalid language code, a duplicate row, or a
row with all four columns empty each raise an error naming the line number.

## The two copies

This file lives here, where it is written, and in the app repository as
`data/verse-content.csv`, which is what the build reads. They are kept together
by `scripts/sync-content.mjs` on the app side:

```bash
npm run content:sync    # fetch what was rewritten here (also runs before db:build)
npm run content:push    # send what was changed there
```

The sync records what it last saw, so it can tell "rewritten upstream" from
"edited locally" from a genuine conflict, and refuses to overwrite either side
without `--force`. Editing this file in GitHub's web interface is the expected
way to write; nothing else is needed.
