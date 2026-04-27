# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

**Quinta Escuela Teoría Geométrica de Grupos 2026** — static website for a mathematics summer school at IM UNAM Unidad Oaxaca, July 27–31, 2026.

## Stack

Pure static site: HTML5 + CSS3 + vanilla JS, all in `index.html`. No build tools, no package manager.

External dependencies via CDN:
- Bootstrap 5.3.2 (layout, components, modals, tab-pills)
- Bootstrap Icons 1.11.3
- Google Fonts — Archivo

## Development

Open `index.html` directly in a browser, or serve locally (needed for any `fetch()` calls):

```bash
python -m http.server 8000
# or
npx serve .
```

## Architecture

Everything lives in `index.html` (~920 lines) and `css/esver.css`. Images in `img/`.

**Page sections** (by `id`): `#infographic` → `#intro` → `#registro` → `#cursos-platicas` → `#program` → `#contacto` → `#footer`.

**Program section (`#program`)** — Bootstrap nav-pills with tab-panes, one per weekday (`#lunes` … `#viernes`). On `DOMContentLoaded`, JS maps `new Date().getDay()` with `(today + 6) % 7` to auto-activate the tab matching the current weekday.

**Abstract modal** — a single shared `#abstractModal` populated dynamically. Clicking any `.program-item[data-talk-id]` element reads its `data-talk-id`, looks up the entry in the embedded `talkData` JS object, and injects title/presenter/abstract into the modal before showing it. There are 6 courses (`c1`–`c6`) and 3 talks (`p1`–`p3`).

**`abstracts.md`** — source text for abstracts; the current JS does **not** fetch it. Abstract content is duplicated in the `talkData` object inside `index.html`. Keep both in sync when editing abstracts.

**CSS custom properties** (in `css/esver.css`):
- `--main-ev-color: #EF7D00` — brand orange, used for highlights and titles
- `--main-bg-color: #1D1D1B` — dark background
- `--gray-color: #9F9F9F`

**`escuela-matematicas/`** — subdirectory containing a previous version of the site with its own `.git`; not part of this project.

## Known issues

- `<title>` tag reads "Escuela de Verano en Matemáticas 2025" — should be updated to match the 2026 event name.
- `abstracts.md` and the `talkData` JS object are out of sync if either is edited independently.
