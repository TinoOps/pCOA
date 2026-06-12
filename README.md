# Project Code of Accounts

A self-contained, print-ready **Code of Accounts** viewer. The entire application —
markup, styling, logic, and seed data — lives in a single `index.html` file with no
build step. Open it in any modern browser and it works.

It renders a paginated, letter-sized report that maps a project's **Work Breakdown
Structure (WBS)** against the **General Ledger (GL) Chart of Accounts**, with a
searchable GL dictionary and one-click activity-code copying. Built originally for
DMC Mining Services projects; descriptions are bilingual (English / Spanish).

## Features

- **Three report views** — Expenses, Labour, and the combined Code of Accounts, each
  with independent filter state.
- **Rich filtering** — multi-select on WBS L1 / L2 / Item and GL L1 / L2 headers, plus
  free-text search on GL description, activity code, and the GL dictionary
  (definitions, inclusions, exclusions).
- **GL dictionary tooltips** — click any GL cell to see its header, definition,
  inclusions, and exclusions.
- **Click-to-copy** — click any activity code to copy it to the clipboard.
- **Print / Save as PDF** — carefully paginated for US Letter; the on-screen pages are
  the printed pages (no surprise blank sheets).
- **Setup panel** — import your own data from `.xlsx`, `.xls`, or `.csv`:
  - *Application reference data* (shared across all projects): Standardized WBS,
    GL Chart of Accounts, GL Dictionary.
  - *Project data* (per project): Tasks and Costs, plus project code/name.
- **Local persistence** — imports are saved in the browser's `localStorage`
  (`coa.corporate.v1`, `coa.project.v1`, `coa.imports.v1`). Bundled seed data loads on
  first run and is overridden the moment you import your own.

## Quick start

Local use needs nothing more than a browser:

```bash
# clone, then either double-click index.html or serve it
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Deploy to GitHub Pages

A workflow is included at `.github/workflows/pages.yml` that publishes the site on
every push to `main`. To turn it on:

1. Push this repo to GitHub.
2. Go to **Settings → Pages → Build and deployment** and set **Source** to
   **GitHub Actions**.
3. Push to `main`. The site goes live at
   `https://<your-user>.github.io/<repo-name>/`.

Because the app is a single static file, you can alternatively use
**Settings → Pages → Deploy from a branch** without any workflow.

## Dependencies

There is no package manager and nothing to install. At runtime the page pulls two
things from the network:

- **Google Fonts** (IBM Plex Mono, Source Sans 3, Barlow Condensed) via CSS `@import`.
- **SheetJS (xlsx 0.18.5)** lazy-loaded from cdnjs **only** when importing an `.xlsx`
  file.

**Air-gapped / offline networks:** CSV import is parsed natively and needs no network,
so saving a sheet as `.csv` always provides a working import path even when cdnjs is
unreachable. Fonts will fall back to system fonts if Google Fonts is blocked.

## Data import format

Each importer expects a header row followed by data rows. The exact column headers the
parser looks for are defined inline in `index.html` (search for the import
configuration). The first worksheet of a workbook is used. Currency-formatted cells are
read as raw numbers to avoid `"$47,692"`-style strings.

## Project structure

```
.
├── index.html                  # the entire application + bundled seed data
├── README.md
├── LICENSE
├── CHANGELOG.md
├── .gitignore
└── .github/workflows/pages.yml # optional GitHub Pages deploy
```

## A note on the bundled data

`index.html` ships with roughly 2.9 MB of embedded seed data (the WBS structure and GL
chart). **If you publish this repository publicly, that data becomes public too.** If
the seed reflects real internal financials, consider one of:

- making the repository **private**, or
- replacing the embedded `SEED` object in `index.html` with sample/placeholder data and
  letting users load real data through the Setup panel.

## License

See [LICENSE](LICENSE). It currently defaults to **All Rights Reserved** (the safe
default for internal tooling). Swap it for MIT/Apache-2.0/etc. if you intend to open
this up.
