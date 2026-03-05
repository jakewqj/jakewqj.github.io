# CLAUDE.md — AI Assistant Guide for jakewqj.github.io

## Overview

This is a **personal Jekyll blog** by Jake Wu, built on the [Chirpy theme](https://github.com/cotes2020/jekyll-theme-chirpy) (v6.0.1) and deployed to **GitHub Pages** at `https://jakewqj.github.io`. The content focuses on music industry interviews, reviews, and personal writing.

---

## Repository Structure

```
.
├── _config.yml            # Main Jekyll configuration
├── _data/                 # YAML data: locales, authors, contact, share
├── _includes/             # Reusable Liquid template fragments
├── _javascript/           # JavaScript source modules (pre-bundled)
│   └── modules/           # Component and layout sub-modules
├── _layouts/              # Page layout templates (Liquid)
├── _plugins/              # Ruby plugins (posts-lastmod-hook.rb)
├── _posts/                # Blog post Markdown files
├── _sass/                 # SCSS source stylesheets
│   ├── addon/             # Utility variables, commons, syntax
│   ├── colors/            # Light/dark theme typography & syntax
│   └── layout/            # Per-layout styles
├── _tabs/                 # Static pages (about, archives, categories, tags)
├── assets/
│   ├── css/style.scss     # CSS entry point (imports _sass)
│   ├── img/favicons/      # Favicon assets
│   └── js/
│       ├── dist/          # Bundled & minified JS (committed)
│       ├── pwa/           # Service worker files
│       └── lib/           # Git submodule: chirpy-static-assets
├── tools/                 # Dev scripts: init, release, run, test
├── .github/workflows/     # GitHub Actions CI/CD
├── package.json           # Node dependencies (Rollup, Babel, Terser, Stylelint)
├── Gemfile                # Ruby dependencies (Jekyll, html-proofer)
├── rollup.config.js       # JS bundler configuration
└── _config.yml            # Jekyll site configuration
```

---

## Technology Stack

| Concern | Technology |
|---|---|
| Static site generator | Jekyll 4.3+ |
| Theme | Chirpy v6.0.1 |
| Templating | Liquid |
| CSS | SCSS → Bootstrap 5 base |
| JavaScript | Vanilla ES6+ modules |
| JS bundler | Rollup + Babel + Terser |
| Ruby package manager | Bundler |
| Node package manager | npm |
| Linting | Stylelint (SCSS), Prettier, commitlint |
| Git hooks | Husky |
| Testing | html-proofer |
| Deployment | GitHub Pages via GitHub Actions |
| PWA | Enabled (service worker + cache manifest) |
| Analytics | Google Analytics (G-2DTX8CBY2C) |

---

## Development Workflows

### Prerequisites

Install both Ruby and Node dependencies:

```bash
bundle install    # Ruby/Jekyll dependencies
npm install       # Node/build tool dependencies
```

### Local Development

```bash
bundle exec jekyll serve    # Serve site at http://localhost:4000
npm run watch               # Watch and rebuild JS in development mode
```

### Building JavaScript

Source JS lives in `_javascript/`. **Do not edit files in `assets/js/dist/` directly** — they are generated.

```bash
npm run build       # Production build (minified, no source maps)
npm run watch       # Development watch mode (with source maps)
npm run test        # Run Stylelint on SCSS files
npm run fixlint     # Auto-fix linting issues
```

Rollup bundles each entry point (`commons.js`, `home.js`, `post.js`, `page.js`, `categories.js`, `misc.js`) into a corresponding `.min.js` in `assets/js/dist/`.

### CI/CD (GitHub Actions)

`.github/workflows/pages-deploy.yml` handles automated deployment:
1. Triggered on push to `master`/`main` (excluding docs-only changes)
2. Builds the Jekyll site: `bundle exec jekyll b`
3. Runs html-proofer (internal links only)
4. Deploys to GitHub Pages

---

## Writing Blog Posts

Posts go in `_posts/` with the filename format: `YYYY-MM-DD-title-slug.md`

### Front Matter Template

```yaml
---
title: "Post Title"
author: jakewqj              # matches _data/authors.yml key
date: 2024-01-15 10:00:00 +0800
categories: [Category]       # 1–2 levels, e.g. [Music Industry]
tags: [tag1, tag2]           # lowercase, hyphenated
pin: false                   # optional: pin to top of home page
render_with_liquid: false    # optional: disable Liquid in post content
---
```

### Content Guidelines

- Write in Markdown
- Timezone is `Asia/Shanghai` (`+0800`)
- Code blocks use Rouge syntax highlighting
- Images: place in `assets/img/` or use CDN URLs
- Math, Mermaid diagrams, and `{% raw %}` Liquid escaping are supported
- The `last_modified_at` date is auto-tracked by `_plugins/posts-lastmod-hook.rb`

---

## Styling

### SCSS Architecture

- `_sass/addon/variables.scss` — core theme variables (override via `_sass/variables-hook.scss`)
- `_sass/colors/` — light/dark theme color tokens
- `_sass/layout/` — per-layout styles (home, post, tags, archives, categories)
- `assets/css/style.scss` — single entry point that imports everything

### Customization Entry Points

- **SCSS variables**: override in `_sass/variables-hook.scss`
- **Custom styles**: append to `assets/css/style.scss` or add new partials
- **Dark/light themes**: edit `_sass/colors/light-typography.scss` or `dark-typography.scss`

### Code Style Rules

- 2-space indentation (EditorConfig)
- Unix line endings, UTF-8 encoding
- Stylelint Standard SCSS config; max 3 declarations per single-line rule
- No trailing commas (Prettier config)

---

## JavaScript Conventions

- Source in `_javascript/modules/` — organized as **components** and **layouts**
- Each component is a standalone ES6 module with a clear single responsibility
- File naming: `kebab-case.js`
- Do not import npm packages directly in components — add them to `modules/plugins.js`
- After editing JS source, run `npm run build` and commit the updated `assets/js/dist/*.min.js` files

---

## Configuration Reference (_config.yml)

Key settings to know:

| Key | Value | Notes |
|---|---|---|
| `url` | `https://jakewqj.github.io` | Production URL |
| `timezone` | `Asia/Shanghai` | Used for post dates |
| `theme` | `jekyll-theme-chirpy` | Do not change |
| `paginate` | `10` | Posts per page |
| `comments.active` | `""` (disabled) | Set to `disqus`, `utterances`, or `giscus` to enable |
| `pwa.enabled` | `true` | Service worker active |
| `analytics.google.id` | `G-2DTX8CBY2C` | GA4 tracking ID |

---

## Key Conventions for AI Assistants

1. **Never edit `assets/js/dist/*.min.js` directly.** Always edit source in `_javascript/` and rebuild.
2. **Front matter is required** for all posts. Use the template above.
3. **Post filenames must follow** `YYYY-MM-DD-slug.md` format.
4. **SCSS changes** belong in `_sass/` or `variables-hook.scss`, not inline in templates.
5. **Do not modify `_layouts/` or `_includes/`** unless the change is clearly theme-level; prefer post/page front matter options instead.
6. **Commit the compiled JS** (`assets/js/dist/`) alongside source changes.
7. **Run `npm run test`** to check SCSS linting before committing style changes.
8. **The `assets/lib/` directory** is a git submodule — do not manually edit files inside it.
9. **HTML compression** is active in production; avoid inserting raw `<pre>` blocks outside Markdown code fences.
10. **Locale strings** for UI labels live in `_data/locales/` — edit there rather than hardcoding text in templates.

---

## Testing

```bash
# SCSS linting
npm run test

# Full HTML validation (run after a Jekyll build)
bundle exec htmlproofer ./_site --disable-external
```

GitHub Actions runs both automatically on each push to master.

---

## Deployment

Push to `master` — GitHub Actions handles the rest. The workflow:
1. Builds with `bundle exec jekyll b`
2. Validates HTML
3. Deploys artifact to GitHub Pages

No manual deployment steps are needed.
