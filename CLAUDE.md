# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A personal academic portfolio site for Yoonjin Chung built on the [al-folio](https://github.com/alshedivat/al-folio) Jekyll theme. Deployed to `https://yoonjinxd.github.io` via GitHub Pages (auto-deploys on push to `main`).

## Local development

**Recommended — Docker** (serves at `http://localhost:8080`):
```bash
docker compose pull && docker compose up
# slim variant (~100MB image):
docker compose -f docker-compose-slim.yml up
```

**Legacy — Ruby/Bundler** (serves at `http://localhost:4000`):
```bash
bundle install
pip install jupyter   # only needed if using Jupyter notebook posts
bundle exec jekyll serve
```

**Production build:**
```bash
bundle exec jekyll build                 # outputs to _site/
purgecss -c purgecss.config.js          # optional: strip unused CSS from _site/
```

Changes to `_config.yml` require a full server restart to take effect; all other file changes hot-reload.

## Architecture

### Content (edit these to customize the site)

| Path | Purpose |
|------|---------|
| `_config.yml` | Site-wide settings: title, URL, feature flags, plugin config |
| `_pages/about.md` | Homepage — biography, profile photo, section toggles |
| `_bibliography/papers.bib` | Publications in BibTeX; rendered by jekyll-scholar |
| `_data/cv.yml` | CV fallback (used when `assets/json/resume.json` is absent) |
| `assets/json/resume.json` | Primary CV source (JSON Resume standard) |
| `_data/coauthors.yml` | Co-author → URL mapping for auto-linking in publications |
| `_data/socials.yml` | Social media / contact links |
| `_data/repositories.yml` | GitHub users/repos to display on the repositories page |
| `_news/` | Homepage news items (inline or link style) |
| `_posts/` | Blog posts — filename must be `YYYY-MM-DD-title.md` |
| `_projects/` | Project cards shown on the projects page |
| `_books/` | Book reviews shown on the bookshelf page |

### Layouts & templates

- `_layouts/` — page-level Liquid templates (`about`, `post`, `bib`, `cv`, `distill`, etc.)
- `_includes/` — reusable partials (header, footer, news, citations, social icons, etc.)
- `_sass/` — SCSS styles; key files:
  - `_themes.scss` — theme color (`--global-theme-color`) and dark/light variables
  - `_variables.scss` — named color palette
  - `_base.scss` — fonts, spacing, base element styles

### Publications system

jekyll-scholar reads `_bibliography/papers.bib` and renders the publications page. Author name matching (for underlining your own name) is set in `_config.yml`:
```yaml
scholar:
  last_name: [Chung]
  first_name: [Yoonjin]
```
Custom BibTeX fields (`pdf`, `code`, `arxiv`, `abstract`, `bibtex_show`, etc.) control which buttons appear on each entry — see `_layouts/bib.liquid` for the full list.

### Removing content

Prefer adding unwanted files to the `exclude:` list in `_config.yml` over deleting them — this avoids merge conflicts when upgrading:
```yaml
exclude:
  - _news/announcement_*.md
  - _pages/blog.md
```
