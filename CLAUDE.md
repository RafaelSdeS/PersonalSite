# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A static, no-build personal portfolio site (in Portuguese, "SitePessoal"). Plain HTML/CSS, no JavaScript, no package manager, no build step. Hosted via GitHub Pages (see CNAME history in git log).

## Running locally

There is no build/test/lint tooling. Open the HTML files directly in a browser, or use the VS Code "Live Server" extension (`.vscode/settings.json` sets its port to 5503) for auto-reload while editing.

## Architecture

- `index.html` — **English homepage, the default/primary version.**
- `indexpt.html` — Portuguese translation of the same page. **Both files share identical structure, section IDs, and classes** — they are kept in sync manually. Any structural or styling change to one almost always needs the same change in the other; only the text content and the `.lang` language-toggle link differ (`index.html` links to `indexpt.html` and vice versa).
- `style.css` — base styles, reset, and design tokens. Colors/fonts/sizes are defined once as CSS custom properties in `:root` (`--bg`, `--surface`, `--text`, `--accent`, `--title-fontsize`, `--font-sans`, `--font-mono`, etc.) — change a token here rather than hardcoding values in section rules. Dark theme is the default; `@media (prefers-color-scheme: light)` overrides the same tokens for light mode — no separate stylesheet or JS toggle.
- `media.css` — all responsive/breakpoint overrides, loaded after `style.css`. Breakpoints in use: `min-width: 1400px`, and `max-width: 1080px / 900px / 760px / 460px`. Keep breakpoint-specific rules here, not in `style.css`.
- `assets/images/` — legacy photos (`minha-foto.jpg`, `premio.jpg`, `programando.jpg`, `github.jpg`) no longer referenced by either HTML page since the redesign moved to CSS-only decoration; left in place but unused.
- `assets/icons/` — a custom icon font (generated via IcoMoon, per `assets/icons/Read Me.txt`) with its own `style.css`, `fonts/`, and `selection.json` (the IcoMoon project file — reimport it into icomoon.io if icons need to be added/changed, don't hand-edit the font). `icon-linkedin` in `assets/icons/style.css` is a placeholder codepoint (`\e903`) with no matching glyph yet — regenerate the font via icomoon.io before it will render.

Both HTML pages follow the same section layout, identified by `id`: `#hero` (intro/tagline/CTA/"Open to" pills) → `#about` (background + stat card) → `#work` ("Current Work": Research + Quant Finance cards) → `#experience` (prior research/engineering: Projeto VORIS + E3 UFSC cards) → `#skills` (tech-stack and language pills) → `<footer>` (contact links: email, WhatsApp, GitHub, LinkedIn). A fixed `.site-header` holds the anchor nav and the language toggle. `#work` and `#experience` share the `.card-grid`/`.card` component (defined once in `style.css`) — reuse it rather than adding a new grid variant if another card section is added later.
