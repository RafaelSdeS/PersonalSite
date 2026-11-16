# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static, no-build personal portfolio site (in Portuguese, "SitePessoal"). Plain HTML/CSS with a small inline vanilla-JS theme toggle (no package manager, no build step). Hosted via GitHub Pages (see CNAME history in git log).

## Running locally

There is no build/test/lint tooling. Open the HTML files directly in a browser, or use the VS Code "Live Server" extension (`.vscode/settings.json` sets its port to 5503) for auto-reload while editing.

## Architecture

- `index.html` — **English homepage, the default/primary version.**
- `indexpt.html` — Portuguese translation of the same page. **Both files share identical structure, section IDs, and classes** — they are kept in sync manually. Any structural or styling change to one almost always needs the same change in the other; only the text content and the `.lang` language-toggle link differ (`index.html` links to `indexpt.html` and vice versa).
- `style.css` — base styles, reset, design tokens and every component. Colors/fonts are CSS custom properties in `:root` (`--bg`, `--tile`, `--border`, `--border-hi`, `--text`, `--muted`, `--faint`, `--accent`, `--on-accent`, `--header-bg`, `--sans` Space Grotesk, `--mono` JetBrains Mono) — change a token rather than hardcoding values. Dark theme is the default. Light-mode token values are duplicated in two places: a `@media (prefers-color-scheme: light)` block (guarded by `:root:not([data-theme="dark"])` so an explicit manual choice can override system preference) and a `:root[data-theme="light"]` block for the manual toggle. `.theme-toggle` (in the sticky `.top` header, both pages) calls the inline `toggleTheme()` defined in each page's `<head>`, which flips `data-theme` on `<html>` and persists it to `localStorage`; the same head script re-applies the stored choice before first paint. A second inline script at the end of `<body>` adds `.scrolled` to the header and highlights the nav link of the section in view. Keep both scripts identical across `index.html`/`indexpt.html`.
- `media.css` — all responsive overrides, loaded after `style.css`. Breakpoints: `max-width: 1240px` (background + team photo stack), `1080px` (tablet: tiles full width, project tiles pair up), `680px` (phone: one column), `460px` (nav links hidden, header actions stay). Keep breakpoint-specific rules here.
- `assets/images/` — `rafael-portrait.jpg` (hero), `ufrgsmun-team.jpg` (next to Background) and `projects/*.webp` (one 3:2 figure per tile: real screenshots, figures from the user's slides, or renders). Older photos (`minha-foto.jpg`, `premio.jpg`, `programando.jpg`, `github.jpg`) are unused. Two figures (Quant, File Organizer) are inline SVG diagrams in the HTML, styled by `.diagram` in `style.css` — schematic only, never invented data.
- `assets/icons/` — a custom icon font (generated via IcoMoon, per `assets/icons/Read Me.txt`) with its own `style.css`, `fonts/`, and `selection.json` (reimport into icomoon.io to change icons; don't hand-edit the font). Covers `icon-gmail`, `icon-github`. LinkedIn has no glyph, so the contact tile uses an inline SVG.
- `variants/` — untracked design prototypes (Split, Bento preview) kept locally; not part of the site.

- `resume.html` / `resumept.html` + `resume.css` — one-page printable résumé (EN/PT), linked from the `CV` pill in each page's `.header-actions`. Deliberately standalone (does not load `style.css`): white, single column, semantic `h2`/`h3`/`ul` for ATS/AI parsing, must print to exactly 1 page (Letter and A4). It restates the site's facts in condensed form — when a project/experience changes on the site, update both résumés too.

Both HTML pages use one 12-column "bento" grid (`main.bento`, tiles `.tile` sized with `.s4/.s6/.s8/.s12`), in this order: intro (`.intro`) + portrait → `#work` group ("Current work": Research + Quant tiles, then the `.focus` strip) → Background (`.about`) + team photo (`.photo`) → `#experience` (VORIS + E3) → `#projects` (six project tiles) → `#contact` (skills + contact tiles) → `footer.credit`. Work, experience and project tiles share one shape (`.tile.p`): `a.shot-link` figure (links to the live demo when there is one, else the title link) → `.label` → `.hl` headline → `.metric-label` facts line → `h3` title link → optional `.sublinks` → `.desc` → `details` ("How I contributed / What I learned") → `.tags`. Keep that shape on every card. Headlines state what the project shows; never replace them with bare "effect numbers". The sticky `.top` header holds the section nav, the theme toggle and the CV / language pills (`index.html` → `resume.html` + `indexpt.html`; `indexpt.html` → `resumept.html` + `index.html`).
