# Repository Guidelines

## Project Structure & Module Organization
- Hugo site configured via `config.toml` with multilingual (English and Vietnamese) outputs and the `hugo-theme-learn` theme (submodule in `themes/hugo-theme-learn`).
- Content lives in `content/` using numbered folders (`1-Worklog/1.1-Week1`, `2-Proposal`, `3-BlogsTranslated`) and paired files for each language (`_index.md` and `_index.vi.md`). Use `weight`, `title`, `chapter`, and optional `pre` fields to control menus.
- `archetypes/default.md` defines the starter front matter for new pages; prefer creating new sections via `hugo new section/name` to keep metadata consistent.
- `static/` holds unprocessed assets; `public/` is build output and should not be edited by hand (CI regenerates it).

## Build, Test, and Development Commands
- Install Hugo Extended 0.134.3 (matches CI). Initialize the theme with `git submodule update --init --recursive` after cloning.
- Local preview: `hugo server -D --disableFastRender` to serve drafts and catch live changes at <http://localhost:1313>.
- Production build: `hugo --minify` (same as GitHub Actions) to emit `public/`. Use `hugo --panicOnWarning` locally if you want broken links or shortcode errors to fail the build.

## Coding Style & Naming Conventions
- Write content in Markdown with YAML front matter delimited by `---`. Keep titles sentence case; use `weight` for ordering instead of filename hacks.
- Maintain the numbered folder pattern and match language pairs (`_index.md` / `_index.vi.md`) to keep navigation aligned across locales.
- Prefer relative links (`./path`), shortcode notices (`{{% notice %}}`) and Hugo internal links over hard-coded URLs; avoid embedding secrets or tokens in content.
- For tables and lists, stay consistent with existing Markdown style (`-` for bullets, pipe tables for schedules). Keep `draft: true` only while iterating.

## Testing Guidelines
- Before opening a PR, run `hugo --minify` (or `hugo --panicOnWarning --minify`) and ensure the generated site loads correctly, navigation order is correct, and bilingual pages render without missing assets.
- If you change menus or weights, verify both locales via `hugo server -D` to confirm sidebars and breadcrumbs behave as expected.

## Commit & Pull Request Guidelines
- Use clear, imperative commits (e.g., `Add week7 worklog content`, `Fix vi sidebar weights`). Include a short scope in the first line; additional context can follow in the body.
- For PRs, include: what changed, why, how to review (links to preview/build commands), and screenshots/GIFs when altering layout or styling. Link related issues if applicable.
- Do not commit the `public/` output or local caches; CI handles deployment to the `gh-pages` branch via `.github/workflows/hugo.yml`.

## Security & Configuration Tips
- Keep `baseURL` in `config.toml` in sync with the deployment URL; mismatches break relative URLs when published.
- Avoid storing credentials or tokens in content, configs, or Git history. Secrets for deployment should stay in GitHub Actions settings, not in this repo.
