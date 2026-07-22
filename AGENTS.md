# AGENTS.md

Guidance for AI coding agents working in this repository.

## What this repo is

This is **`Allan-Nava/Allan-Nava`** — the special GitHub **profile README** repository.
Its `README.md` is rendered on <https://github.com/Allan-Nava> as the profile landing page.

There is **no application code, build step, or test suite**. The "product" is the
rendered `README.md` plus a set of GitHub Actions that keep it fresh automatically.

## Layout

| Path | Purpose |
|------|---------|
| `README.md` | The profile page. The only file most changes touch. |
| `.github/workflows/` | Automations that regenerate parts of the README. |
| `*.png`, `*.jpeg`, `*.jpg`, `_cover.PNG` | Social icons and banner referenced by the README (served via `raw.githubusercontent.com`). |
| `profile-3d-contrib/` | **Generated** 3D contribution SVGs. Do not hand-edit. |
| `renovate.json`, `.github/dependabot.yml` | Dependency automation config. |

## Automations (do not edit generated output by hand)

| Workflow | Writes to | Trigger |
|----------|-----------|---------|
| `.github/workflows/UPDATE-READMEV2.yml` | `README.md` between `<!--START_SECTION:activity-->` / `<!--END_SECTION:activity-->` | every 30 min + push |
| `.github/workflows/snake.yml` | `output` branch (SVG/GIF) | every 12 h + push |
| `.github/workflows/profile-3d.yml` | `profile-3d-contrib/` folder | daily |
| `.github/workflows/blog-posts.yml` | `README.md` between `<!-- BLOG-POST-LIST:START -->` / `<!-- BLOG-POST-LIST:END -->` | hourly |

The regions marked by those HTML comment markers are **machine-managed** — keep the
markers intact and never delete content inside them by hand; the workflows overwrite it.

## Conventions

- **Theme:** green `#10cf53` accent on black `#050505` background, white `#ffffff` text.
  Keep any new stat cards / badges consistent with this palette.
- **Images:** reference repo images with the full raw URL
  (`https://raw.githubusercontent.com/Allan-Nava/Allan-Nava/master/<file>`) so they
  render on the profile page, or a repo-relative path for generated files.
- **Username:** `Allan-Nava` in stat-service URLs; `allannava` on dev.to; `allan__nava` on X/Twitter.
- **Badges:** use `shields.io` `for-the-badge` style to match the existing tech-stack row.
- **Commits:** the existing history uses gitmoji-style prefixes (e.g. `:zap:`, `:robot:`). Match that style.

## Validating changes

There are no unit tests. Before committing:

- Preview `README.md` rendering (GitHub-flavored markdown).
- If you touch a workflow, validate the YAML, e.g.
  `ruby -ryaml -e 'Dir[".github/workflows/*.yml"].each{|f| YAML.load_file(f)}'`.
- All workflows rely only on the built-in `GITHUB_TOKEN` — do not introduce new
  secrets without flagging it, and note that `snake.yml` / `profile-3d.yml` need the
  repo's **Actions → Workflow permissions** set to *Read and write*.

## Do / Don't

- ✅ Edit `README.md` prose, badges, and layout freely.
- ✅ Add new automation workflows under `.github/workflows/`.
- ❌ Don't hand-edit content inside the marker regions or `profile-3d-contrib/`.
- ❌ Don't commit or push unless the user explicitly asks.
