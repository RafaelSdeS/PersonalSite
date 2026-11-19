# Rafael Silva de Souza — personal site

Portfolio of a Computer Engineering student at UFRGS (Brazil) working on hardware-aware machine learning, HPC and quantitative finance.

**Live:** [English](https://rafaelsdes.github.io/) · [Português](https://rafaelsdes.github.io/indexpt.html) · [Español](https://rafaelsdes.github.io/indexes.html) · [Français](https://rafaelsdes.github.io/indexfr.html) · [Résumé (PDF)](https://rafaelsdes.github.io/resume.pdf) · [Currículo (PDF)](https://rafaelsdes.github.io/resumept.pdf)

## What's on it

- **Current work:** undergraduate research on restricting CNN kernels for Winograd hardware, the Winograd FPGA accelerators I work on with Thomas Schmitz (co-author of two papers in submission), and an ML portfolio-construction study for the Brazilian stock market (B3).
- **Experience:** Projeto VORIS (laser-triangulation 3D measurement at LABMETRO, UFSC) and E3 UFSC (Shell Eco-marathon team).
- **Projects:** LUNAX Fire Calendar (NASA Space Apps 2026), Conecta+ (IBM SkillsBuild challenge, team lead), a vLLM parallelism benchmark, File Organizer, Breakout in C and an open-source contribution to PAM UFRGS.
- **Activities:** UFRGSMUN 2026, UN Office of Counter-Terrorism committee.
- **Stack & contact**, plus a one-page résumé in each language.

Every card follows the same shape: a figure, a headline with the result, what I did and what I learned.

## How it's built

Plain HTML and CSS, with a small inline vanilla-JS script. No framework, no build step, no dependencies.

- Four languages with the same structure: `index.html` (English, primary), `indexpt.html` (Portuguese), `indexes.html` (Spanish) and `indexfr.html` (French). The header pill switches EN ↔ PT; Spanish and French are linked only from the footer.
- Dark and light themes, following the system preference, with a manual toggle.
- Responsive bento grid, with breakpoints at 1080, 680 and 540 px.
- Motion (scroll reveal, hover light, theme wipe) only when the visitor hasn't asked for reduced motion.
- Open Graph / Twitter link previews and JSON-LD `Person` data.
- Printable one-page résumés (`resume.html`, `resumept.html`, `resumees.html`, `resumefr.html`) with matching PDFs.

```
index*.html                 the site (EN / PT / ES / FR)
style.css, media.css        components and tokens / responsive overrides
resume*.html, resume.css    one-page résumés; resume*.pdf are their printed versions
assets/images/              portrait, event photos, project figures, link-preview cards
assets/icons/               IcoMoon icon font
tools/og-card.html          template for the link-preview images
```

## Run locally

Open `index.html` in a browser, or serve the folder:

```sh
python3 -m http.server 8000   # then open http://localhost:8000
```

## Keeping it up to date

- **Résumé changed?** Regenerate its PDF and check that it is still one page:

  ```sh
  chromium-browser --headless --no-pdf-header-footer --print-to-pdf="$PWD/resume.pdf" "file://$PWD/resume.html"
  chromium-browser --headless --no-pdf-header-footer --print-to-pdf="$PWD/resumept.pdf" "file://$PWD/resumept.html"
  chromium-browser --headless --no-pdf-header-footer --print-to-pdf="$PWD/resumees.pdf" "file://$PWD/resumees.html"
  chromium-browser --headless --no-pdf-header-footer --print-to-pdf="$PWD/resumefr.pdf" "file://$PWD/resumefr.html"
  for f in resume*.pdf; do pdfinfo "$f" | grep Pages; done
  ```

- **Hero or portrait changed?** Re-render the link-preview cards from `tools/og-card.html` (steps in `CLAUDE.md`).
- **Any content change** goes into all four `index*.html` pages, and into all four résumés when it's a fact they list.

GitHub Pages publishes `main` automatically.

## Contact

[rafael.souza@inf.ufrgs.br](mailto:rafael.souza@inf.ufrgs.br) · [LinkedIn](https://www.linkedin.com/in/rafael-silva-de-souza/) · [GitHub](https://github.com/RafaelSdeS)

Suggestions and critiques are welcome.
