# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page static personal site (zincplusplus.com). Three files matter: `index.html`, `main.css`,
`img/`. No build step, no dependencies, no tests, no package manager. Repo is
`zincplusplus/zincplusplus` on GitHub, served as GitHub Pages from `master`. There is no `CNAME`
file in the repo (deleted in `273ed49`), so the custom domain is set in the repo's Pages settings —
if the domain ever stops resolving, check there first.

## Running it

Open `index.html` in a browser, or `python3 -m http.server` in the repo root and hit
`localhost:8000`. Nothing to compile.

## Layout architecture

The whole page is one `.hero` container. Two distinct layouts, mobile-first:

- **Default (mobile):** normal document flow. Full-bleed images (`width: 100vw`), portrait pulled up
  with a negative `margin-top` and `z-index: -1` so text overlaps it.
- **≥1366px (laptop):** `.hero` becomes a 2-column CSS Grid, `100vh` tall, and `.ecsspert` becomes a
  nested grid. The portrait spans rows 1–3 in column 1; its *second* image
  (`img/zinc-big-other-side.jpg`) is a `.portrait:after` pseudo-element positioned at `left: 100%`
  so the face continues past the column edge behind the text.

There is no tablet breakpoint — the iPad media query at the bottom of `main.css` is commented out
deliberately (`8846576`). Below 1366px everything falls back to the mobile layout, which the author
notes still needs work for smaller laptops.

## Conventions

- BEM-ish class names: `.block__element`, plus state/brand modifiers like `.github`, `.youtube`.
- Social icons are inline SVG from simpleicons.org, each with a `<title>` and `aria-labelledby`.
  Brand hover colours live in a flat block near the bottom of the base styles; add a new icon by
  adding the `<a class="world__link <brand">` with inline SVG and one `.brand:hover` rule, then
  bump the `repeat(5, 1fr)` in `.world`'s `grid-template-columns`.
- CSS properties inside a rule are alphabetised in most blocks. Blocks are separated by several
  blank lines rather than comments.
- Only external asset is the Exo webfont from Google Fonts.
