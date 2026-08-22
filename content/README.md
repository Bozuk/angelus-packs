# Verse content

Editorial text that accompanies scripture in the Angelus app. Two kinds hang
off a reference:

- **detail** — what the passage is: who wrote it, what it answers, how it is
  built.
- **meditation** — what to do with it today.

They fill the two sections of the app's verse screen and chapter reader.

## Format

One CSV, `verse-content.csv`:

```csv
book,chapter,verse,language,detail,meditation
1,1,1,fr,"Premier verset de la Torah…","Contemple aujourd'hui…"
19,23,0,fr,"Psaume attribué à David…","Relis-le lentement…"
```

| Column | Meaning |
| --- | --- |
| `book` | 1-66, Genesis to Revelation, in the BibleSuperSearch numbering |
| `chapter` | 1-based |
| `verse` | the verse, or **0 for the whole chapter** |
| `language` | ISO 639-1 code — `fr`, `en`, `es`, `pt`, or any other |
| `detail` | may be empty |
| `meditation` | may be empty |

One row carries both columns for one reference in one language, and either may
be blank — but not both. Rows are keyed by reference alone, never by edition: a
note about Psalm 23 is equally true of Segond and of the World English Bible.

Each text exists **per language**, with no obligation to cover them all. The app
uses the reader's language when it has been written, English otherwise, and
draws **no heading at all** when neither exists — an empty section is worse than
an absent one.

## Status

This file is a **seed, not the finished corpus**. Its meditations are the app's
twelve thematic ones spread over 127 curated references; only two references
carry a hand-written detail, as a worked example of the verse and whole-chapter
shapes. It is meant to be replaced wholesale by the authored CSV.

## How the app consumes it

The file is a build input, not a runtime download: `npm run db:build` in the app
repository loads it into the `verse_details` and `verse_meditations` tables of
the bundled database. Updating the text therefore needs a new app build, unlike
the bible editions in this repository's releases.

A malformed row stops that build rather than silently shipping a blank section —
a reference outside books 1-66, an invalid language code, a duplicate row, or a
row with both columns empty each raise an error naming the line number.
