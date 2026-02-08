# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A Jekyll static site publishing the 18F Lean Product Design guide — a 9-step methodology for hypothesis-driven product development. Live at https://gsiener.github.io/lean-product-design/.

## Build & Develop

```bash
bundle install                                              # install deps
bundle exec jekyll serve --baseurl "/lean-product-design"   # local dev at localhost:4000/lean-product-design/
bundle exec jekyll build --baseurl "/lean-product-design"   # production build to _site/
```

Ruby 3.3+ required. The `baseurl` flag is needed locally to match the GitHub Pages path; in CI, `_config.yml` already sets `baseurl: /lean-product-design`.

## Deployment

Push to `18f-pages` branch triggers `.github/workflows/pages.yml` → Jekyll build → GitHub Pages deploy. No manual steps needed. Source must be set to "GitHub Actions" in repo Settings > Pages.

## Architecture

- **Theme:** `just-the-docs` 0.12 — provides sidebar nav, search, and responsive layout from front matter alone
- **Navigation:** Controlled entirely by `nav_order` in each page's YAML front matter (1–11). No config-based nav.
- **Layout:** All pages use the `default` layout (set in `_config.yml` defaults)
- **Markdown engine:** kramdown (Jekyll 4 default) — requires a space after `#` in headings

## Content Structure

| nav_order | File | Permalink |
|-----------|------|-----------|
| 1 | `index.md` | `/` |
| 2 | `pages/lean-product-principles.md` | `/lean-product-principles/` |
| 3–11 | `pages/1-discovery-research.md` through `pages/9-plan-sprint-agile.md` | `/1-discovery-research/` etc. |

Images referenced as `{{site.baseurl}}/images/...` — the `images/` directory contains Trello board screenshots and process diagrams used in steps 7–9.

## Key Gotchas

- **kramdown vs redcarpet:** This site was migrated from redcarpet. Headings like `###Foo` (no space) won't render — kramdown requires `### Foo`.
- **Inline HTML tables:** Pages 5, 6, and 7 contain raw HTML `<table>` elements with inline `<style>` tags. kramdown passes these through as-is, but blank lines around HTML blocks are required.
- **baseurl:** Must be `/lean-product-design` for all asset/link paths to resolve on `gsiener.github.io`. The `actions/configure-pages` action does NOT inject this automatically — it's set explicitly in `_config.yml`.

## License

CC0 1.0 — public domain.
