# Angelus bible packs

Downloadable bible editions for the [Angelus](https://github.com/Bozuk) app.

Each release asset is a standalone SQLite database holding one edition;
`manifest.json` lists them all. The app reads them from
`https://github.com/Bozuk/angelus-packs/releases/download/<tag>/`.

Texts come from the [BibleSuperSearch](https://www.biblesupersearch.com) corpus.
Most are public domain; a few carry their own licence, recorded in each pack's
`pack_meta.attribution`.

`content/` holds the editorial text that accompanies scripture in the app — a
detail and a meditation per reference, in every language they have been written
in. See [`content/README.md`](content/README.md) for the format. It is a build
input rather than a release asset: the app compiles it into its bundled
database.

Everything else here is release assets, not source.
