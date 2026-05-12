# Copilot Instructions for jakewqj.github.io

## Overview

Personal Jekyll blog by Jake Wu, built on the [Chirpy theme](https://github.com/cotes2020/jekyll-theme-chirpy) (v6.0.1), deployed to GitHub Pages at `https://jakewqj.github.io`. Content covers music industry interviews, album reviews, Chinese indie music, live shows, and music gear.

## Build, Test, and Lint Commands

```bash
# Ruby dependencies (run once)
bundle install

# Node dependencies (run once)
npm install

# Local dev server at http://localhost:4000
bundle exec jekyll serve

# JS build (minified, production — run before committing JS changes)
npm run build

# JS watch mode (development, with source maps)
npm run watch

# SCSS linting
npm run test

# SCSS auto-fix
npm run fixlint

# HTML validation (run after jekyll build)
bundle exec htmlproofer ./_site --disable-external --check-html --allow_hash_href
```

## Architecture

**Dual build pipeline:**
- **Ruby/Jekyll** — builds the static site from Markdown posts, Liquid templates, SCSS, and YAML data
- **Node/Rollup** — bundles ES6 JavaScript modules (`_javascript/`) into minified IIFE bundles (`assets/js/dist/`)

The two pipelines are independent. Jekyll serves the already-built JS from `assets/js/dist/`; it does not invoke Rollup.

**Key directories:**
- `_posts/` — blog posts in Markdown with YAML front matter
- `_tabs/` — static pages (about, archives, categories, tags)
- `_data/` — YAML data files (authors, locales, contact, share)
- `_layouts/` — Liquid page layouts (post, home, page, archives, categories, tag)
- `_includes/` — reusable Liquid template fragments
- `_sass/` — SCSS source; entry point is `assets/css/style.scss` which imports the theme and custom partials
- `_javascript/` — ES6 module source; Rollup builds entry points (`commons.js`, `home.js`, `post.js`, `page.js`, `categories.js`, `misc.js`) into `assets/js/dist/*.min.js`
- `_plugins/` — Ruby plugins (e.g., `posts-lastmod-hook.rb` auto-tracks modification dates)
- `assets/lib/` — git submodule (chirpy-static-assets); **do not edit manually**
- `.github/workflows/pages-deploy.yml` — CI/CD: builds Jekyll site, runs html-proofer, deploys to GitHub Pages

**SCSS architecture:**
- `_sass/addon/variables.scss` — core theme variables (override in `_sass/variables-hook.scss`)
- `_sass/addon/commons.scss` — shared utility styles
- `_sass/colors/` — light/dark theme color tokens (typography + syntax highlighting)
- `_sass/layout/` — per-layout styles
- `_sass/jekyll-theme-chirpy.scss` — theme entry point (imports everything above)
- Custom SCSS partials (like `_spotify-embed.scss`) are imported in `assets/css/style.scss`

## Key Conventions

### Posts
- Filename format: `YYYY-MM-DD-title-slug.md`
- Timezone: `Asia/Shanghai` (`+0800`)
- Required front matter: `title`, `author: jake`, `date` (with timezone offset), `categories` (1–2 levels), `tags` (lowercase, hyphenated)
- Author key `jake` maps to `_data/authors.yml`
- `last_modified_at` is auto-tracked by the plugin — do not set it manually
- Code highlighting uses Rouge; math (MathJax) and Mermaid diagrams are supported

### JavaScript
- **Never edit `assets/js/dist/*.min.js` directly.** Edit source in `_javascript/`, then run `npm run build` and commit the rebuilt dist files.
- Source modules live in `_javascript/modules/`, organized as `components/` and `layouts/`
- Each module is a standalone ES6 file; file naming uses `kebab-case.js`
- Bundled output format is IIFE, namespaced under `Chirpy`

### Styling
- SCSS changes go in `_sass/` partials or `variables-hook.scss`, never inline in HTML/Liquid templates
- `_sass/variables-hook.scss` is the designated spot for overriding theme variables
- Custom SCSS partials are imported in `assets/css/style.scss` after the theme import
- CSS is compressed in production via `sass.style: compressed` in `_config.yml`

### Configuration
- `_config.yml` — primary Jekyll config. Comments are disabled, PWA is enabled, pagination is 10 posts per page
- `comments.active` is empty (disabled); set to `disqus`, `utterances`, or `giscus` to enable
- `img_cdn` is set to `https://raw.githubusercontent.com` — all image paths starting with `/` get this prefix

### General
- 2-space indentation, Unix line endings, UTF-8 (enforced by `.editorconfig`)
- Prettier with `trailingComma: "none"`; Stylelint Standard SCSS with max 3 declarations per single-line rule
- HTML compression is active in production; avoid raw `<pre>` blocks outside Markdown code fences
- Locale strings for UI labels live in `_data/locales/` — edit there rather than hardcoding text in templates
- `assets/lib/` is a git submodule; do not edit files inside it
- Commit compiled JS (`assets/js/dist/`) alongside source changes
- The `_posts/.DS_Store`, `.obsidian/`, and `.vscode/` directories are gitignored
- Push to `main` or `master` triggers automatic deployment via GitHub Actions
