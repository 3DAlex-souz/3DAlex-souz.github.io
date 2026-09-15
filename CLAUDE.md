# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A personal resume folder for Alexander Sokolov (Соколов Александр Викторович), synced via Yandex.Disk and also a git repo (branch `main`, no commits yet). Everything is in Russian; keep it that way unless asked otherwise. There is no build, lint, or test tooling — the only "run" step is a static file server.

Two deliverables live here:

1. **Static one-page site** — `index.html` + `style.css` at the repo root (the current focus).
2. **Printable resume** — `Docs/A. Sokolov_qw_resume_example.html` (older, self-contained).

Both draw their content from `Docs/My skills.xlsx`.

## Running / previewing

Preview the site with the `resume` config in `.claude/launch.json` (`python -m http.server 8000` → http://localhost:8000). Use `preview_start {name: "resume"}`, not Bash, to start it. Verify changes in the Browser pane at both mobile (~375px) and desktop widths — the layout is mobile-first with a single `@media (min-width: 640px)` breakpoint in `style.css`.

## Content source of truth

`Docs/My skills.xlsx` — single sheet, two columns: `Раздел` (section) / `Что указать` (what to write). Sections: Личная информация, Цель / Позиция, Опыт работы, Образование, Навыки, Сертификаты / Курсы, Языки, Проекты. Edit content here first, then propagate to the HTML. `openpyxl` is not installed; read it via `zipfile` + `xl/sharedStrings.xml`, or use the `xlsx` skill.

Contact data (phone `+7 903 579-18-43`, email `waidosddcube@gmail.com`, GitHub `3DAlex-souz`) must stay consistent across the xlsx, `index.html` (hero contacts + footer icons) and the Docs resume. The Docs resume still links the old handle `@Waidoss` — `3DAlex-souz` is the current one.

## Site (`index.html` + `style.css`)

- Dark theme, single centered column. All colors, sizes and the font stack are CSS custom properties in `:root` at the top of `style.css` — change tokens there rather than hardcoding values. Brand colors: `--accent` (navy-derived `#6b7fd7`) for links/hover, `--accent-red` (`#b21f1f`) used only for the section-title left marker.
- Structure: `header.hero.wrap` (base64 photo, name, role, city, `.contacts` buttons) → `section.section` blocks with `h2.section__title` (О себе, Ключевой проект, Навыки, Образование и языки) → `footer.footer.wrap` with inline-SVG icon links and `© <year>`.
- BEM-ish naming: `.hero__photo`, `.card__head`, `.card__title`, `.stats`, `.skills` > `h3` groups. Reuse these blocks for new content.
- The photo is embedded as a base64 `data:image/jpeg` (source: `Docs/A_Sokolov.JPG`); re-encode if the photo changes. Keep `<meta name="description">`, `<title>` and `theme-color` in sync with content/theme changes.
- External links: `souz-m3d.online`, `souz-m3d.ru`, `stabledif.ru/comfyui` — keep `target="_blank" rel="noopener"`.

## Printable resume (`Docs/A. Sokolov_qw_resume_example.html`)

Self-contained single file: inline `<style>`, Google Fonts via `@import`, base64 photo. Has an `@media print` block — keep it working when changing styles.

- Section order: Профессиональная цель → Опыт работы → Технологический стек → Ключевые компетенции → Образование → Языки → Повышение квалификации → closing `.quote` → `.footer`.
- Each section is `<div class="section"><h2 class="section-title">EMOJI Title</h2>…</div>`; jobs use `.job` / `.job-header` / `.job-title` / `.job-company` / `.job-period` / `.job-description`; skills use `.skills-grid` > `.skill-category`; emphasis uses `<span class="highlight">`.
- Brand colors: navy `#1a2a6c`, red `#b21f1f`. Reuse them rather than introducing new colors.
- `.footer` contains "Последнее обновление: <date>" — bump it whenever the content changes.
