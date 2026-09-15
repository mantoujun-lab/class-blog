# Project Contribution Rules

## Project Overview

This repository is the Chinese class blog of the 2025 Computer Application Class 1 (25 级计算机应用 1 班). It is a **Hugo** static site using the **Stack** theme (`hugo-theme-stack`, vendored as a Git submodule), deployed on **Vercel**, and published at `https://blog.hjx-25pc1.xyz/`. Comments are provided by **Waline**.

The generated site contains a homepage with archive, category, tag-cloud, and search widgets; blog posts; archive and search pages; a custom 404 page; RSS feeds; `sitemap.xml`; and `robots.txt`.

There is no Node.js toolchain here: the repository has no `package.json`, and it is **not** a VitePress, Astro, or npm/pnpm project.

Licensing is split: source code (templates, configuration, scripts) is MIT (`LICENSE`), while all article content under `content/` is CC BY 4.0 (`LICENSE-CONTENT`).

## Repository Structure

- `hugo.toml`: the single site configuration file — permalinks, pagination, Markdown/highlight settings, SEO defaults, sidebar widgets, Waline comments, and the social menu.
- `archetypes/default.md`, `archetypes/post.md`: front matter templates used by `hugo new` (TOML `+++`).
- `content/post/*.md`: blog articles.
- `content/page/<name>/index.md`: standalone pages such as `archives` and `search`; these use YAML front matter with `layout`, `slug`, `menu`, and `outputs`.
- `layouts/`: project-level template overrides.
  - `layouts/404.html`: custom 404 page.
  - `layouts/robots.txt`: dynamic `robots.txt` template that derives the sitemap URL from `.Site.BaseURL`.
  - `layouts/_partials/head/head.html`: overrides the theme's head partial and appends the JSON-LD block.
  - `layouts/_partials/head/jsonld.html`: Schema.org `WebSite` node for every page plus `BlogPosting` for `content/post/` articles.
  - `layouts/_partials/article/components/details.html`, `author.html`: author display on article cards and detail pages.
- `static/`: files copied verbatim to the site root; article covers live in `static/images/covers/`.
- `assets/`: files processed by the Hugo asset pipeline (`favicon.png`, `icons/user.svg`, `funding_wechat.png`, `jsconfig.json`).
- `data/`, `i18n/`: no project-level override files exist here, since Git cannot track empty directories; the theme's own `data/` and `i18n/` files under `themes/hugo-theme-stack/` are the ones in effect.
- `themes/hugo-theme-stack/`: the Stack theme as a Git submodule — read it for reference, never edit it.
- `.github/workflows/build.yml`: CI build check (`hugo --gc --minify`); `.github/workflows/pullfrog.yml` is generated tooling and must not be edited.
- `vercel.json`: deployment settings (Hugo 0.165.0 Extended, build command, output directory, cache headers, `cleanUrls`, `trailingSlash`).
- `.pre-commit-config.yaml`: pre-commit hooks, with `themes/` excluded.
- `public/`, `resources/_gen/`, `.hugo_build.lock`: build output and lock files; git-ignored, never edited or committed.

Add new articles under `content/post/` and new standalone pages under `content/page/<name>/index.md`. If a page should appear in the site menu, declare it through the `menu` block in its front matter instead of editing theme templates. Use lowercase English letters, numbers, and hyphens for file names and slugs; avoid spaces.

## Content Guidelines

- Write documentation in clear, consistent Simplified Chinese unless the task explicitly requires another language.
- Use full-width Chinese punctuation (for example `，`, `。`, `、`, `；`, `：`, `？`, `！`, `（）`, and `「」`) in Chinese prose. Keep half-width punctuation inside fenced code blocks, inline code, URLs, file paths, English text, TOML/YAML frontmatter keys, `{{ }}` template expressions, and code-style syntax in tables.
- Keep heading levels continuous and use descriptive titles. Article headings start at `##`, because `title` already renders as the page heading and the table of contents is generated from level 2 through 4.
- Include a short introduction and practical examples where appropriate.
- Specify a language for every fenced code block so syntax highlighting and line numbers work correctly.
- Write command examples for Windows PowerShell unless another shell is explicitly required.
- Use complete HTTPS URLs for external links.
- Author article front matter in TOML (`+++`) following `archetypes/post.md`: `title`, `date`, `draft`, `description`, `categories`, `tags`, `keywords`, `author`, `image`, `robots`. Set `draft = false` before a post is published, because drafts are skipped by `hugo --gc --minify`.
- Put `description` on every article: it is used for the card subtitle, meta description, and JSON-LD output.
- Store images under `static/` and reference them with site-root paths such as `/images/covers/example.png`.
- Preserve the existing terminology, link structure, and page style when editing content.
- Never commit secrets, tokens, personal information, build output, or local environment files. The Waline `serverURL` in `hugo.toml` is public configuration, not a credential.

## Theme and Template Conventions

