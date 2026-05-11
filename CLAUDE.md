# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Hexo static blog using the Butterfly theme. Content is written in Markdown with YAML frontmatter in `source/_posts/`. The site is generated to static HTML in `public/`.

## Build & Development Commands

| Command | Description |
|---|---|
| `pnpm run server` | Start dev server with hot reload (default http://localhost:4000) |
| `pnpm run build` | Generate static site to `public/` |
| `pnpm run clean` | Delete `public/` and clear Hexo cache (`db.json`) |
| `pnpm run deploy` | Deploy (requires configured deploy target in `_config.yml`) |

Create a new post: `npx hexo new "Post Title"`
Create a new draft: `npx hexo new draft "Draft Title"`

## Architecture

### Content (source/)
- `source/_posts/` — all blog posts as `.md` files. Frontmatter fields: `title`, `date`, `tags`. The `scaffolds/` directory holds templates used by `hexo new`.
- `source/_drafts/` — unpublished drafts (create this directory if needed; Hexo does not render drafts by default unless `--draft` flag is used).

### Configuration
- `_config.yml` — main Hexo configuration (site metadata, URL structure, permalink format, syntax highlighting, pagination, theme selection, deploy settings).
- `themes/butterfly/_config.yml` — theme-specific configuration (navigation, code blocks, social links, comments, widgets, analytics, etc.). This file is large and controls most visual/functional theme behavior.

### Theme (themes/butterfly/)
The Butterfly theme (`hexo-theme-butterfly` v5.5.5-b1) is a git submodule pinned to the `dev` branch.

- **Layouts** (`layout/`) — Pug templates. Key files:
  - `layout/index.pug` — homepage
  - `layout/post.pug` — single post page
  - `layout/archive.pug`, `layout/category.pug`, `layout/tag.pug`, `layout/page.pug`
  - `layout/includes/` — partials organized by purpose: `head/`, `header/`, `page/`, `post/`, `widget/`, `third-party/`, `mixins/`
- **Styles** (`source/css/`) — Stylus (`.styl`) files organized into `_layout/`, `_tags/`, `_page/`, `_highlight/`, `_mode/`, `_search/`
- **Scripts** (`scripts/`) — theme JavaScript helpers: custom Hexo tag plugins (`scripts/tag/`), helper functions (`scripts/helpers/`), event hooks (`scripts/events/`), and filters (`scripts/filters/`). These run at build time, not in the browser.
- **Client-side JS** (`source/js/`) — browser JavaScript: `main.js`, `utils.js`, `tw_cn.js`, plus search implementations.
- **Languages** (`languages/`) — i18n YAML files (zh-CN, zh-TW, zh-HK, en, ja, ko).

### Rendering Pipeline
Hexo renderers in use: `hexo-renderer-marked` (Markdown → HTML), `hexo-renderer-pug` (Pug templates), `hexo-renderer-stylus` (Stylus stylesheets), `hexo-renderer-ejs` (EJS templates). Hexo generators (`hexo-generator-index`, `hexo-generator-archive`, `hexo-generator-category`, `hexo-generator-tag`) produce the respective listing pages.

### Cache
`db.json` is Hexo's internal database/cache (gitignored). Run `pnpm run clean` to wipe it along with the generated output.

## Package Manager

This project uses **pnpm** (see `pnpm-lock.yaml` and `pnpm-workspace.yaml`). Always use `pnpm` commands rather than npm or yarn.
