# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static, no-build personal portfolio site (in Portuguese, "SitePessoal"). Plain HTML/CSS with a small inline vanilla-JS theme toggle (no package manager, no build step). Hosted via GitHub Pages (see CNAME history in git log).

## Running locally

There is no build/test/lint tooling. Open the HTML files directly in a browser, or use the VS Code "Live Server" extension (`.vscode/settings.json` sets its port to 5503) for auto-reload while editing.

## Architecture

- `index.html` — **English homepage, the default/primary version.**
- `indexpt.html` — Portuguese translation of the same page. **Both files share identical structure, section IDs, and classes** — they are kept in sync manually. Any structural or styling change to one almost always needs the same change in the other; only the text content and the `.lang` language-toggle link differ (`index.html` links to `indexpt.html` and vice versa).
- `style.css` — base styles, reset, and design tokens. Colors/fonts/sizes are defined once as CSS custom properties in `:root` (`--bg`, `--surface`, `--text`, `--accent`, `--header-bg`, `--title-fontsize`, `--font-sans`, `--font-mono`, etc.) — change a token here rather than hardcoding values in section rules. Dark theme is the default. Light-mode token values are duplicated in two places: a `@media (prefers-color-scheme: light)` block (guarded by `:root:not([data-theme="dark"])` so an explicit manual choice can override system preference) and a `:root[data-theme="light"]` block for the manual toggle. `.theme-toggle` (in `.site-header`, both pages) calls the inline `toggleTheme()` function defined in each page's `<head>`, which flips `data-theme` on `<html>` and persists it to `localStorage`; a second inline head script re-applies the stored choice before first paint to avoid a flash of the wrong theme. Keep this toggle script identical across `index.html`/`indexpt.html`.
- `media.css` — all responsive/breakpoint overrides, loaded after `style.css`. Breakpoints in use: `min-width: 1400px`, and `max-width: 1080px / 900px / 760px / 460px`. Keep breakpoint-specific rules here, not in `style.css`.
- `assets/images/` — legacy photos (`minha-foto.jpg`, `premio.jpg`, `programando.jpg`, `github.jpg`) no longer referenced by either HTML page since the redesign moved to CSS-only decoration; left in place but unused.
- `assets/icons/` — a custom icon font (generated via IcoMoon, per `assets/icons/Read Me.txt`) with its own `style.css`, `fonts/`, and `selection.json` (the IcoMoon project file — reimport it into icomoon.io if icons need to be added/changed, don't hand-edit the font). Covers `icon-gmail`, `icon-github` (a leftover `icon-whatsapp` glyph exists in the font/`selection.json` but is unused now that WhatsApp was removed from the footer). LinkedIn has no glyph in the font, so the footer's LinkedIn icon is an inline SVG (`.icon-linkedin` in the markup, sized via `.contact svg` in `style.css`) instead of an `<i class="icon-...">` font glyph — add a real LinkedIn glyph to the IcoMoon set only if you want to switch it back.

- `resume.html` / `resumept.html` + `resume.css` — one-page printable résumé (EN/PT), linked from the `CV` pill in each page's `.header-actions`. Deliberately standalone (does not load `style.css`): white, single column, semantic `h2`/`h3`/`ul` for ATS/AI parsing, must print to exactly 1 page (Letter and A4). It restates the site's facts in condensed form — when a project/experience changes on the site, update both résumés too.

Both HTML pages follow the same section layout, identified by `id`: `#hero` (intro/tagline/CTA/"Open to" pills) → `#about` (background + stat card) → `#work` ("Current Work": Research + Quant Finance cards) → `#experience` (prior research/engineering: Projeto VORIS + E3 UFSC cards) → `#projects` (project cards) → `#skills` (tech-stack and language pills) → `<footer>` (contact links: email, GitHub, LinkedIn). Cards in `#work`, `#experience` and `#projects` share one shape: title link → optional `.project-links` row → description → `.project-meta` → two `.project-result` paragraphs ("How I contributed/contribute" / "What I learned/I'm learning") → pills; keep it on every card. A fixed `.site-header` holds the anchor nav and the language toggle. `#work` and `#experience` share the `.card-grid`/`.card` component (defined once in `style.css`) — reuse it rather than adding a new grid variant if another card section is added later.
