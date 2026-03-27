# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development commands

- `npm install` — install Hexo and renderer dependencies.
- `npm run server` — start the local Hexo server for previewing the site at `http://localhost:4000`.
- `npm run build` — generate the static site with `hexo generate`.
- `npm run clean` — clear generated/cache artifacts before rebuilding.
- `npm run deploy` — deploy via `hexo-deployer-git` using the target configured in `_config.yml`.
- `npx hexo new post "<title>"` — create a new post from `scaffolds/post.md`.
- `npx hexo new page "<name>"` — create a new page from `scaffolds/page.md`.

## Validation

- There is no lint or automated test setup in this repository.
- There is no single-test command.
- The main validation step is `npm run build` and then checking the site locally with `npm run server`.

## Architecture overview

- This is a Hexo static blog repo. Root `_config.yml` defines the site-level Hexo settings, permalink behavior, GitHub Pages deployment target, and selects the `gal-theme` theme.
- Author-written content lives under `source/`. Most posts are in `source/_posts/`; standalone pages such as `about`, `categories`, `tags`, `search`, and `404` live in their own subdirectories.
- `scaffolds/` contains the default front matter templates used by `hexo new`.
- `public/` is generated output, `db.json` is Hexo cache/state, and `.deploy_git/` is the deploy checkout. These are generated artifacts, not source of truth.
- The blog uses `post_asset_folder: true`, so posts can keep adjacent assets in per-post folders under `source/_posts/`.

## Theme structure

- The site rendering logic lives in `themes/gal-theme`, which README treats as a separately cloned theme repo.
- Important: root `.gitignore` ignores `themes/*` except `.gitkeep`, so edits inside `themes/gal-theme` are not tracked by this repo by default.
- `themes/gal-theme/_config.yml` controls theme-level behavior such as navigation, sidebar widgets, preview images, slideshow images, and optional integrations like comments, music player, and live2d.
- `themes/gal-theme/layout/layout.ejs` is the main page shell. It assembles the head, slideshow, header, main body, sidebar, footer, and optional widgets/scripts.
- `themes/gal-theme/layout/index.ejs` renders the homepage feed. It supports pinned posts via `top: true` front matter and assigns fallback preview images from the theme config.
- `themes/gal-theme/layout/post.ejs`, `page.ejs`, and partials under `layout/_partial/` handle post/page rendering and shared fragments.
- `themes/gal-theme/source/css/style.scss` is the main theme stylesheet entrypoint.
- `themes/gal-theme/source/js/blog.js` contains the main client-side behavior for the slideshow, search form validation, widget toggles, and back-to-top behavior.

## Content and search conventions

- The theme recognizes extra front matter used by the templates, especially `preview`, `preview_text`, `top`, and `comments`.
- Search is static/client-side: the theme search page reads `content.json`, so the repo depends on `hexo-generator-json-content` being present during builds.
- Theme README notes that comment system behavior is not fully testable locally and is typically verified after deployment.

## Notes from existing docs

- `README.md` documents the standard Hexo workflow for writing posts, generating the site, serving locally, and deploying.
- The README and theme README both mention historical Sass/npm compatibility issues. The current repo already uses `hexo-renderer-sass-next`, which is the intended renderer path here.
