# BeerReel — legal documents

The public legal documents for the [BeerReel](https://github.com/BeerReel) app.

| File | |
|---|---|
| [privacy.html](./privacy.html) | Privacy Policy |
| [terms.html](./terms.html) | Terms & Conditions |
| [delete-account.html](./delete-account.html) | How to delete your account, and what is removed (the URL Google Play lists) |
| [SUPPORT.md](./SUPPORT.md) | Support page and FAQ |
| [osm-pubs-odbl.csv](./osm-pubs-odbl.csv) | The ODbL extract of pub data derived from OpenStreetMap |

## These files are live

The app does not ship a copy of them. `app/src/legal.js` fetches them from
`raw.githubusercontent.com/BeerReel/BeerReel-Privacy/main` at run time and
`LegalScreen.js` renders them natively, so **an edit here reaches every user
without an app release** — and a broken edit reaches them just as fast.

`privacy.html` is the canonical policy. Do not keep a second copy of it in this
repo; the one that used to live in this README drifted from it within days.

## Editing them

`LegalScreen.js` parses the HTML with a small regex parser, not a browser. Stay
inside what it understands:

- **Blocks:** `<h1>`, `<h2>`, `<p>`, `<ul>`/`<li>` only. Anything else — `<ol>`,
  `<h3>`, `<table>`, `<section>` — is dropped silently, along with its contents.
- **Inline:** `<strong>` and `<a href>` only. An `<em>` or `<b>` does not
  degrade gracefully; it renders as visible garbage.
- **No nested lists**, and **no links inside a heading** (the href is discarded).
- **Entities:** only `&amp; &nbsp; &lt; &gt; &quot; &#39; &mdash; &rsquo;` are
  decoded. Write literal UTF-8 for everything else — `©`, `—`, `→`.
- **Links** must be absolute `https://` or `mailto:`. The only relative hrefs
  that resolve are ones containing `privacy` or `terms`.
- Terms section numbers are typed into the `<h2>` text by hand, because `<ol>`
  would vanish.

Numbered sections in the Terms are cross-referenced by number in a few places —
renumbering means re-checking those references.

After editing, open both files in a browser, then check them in the app: the
screens fetch `main`, so the change has to be pushed before that test means
anything.

## Attribution

Pub data comes from OpenStreetMap (ODbL) and the Food Standards Agency (OGL
v3.0). Both licences require a credit the user can actually see, so it is
rendered by `app/src/components/DataCredit.js` in the app itself rather than
from here — the obligation does not lapse because GitHub is unreachable. The
credits in `privacy.html` are for people reading the documents on the web, where
that component never runs. Keep both.

`osm-pubs-odbl.csv` is the ODbL share-alike extract. It is regenerated on the
first of each month by `.github/workflows/import-pubs.yml` in the app repo,
which runs `api/import_pubs.py --export-odbl` and uploads the result as a build
artifact. Copy that artifact here when it changes.
