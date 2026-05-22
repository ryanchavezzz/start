# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A personal start-page / new-tab page hosted at `rchvz.dev`. The entire site is a single, self-contained `index.html` — no build step, no framework, no package manager. Open the file directly in a browser to develop locally.

## Architecture

Everything lives in `index.html`: all HTML, CSS (in a `<style>` block), and JavaScript (in a `<script>` block at the bottom). There is intentionally no bundler or toolchain.

**`chips.csv`** is a human-readable reference catalogue of all links (columns: `section, label, url, icon_type, icon_value`). It is *not* loaded at runtime — it exists to track what links are in the page and serves as a quick reference when adding/removing entries. Keep it in sync with `index.html` when editing links.

**`icons/`** holds locally-hosted SVG icons. **`fonts/`** holds local Proxima Nova OTF font files. Both are referenced directly from `index.html`.

## Key patterns

### Panels and chips
Links are grouped into `<div class="panel">` blocks, each with a `.panel-heading` and a `.chips` flex container. Every link is an `<a class="btn">` element.

### Icon types — three variants exist, choose the right one:
- **`.si`** — monochrome silhouette (CSS `mask-image`), uses SimpleIcons CDN (`https://cdn.simpleicons.org/<slug>`) or a local SVG. Inherits `--text` color automatically.
- **`.si-color`** — full-color icon loaded as `background-image`; always a local file in `icons/`.
- **Inline `<svg class="svg-icon">`** — used when a custom or one-off SVG is needed directly in markup.
- **`<img class="fav">`** — a Google Favicons API image (`https://www.google.com/s2/favicons?domain=…&sz=64`); starts grayscale and gains color on hover.

### Theming
CSS custom properties on `:root` (light) and `[data-theme="dark"]` drive all colors. The `data-theme` attribute is toggled on `<html>` by `toggleTheme()`. The theme switch is a fixed-position sun/moon toggle on desktop (≥600 px), inline on mobile.

### JavaScript modules (all inline, IIFE-based)
- **Clock** — `updateClock()` called immediately and every 1 s via `setInterval`.
- **Holiday badge** — runs once on load; shows a pill badge next to the clock when a holiday is ≤7 days away (accent color if today).
- **Search** — `/` key opens a spotlight-style modal (`#srchOverlay`). On open, it scrapes all `.btn` elements from the DOM to build `allLinks[]`. Arrow keys navigate, Enter opens in new tab, Escape closes.

## Editing links

1. Add/remove the `<a class="btn">` element inside the appropriate `.chips` div in `index.html`.
2. Update `chips.csv` to match (keep both in sync).
3. If you need a new local icon, add the SVG to `icons/` and reference it from the new button.
