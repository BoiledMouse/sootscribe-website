# Brief for Claude — publish the SootScribe website

You're helping set up and ship the **SootScribe** website from this repository.
SootScribe is a deterministic living-world simulator for tabletop RPG game masters;
this repo is its landing page. The page is already designed and built — your job is
to get it live and replace placeholder data with real values. **Do not redesign
it.**

## What this repo is

A single static HTML page, no build step. `index.html` contains the markup, the
page CSS, and a little vanilla JS (theme toggle, OS detection, logo injection). It
links `styles.css`, which `@import`s the design tokens in `tokens/`. Fonts: Iowan
Old Style is self-hosted in `assets/fonts/`; Inter + JetBrains Mono come from
Google Fonts. See `README.md` for the full file map.

## Tasks

1. **Commit and push** all files in this repo to the default branch (`main`).
   Keep the directory structure exactly as-is — `styles.css`, `tokens/` and
   `assets/` must stay at the repo root next to `index.html`, because the paths
   are relative.

2. **Enable GitHub Pages.** Prefer the included Actions workflow
   (`.github/workflows/deploy.yml`): set **Settings → Pages → Source = GitHub
   Actions**. Confirm the first deploy succeeds and the site loads, including the
   serif headings (Iowan Old Style) and both themes (the sun/moon toggle).

3. **Replace placeholder download data** in `index.html`. The real installer URLs,
   version string, file sizes and system requirements need to go in. The exact
   locations are listed in `README.md` under "What still needs real values". After
   wiring real `href`s on the `[data-dl]` links, **remove** the small script block
   near the bottom of `index.html` that calls `e.preventDefault()` on every
   `[data-dl]` element (it exists only to keep the placeholder links inert):

   ```js
   // Download buttons are placeholders in this preview
   document.querySelectorAll('[data-dl]').forEach(function (el) {
     el.addEventListener('click', function (e) { e.preventDefault(); });
   });
   ```

4. **Update the release notes** in the `#news` section to match real releases
   (version, date, and the `New` / `Improved` / `Fixed` line items). Keep the
   existing markup pattern for each `<article class="release">`.

5. **(Optional) Install-notes link.** The `.dl-foot` paragraph links to "install
   notes" with `href="#"`. Either point it at a real page or remove that link.

6. **(Optional) Custom domain.** If the user wants one, add a `CNAME` file at the
   repo root containing the bare domain and set it under Settings → Pages.

## Hard rules

- **Don't change the visual design, layout, copy tone, or the design tokens** in
  `tokens/` unless explicitly asked. The brand is "ink and ember, vellum and
  lamplight": a warm-neutral surface ramp, a single ember accent used sparingly,
  serif for in-world voice and sans for UI. Restraint is the point.
- Keep it a **zero-build static site**. Don't introduce a framework, bundler, or
  `package.json`. It must keep working by just opening `index.html`.
- Don't add analytics, trackers, or sign-up flows — the page explicitly promises
  "no sign-up, no telemetry". Keep that true.
- There is **no public source repo to link to**; don't add GitHub/source links to
  the page.

## Quick local check

```bash
python3 -m http.server 8000   # then open http://localhost:8000
```

Verify: headings render in the serif, the ember accent shows on "Scribe" and the
first-move block, the theme toggle flips Ink↔Vellum, and the Download cards
highlight the visitor's OS.
