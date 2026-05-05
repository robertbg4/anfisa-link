# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Dev server

The site is plain static HTML/CSS — there is no build step, no package manager, no test runner.

Serve the repo root locally on port 8765 (matches `.claude/launch.json`):

```bash
python3 -m http.server 8765
```

Open `http://localhost:8765/` for the home page and `http://localhost:8765/ui-ux.html` for the UI/UX case page. Static-asset edits are reflected on reload — no watcher needed.

The site is deployed at [anfisa.link](https://anfisa.link) via GitHub Pages (`main` branch, root). The `CNAME` file in the repo root pins the custom domain — do not delete it.

## Architecture

Two HTML pages share the same portfolio:

- `index.html` — single-screen hero (name "Anfisa Utkina", lime badges, category pills). Top-level navigation pills point to `#branding`, `#books`, `#social`, `#motion` anchors that **do not yet have target pages or sections** — only `UI/UX` is wired up (it links to `ui-ux.html`). Treat the missing pages as the natural extension points when adding new case pages.
- `ui-ux.html` — vertical case-study page with two cases (Joki Joya, Yappy). Layout is built from `.meta` (task/results two-column blocks), `.grid grid-3` / `.grid grid-2` image grids, `.block` text sections, and a centered `.quote` block. Reuse these primitives when adding new cases rather than inventing new class names.

Both pages embed all CSS inline in a `<style>` block. **`styles.css` in the repo root is not linked from any HTML file** — it is a stale/reference copy of an earlier design system. Editing it will not change the rendered site; either delete it or fold its rules into the inline blocks if you intend to migrate to an external stylesheet.

## Design tokens

The two pages share a palette but redefine it locally in each `<style>` block. When changing a colour, update **both** `index.html` and `ui-ux.html` (and `styles.css` if you decide to keep it):

- `--blue-bg #1317D0` — homepage background
- `--navy #05082A` — dark text on light surfaces
- `--lime #C9F63F` — homepage badges
- `--sky #5FC0E8` — Telegram CTA pills (used on both pages)
- Font: `Inter` from Google Fonts (weights 400–900), with `Helvetica Neue`/Arial fallback

## Asset layout

- `assets/` — production images served by the site (`hero-photo.png`, `blob-blue*.png`, plus `assets/uiux/` for case screenshots: `jj-*`, `yappy-*`, `case-*`, `shot-*`).
- Top-level directories with **Cyrillic names containing spaces** are design references (Figma exports / Telegram screenshots), not site assets:
  - `главный экран/`, `как должно выглядеть/`, `страница books/`, `страница UI:UX/`, `страница some projects/`, `cтраница branding/`
  - **Gotcha:** `cтраница branding/` starts with a Latin `c`, not Cyrillic `с` — they look identical. Always copy the path from `ls` output rather than retyping, and quote it (`"cтраница branding"`).
  - **Gotcha:** `страница UI:UX/` contains a `:` — quote the path on macOS shells; tab-completion handles it correctly.
- These reference folders are read-only design comps. When implementing a new section, consult them for the intended look but do not copy PNGs into `assets/` unless they are actually used.

## Working conventions

- The site language is Russian (`<html lang="ru">`); UI copy is in Russian. Keep new copy in Russian unless the user explicitly asks for English.
- Image filenames in `assets/uiux/` follow patterns: `jj-mob-*` (Joki Joya mobile), `jj-desk-*` (desktop), `jj-promo-*` (campaign landings), `yappy-*` (Yappy app), `case-*` / `shot-*` (other cases not yet referenced from HTML). Reuse these prefixes when adding screenshots.
- The repo is published to GitHub Pages from the `main` branch root. A push to `main` triggers a redeploy automatically — no separate build step.
