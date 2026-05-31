# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static single-page artist website for Val Monique (valmonique.com), a Panamanian singer-songwriter. No build tools, no frameworks, no package manager — everything lives in one `index.html` file.

## Local development

Serve the root directory with any static file server:

```bash
python3 -m http.server 8080
# or
npx serve .
```

Open `http://localhost:8080`. No build step needed.

## Deployment

The site deploys to GitHub Pages at `valmonique.com` (CNAME file). Pushing to `main` triggers Pages rebuild automatically.

## Architecture

Everything is in `index.html`:

- **`<style>`** — all CSS, inline. No external stylesheet.
- **`<body>`** — eight named sections in order: `#hero`, `#galeria`, `#bio`, `#musica`, `#proceso`, `#entrevistas`, `#presentaciones`, `#proyectos`, then `<footer>`.
- **`<script>`** — all JavaScript, inline at the bottom.

Assets in `assets/images/` (photos, logo, cover art) and `assets/audio/` (quemame.mp3).

## CSS patterns

Brand palette is defined as CSS custom properties on `:root` (terracotta, deep-pink, dusty-rose, olive, teal, lavender, cherry, magenta, royal-blue). Two key gradients:

- `--grad-universe` — used on section headings (`.section-heading`)
- `--grad-quemame` — used on primary buttons (`.btn-primary`)

Scroll-reveal animation: elements start with class `reveal`, `reveal-l`, or `reveal-r` (opacity 0, offset). IntersectionObserver adds class `.in` to trigger the transition. Stagger delays use `.d1`–`.d5`.

## JavaScript patterns

Three IntersectionObservers:
1. **Scroll reveal** — watches all `.reveal*` elements, adds `.in` on entry.
2. **Nav background** — watches `#hero`; adds `.solid` to `#nav` when hero exits viewport.
3. **Active nav link** — one observer per section (`#musica`, `#galeria`, `#bio`, etc.), toggles `.active` on the matching `[data-s]` nav link using `rootMargin: '-28% 0px -65% 0px'`.

**YouTube embeds** — cards with class `.yt-player` and `data-yt="VIDEO_ID"` replace their inner HTML with an `<iframe>` on click (lazy load).

**Instagram Reels** — cards with class `.ig-card` call `openIg(embedUrl)` on click, which sets the `src` of `#ig-iframe` inside `#ig-modal` and adds class `.open`. `closeIg()` clears the src and removes `.open`. Escape key also closes.

**Background audio** — `#bg-music` plays `quemame.mp3` at volume 0.07, triggered on first user gesture (click/scroll/touch/keydown), then removes its own listeners.
