# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository overview

Static marketing site for **Liquicapital** (www.liquicapital.com), deployed via GitHub Pages from the `main` branch of `LiquiCapital/liquicapital-landing` (the repo root contains a `CNAME` file pointing to `www.liquicapital.com`). There is **no build step, no package manager, no test suite** — every page is hand-written HTML that links directly to the shared assets in `assets/`.

The site is based on the BootstrapMade "Bootslander" template (see `Readme.txt`); the original template's CSS/JS conventions still drive the markup.

## Local development

There is nothing to install or compile. Serve the directory with any static server, e.g.:

```sh
python3 -m http.server 8000
# then open http://localhost:8000/
```

Open individual landings directly by path (e.g. `http://localhost:8000/prontopago/darovi/`). Because all asset paths in the partner landings are written relative to repo root (`assets/...`, `../../assets/...`), serving any single subfolder in isolation will break styles — always serve from the repo root.

## Architecture

### Page layout

The repo is a flat collection of independent landing pages, not a single SPA. Each landing is a self-contained `index.html` that references the shared `assets/` tree. Top-level structure:

- `index.html` — main Liquicapital landing.
- `inner-page.html`, `aviso-privacidad.html`, `selectTest.html`, `calculadora*.html` — auxiliary single-page assets (privacy notice, financing calculators, etc.).
- `prontopago/`, `pagadespues/`, `cubocapital/`, `masseguros/` — **product lines**. Each contains its own `index.html` plus per-partner subfolders (`prontopago/darovi/`, `prontopago/grizzly/`, `prontopago/grupoalpha/`, `pagadespues/bajaj/`, etc.). New partner landings follow this `productline/<partner>/index.html` convention.
- `a/`, `c/`, `f/`, `gr/` — short-URL redirects/aliases (single `index.html` each); historical equivalents like `cubocapital/{a,b,c,d,e,f,g}/` exist for the same reason. When you change a canonical landing, check whether one of these short-URL stubs needs to track the change.

### Shared assets (`assets/`)

- `assets/vendor/` — vendored third-party libraries used by every page: Bootstrap, AOS, GLightbox, Swiper, PureCounter, Remixicon, Boxicons, Bootstrap Icons, php-email-form. **Do not edit vendor files**; replace the whole folder if upgrading.
- `assets/css/style.css` — site-wide styles (single file from the template, hand-edited). There is `assets/scss/` but no SCSS pipeline is wired up — `style.css` is edited directly. If you touch SCSS, regenerate or remove it; do not assume a watcher is running.
- `assets/js/main.js` — site-wide behavior (mobile nav toggle, AOS init, scroll effects, Swiper init, etc.) sourced from the Bootslander template.
- `assets/img/` and `assets/fonts/` — shared media.

When creating a new landing, the established pattern is to copy an existing partner `index.html` (e.g. `prontopago/darovi/index.html`) and adjust copy/branding rather than starting from `index.html` at the root, since the partner pages already use the correct relative asset paths (`../../assets/...`).

### Forms

Lead-capture forms (e.g. in `index.html`) are currently `<form action="" method="post">` placeholders — they do not submit anywhere. If you need real submissions, wire them to an external endpoint (Formspree/Netlify Forms/etc.) explicitly; do not assume one already exists.

## Conventions

- Page language is Spanish (`lang="en"` is left over from the template — copy uses Spanish; only change `lang` if you have a reason).
- Commit messages in this repo use Spanish, conventional-commit-style prefixes scoped to `landing`, e.g. `feat(landing): nueva landing Pronto Pago para proveedores de Darovi`. Match this style for new commits adding partner landings.
- Each partner landing is treated as a standalone deliverable in its own commit — keep partner changes scoped per commit rather than bundling multiple partners together.
