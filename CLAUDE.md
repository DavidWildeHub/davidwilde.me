# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

David Wilde's personal website (davidwilde.me) — a static site with no build step,
no package manager, and no framework. Pages are hand-written HTML files, most with
inline `<style>`/`<script>` blocks rather than shared assets.

## Development

There is no build, lint, or test tooling in this repo — it's plain static HTML/CSS/JS.

- Preview locally by opening the HTML files directly in a browser, or serve the
  directory with any static file server (e.g. `python -m http.server`) from the repo
  root so relative links resolve correctly.
- Deployment is GitHub Pages, driven by the `CNAME` file (custom domain
  `davidwilde.me`). Pushing to the deployed branch publishes the site — there is no
  CI/build pipeline in between.

## Structure

- [index.html](index.html) — homepage; a hero (name, tagline, live weather chip) plus
  two card-grid sections, `#projects` and `#personal`, each a static list of `.card`
  elements defined directly in the markup (no data-driven rendering here — add new
  entries by copy-pasting a `.card` block).
- [garden.html](garden.html), [nuance-cabinet.html](nuance-cabinet.html) — individual
  project pages, linked from the homepage's Personal/Projects sections respectively.
  Both carry a "← Back to home" link at the top pointing to `index.html`.
- [secret/](secret/) — an unlinked-from-nav page pair (`index.html`, `secret.html`),
  linked only from the homepage's Resume card.
- [responsive-image.css](responsive-image.css) — the one still-shared stylesheet
  (`.responsive-image` class), used by `garden.html`. [style.css](style.css) is legacy
  and currently unreferenced by any page — check before relying on it.
- Root-level `.jpg` files are page assets (garden photos) referenced by relative path
  (with literal spaces in the filenames) from `garden.html` and `index.html`.

## Page conventions

- Add comments to the code that would help a beginner programmer follow along and
  understand different pieces of the program.
- Design language: all three main pages (`index.html`, `garden.html`,
  `nuance-cabinet.html`) share one inline design system — Fraunces (headings) /
  Source Sans 3 (body) / IBM Plex Mono (labels/meta) from Google Fonts, a
  forest-green/amber/paper CSS custom-property palette, and a `@media
  (prefers-color-scheme: dark)` override block that redefines the same variables.
  When adding a page or section, reuse these exact variable names and font stack
  rather than inventing a new palette, and include the dark-mode override block.
- Each page is self-contained: prefer inline `<style>`/`<script>` in the page itself
  over adding new shared assets, unless a style is genuinely reused across multiple
  pages — in which case it belongs in `responsive-image.css` alongside the existing
  shared rule.
- `nuance-cabinet.html` is the pattern for a data-driven interactive page: content
  lives in a single in-page JS array/object (`WORDS`, `CAT_COLORS`), and render
  functions rebuild DOM sections from that data. Follow this pattern for similar
  quiz/browse-style pages rather than introducing a framework or bundler.
- Links between pages are plain relative hrefs (no router). When adding a new page,
  link it from the appropriate `index.html` section (`#projects` or `#personal`) and
  double-check the href matches the actual filename exactly (hyphens vs. dots) — a
  typo here silently 404s since there's no build step to catch it.
