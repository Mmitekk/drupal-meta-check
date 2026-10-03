# drupal-meta-check

<div align="center">

<strong>English</strong> · [<img height="12" src="https://img.shields.io/badge/Русский-grey" alt="Русский">](README.ru.md)

</div>

A single-file, zero-dependency HTML tool for checking the live `<title>`, meta `description` and `H1` of a Drupal (or any) website against a saved SEO audit snapshot. Built for the routine "I've edited the titles — did they actually deploy?" check.

## How it works

The audit table (URL, response code, language, H1, Title, Description, Title/Description duplicate counts, text length) is embedded in `index.html` itself. A button at the top fetches every page live and compares it with the report, field by field.

## Features

- **Check live data** — fetches each page (through public CORS proxies, concurrency 4) and shows three status marks per row: H1 / Title / Description, each `=` (match) or `≠` (differs from the report).
- **Suspected 404 detection** — Drupal serves the homepage for missing URLs; the tool flags pages whose canonical points to the homepage or whose title says "Page not found".
- **Drafts** — every row expands into an editor where you can type the new Title / Description, see the character count (with the recommended 40–70 / 120–180 range marker), and after deploying compare the draft against the live page. Drafts are kept in `localStorage`.
- **Soft comparison** — ignores case, punctuation and extra whitespace by default; switch to strict character-level comparison with one checkbox.
- **Filters & export** — search by URL, filter by status (differs / matches / errors / unchecked / draft mismatch), export everything to CSV.
- **Replaceable data** — paste any other audit as a JSON array (Details → "Report data") to reuse the tool for another site.

## Getting started

1. Open `index.html` in a browser — it works straight from disk, or via GitHub Pages.
2. Click **Check live data** and wait (about a minute per 100 pages through the proxies).
3. Click any row to expand the side-by-side view: report value vs live value, plus draft editors.

No build step, no dependencies, no backend. Nothing is sent anywhere except the page fetches themselves; results and drafts stay in your browser.

## Universal mode — check any site

The embedded snapshot is just an example. To check your own site: open **Own check** under the toolbar, paste a `sitemap.xml` address (or just a list of URLs), and click **Create check**. Then:

1. Let the live check finish — pages appear with the `○ no baseline` mark.
2. Press **Pin baseline** — the fetched values become the comparison base.
3. After editing pages on the site, click **Check live data** again — the rows show exactly what changed since the snapshot (`=` / `≠`).

The current session (URL list, baseline, drafts, live results) is stored in the browser and survives page reloads; **Reset to embedded data** returns the built-in report.

## Updating the audit data

Open **Report data** under the toolbar and paste a JSON array:

```json
[
  {
    "url": "https://example.com/",
    "code": "200",
    "lang": "ru",
    "h1": "…",
    "title": "…",
    "description": "…",
    "dupTitle": "0",
    "dupDescription": "0",
    "textLength": "6930"
  }
]
```

Only `url` is required. The imported data is stored in the browser; **Reset to embedded data** restores the original snapshot. `urls.txt` in the repo root is a plain list of the audited URLs.

## GitHub Pages

Every push to `main` deploys the site automatically via a GitHub Actions workflow (`.github/workflows/pages.yml`) — no manual setup needed.

Live version: **https://mmitekk.github.io/drupal-meta-check/**

If Pages was never enabled for the repository and the workflow cannot toggle it itself, enable it once manually: **Settings → Pages → Source: "GitHub Actions"**, then re-run the workflow (Actions → Deploy to GitHub Pages → Run workflow).

## Privacy

The embedded snapshot contains only public page metadata of a public website — no tokens, keys, credentials or private report links.

## License

[MIT](LICENSE)
