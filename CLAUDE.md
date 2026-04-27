# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static personal portfolio site for Gina Hyunhee Kim (ginakim.org), a Certified Logotherapist and NVC Practitioner based in Milton, Ontario. Deployed via GitHub Pages with the `CNAME` file pointing to `ginakim.org`.

No build step, no package manager, no test suite — plain HTML, CSS, and vanilla JavaScript.

## Local development

Open in a browser directly, or spin up a local server to avoid any path issues:

```bash
python3 -m http.server 8080
# or
npx serve .
```

## File structure

- `index.html` — main single-page portfolio (Hero → About → Services → Philosophy → Contact → SNS)
- `ongo-course.html` — standalone landing page for "The Ongo Book 2.0" online course; **has its own embedded `<style>` block** (no external CSS dependency)
- `css/style.css` — all styles for `index.html`
- `images/` — photos used across both pages

## Bilingual system (EN/KO)

All user-facing text in `index.html` uses paired `data-en` / `data-ko` attributes:

```html
<span data-en="About" data-ko="소개">About</span>
```

The `setLang(lang)` function in the inline `<script>` at the bottom of `index.html` iterates every `[data-en]` element and swaps `innerHTML` to the selected language attribute. When editing any copy, **both `data-en` and `data-ko` must be updated**, as well as the fallback text content (which defaults to English on page load).

`ongo-course.html` is Korean-only with no language toggle.

## Contact form

Handled by Formspree (`action="https://formspree.io/f/xvzdvlrj"`) via AJAX fetch in the inline script. No back-end changes needed for contact form updates — only the Formspree endpoint in the `action` attribute matters.

## Design system

CSS custom properties are defined in `css/style.css` `:root`. Key tokens:

| Variable | Use |
|---|---|
| `--accent` / `--sage` | Primary green tones for interactive elements |
| `--dark` | Headings, footer background |
| `--warm-white` / `--cream` | Alternating section backgrounds |
| `--mid` | Body text |
| `--font-serif` | Cormorant Garamond — headings, quotes, large display text |
| `--font-sans` | Lato + Noto Sans KR — body, labels, navigation |

`ongo-course.html` has its own separate color palette defined inline (uses `--sage`, `--gold`, `--charcoal`, etc.) that does not share values with `style.css`.

## Responsive breakpoints

Defined in `css/style.css`:
- `≤ 1024px`: two-column grids collapse, hero stacks vertically
- `≤ 768px`: nav links hidden, single-column layout throughout