- `themes/hugo-theme-stack/` is a Git submodule. Never modify it: changes are lost on `git submodule update` and are excluded from pre-commit checks.
- Override a theme template by creating a file at the same relative path under the project's `layouts/` directory, since project templates take precedence in Hugo's lookup order.
- `layouts/_partials/head/head.html` deliberately mirrors the theme's head partial and ends with `partial "head/jsonld.html"`. When the theme's head changes, re-sync this override rather than deleting the JSON-LD include.
- Keep the SEO pipeline intact: meta description/keywords, canonical URL, Open Graph, Twitter Card, and JSON-LD are produced by these partials, and `tags`/`categories` feed `keywords`, `article:tag`, and `article:section`.
- Theme features available to content authors include code highlighting with line numbers, Mermaid diagrams, alert blockquotes, lightbox image galleries, a table of contents (on by default, and only rendered when the article has `##` or deeper headings), and shortcodes such as `bilibili`, `youtube`, `tencent`, `video`, `quote`, and `gitlab`.
- LaTeX math is opt-in per article: set `math = true` in the front matter so the KaTeX partial is loaded. `hugo.toml` already enables Goldmark passthrough for `\[...\]`, `$$...$$`, and `\(...\)`.
- Keep project-level templates ASCII-compatible HTML with LF endings; template comments may be written in either Chinese or English, matching the file being edited.

## Configuration Changes

- Edit `hugo.toml` for site metadata, permalinks, pagination, markup behavior, SEO defaults, widgets, comments, and menus.
- Consult `themes/hugo-theme-stack/config/_default/` to discover available theme parameters, then override them in `hugo.toml` instead of editing the theme.
- Preserve `baseURL`, `defaultContentLanguage`/`locale` (`zh-cn`), `hasCJKLanguage`, the permalink patterns (`post = "/p/:slug/"`, `page = "/:slug/"`), the `[outputs]` RSS settings, `[sitemap]`, and `enableRobotsTXT` unless a change is explicitly requested. Changing `baseURL` or permalinks breaks published links and feeds.
- `layouts/robots.txt` builds the sitemap URL from `.Site.BaseURL`; never hardcode the domain there.
- Add page-level SEO metadata through front matter (`description`, `keywords`, `author`, `image`, `robots`); site-wide defaults live under `[params]`.
- The `image` field is reused by Open Graph and JSON-LD, so reference covers with absolute site-root paths.
- Vercel pins Extended Hugo 0.165.0 while CI installs the latest Extended version; avoid features newer than 0.165.0 so deployments stay reproducible.

## Development and Verification

Requirements: Hugo Extended >= 0.165.0 (needed for SCSS compilation) and Git.

```powershell
# Fetch the theme submodule (required before the first build)
git submodule update --init --recursive

# Build the production site
hugo --gc --minify

# Start the local preview server
hugo server
```

The build output is written to `public/` and is git-ignored; inspect `public/index.html` when homepage behavior needs verification. Run `hugo --gc --minify` after any content, template, or configuration change, and confirm it finishes without new errors.

The build currently reports one pre-existing deprecation warning about `.Site.LanguageCode` originating from `layouts/_partials/head/jsonld.html`; it is not caused by new content. Prefer `.Site.Language.Locale` in new or edited template code.

If `pre-commit` is available, run `pre-commit run --all-files` to check TOML/YAML/JSON syntax and large files. Hugo rewrites `assets/jsconfig.json` path separators, so avoid re-adding line-ending auto-fix hooks that fight with it.

Do not run `npm run dev`, `pnpm docs:dev`, `pnpm docs:build`, or `astro dev`: this repository is a Hugo site and has none of those scripts.

## Git Conventions

The default branch is `main`. Before editing, confirm that unrelated worktree changes are not overwritten or reset.

Commit messages must use English Conventional Commits format:

```text
<type>(optional-scope): <imperative subject>
```

Allowed types include `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, and `chore`. Use a lowercase imperative subject, keep it at 50 characters or fewer, and omit the final period.

Additional rules:

- Keep text files at LF endings as enforced by `.gitattributes`; do not reintroduce CRLF normalization or `mixed-line-ending` hooks.
- Never commit generated output (`public/`, `resources/_gen/`) or `.hugo_build.lock`; they are intentionally git-ignored.
- Theme updates change the submodule pointer at `themes/hugo-theme-stack`; commit such pointer changes deliberately and describe the theme version in the commit body.
- Do not create branches, commits, or pushes unless explicitly requested.

## Change Checklist

1. Confirm that the requested scope does not conflict with existing worktree changes.
2. Check Markdown headings, links, code fences, and front matter (TOML `+++` for posts, YAML for pages).
3. Confirm that referenced images exist under `static/` and are referenced with site-root paths.
4. Run `hugo --gc --minify` and confirm that the build succeeds without new errors.
5. Inspect `public/index.html` when the homepage is affected.
6. Report the changes and verification result in concise English.
