# SootScribe — website

The marketing/landing site for **SootScribe**, a deterministic living-world
simulator for tabletop game masters.

It is a single static page with no build step. Open `index.html` in a browser and
it just works.

## Structure

```
.
├── index.html              ← the entire site (markup + page CSS + a little JS)
├── styles.css              ← design-system entry point (only @import lines)
├── tokens/                 ← design tokens (colours, type, spacing, fonts, recipes)
│   ├── fonts.css
│   ├── colors.css
│   ├── typography.css
│   ├── spacing.css
│   └── recipes.css
├── assets/
│   ├── logo-mark.svg       ← the "Chamberstick" logo (favicon + inline mark)
│   └── fonts/              ← self-hosted Iowan Old Style (the in-world serif)
├── .github/workflows/deploy.yml   ← deploys to GitHub Pages on every push to main
├── .nojekyll               ← tell Pages to serve files as-is (no Jekyll)
└── CLAUDE.md               ← the brief / task list for an AI agent
```

No framework, no bundler, no `npm install`. The page links `styles.css`, which
`@import`s the token files; the Iowan serif loads from `assets/fonts/`, and Inter +
JetBrains Mono load from Google Fonts.

## Run locally

Because the page fetches `styles.css` and the fonts over HTTP, open it through a
tiny static server rather than `file://`:

```bash
# any one of these, from the repo root:
python3 -m http.server 8000        # → http://localhost:8000
npx serve .
```

## Deploy (GitHub Pages)

Two options — pick one:

**A. Actions workflow (included, recommended).** Push to `main`; the workflow in
`.github/workflows/deploy.yml` publishes the site. In the repo: **Settings → Pages
→ Build and deployment → Source = GitHub Actions**.

**B. Branch source.** **Settings → Pages → Source = Deploy from a branch → `main` /
`(root)`**. No workflow needed.

Custom domain: add a `CNAME` file containing the domain, and set it under
Settings → Pages.

## What still needs real values

The site ships with **placeholder** download links and release data. Search for
these and replace them:

| What | Where | Currently |
| --- | --- | --- |
| macOS download | `index.html`, `data-dl="mac"` button + `.alt` links | `href="#"` |
| Windows download | `index.html`, `data-dl="win"` button + `.alt` links | `href="#"` |
| Install notes link | `index.html`, `.dl-foot` | `href="#"` |
| Version / build name | `index.html`, `#download` heading + paragraph | `v1.4.0 — "The Forecast"` |
| File sizes / requirements | `index.html`, each `.dl` card (`.req`, `.meta` chips) | placeholder MB |
| Release notes | `index.html`, `#news` `<article class="release">` blocks | sample entries |

When you wire real download URLs, also remove the click-blocker at the bottom of
`index.html` (the `document.querySelectorAll('[data-dl]') … e.preventDefault()`
block) so the links actually navigate.

## Editing the design

The look is driven entirely by the design tokens in `tokens/`. Colours, fonts,
spacing, radii and shadows are CSS custom properties (`--ss-*`). The site supports
two themes — **Ink** (dark, default) and **Vellum** (light) — toggled by the
sun/moon button, which sets `data-theme` on `<html>` and persists to
`localStorage`. Prefer changing the tokens over hard-coding values in `index.html`.
