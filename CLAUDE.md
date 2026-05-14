# CLAUDE.md

## Project

Personal Islamic-resources links page (Linktree-style). Static single-file
site (`index.html`) deployed via GitHub Pages.

- **Live**: https://abuyahyo.github.io/havola/
- **Repo**: `abuyahyo/havola` (only repo in the MCP scope)

## Files at repo root

- `index.html` — entire site (HTML + inline CSS + inline JS)
- `*.png` / `*.svg` — link-button icons (always lowercase names)
- `apple-touch-icon.png` — 180×180 iOS home-screen tile
- `avatar.jpeg` — Masjid an-Nabawi photo. Not shown on the page; only
  referenced by `og:image` / `twitter:image` for rich link previews.

## Design

- **Palette**: green gradient bg (#0a3d2e → #1a5d3a → #0a3d2e), gold accents
  (#d4af37, #f4d77a, #b8941f), cream icon bg (#fbfaf3)
- **Fonts**: Amiri (Cyrillic + base) and Scheherazade New (Arabic), loaded
  from Google Fonts with `display=swap`
- **Decorations**: bismillah at top, ornaments `۞ ❈ ۞`, footer dua
- **Animations**: container border pulse, bismillah glow, twinkling stars
  layer, `fadeInUp` entrance. All gated by `@media (prefers-reduced-motion)`.
  **No hover/tap animations on link buttons** — intentionally removed.

## Icon conventions

- Source resolution **180×180** (one icon is 360×360 — acceptable but prefer
  180 for parity)
- Filenames **always lowercase**. iOS Safari uploads as `Foo.PNG`; rename:
  ```sh
  git mv Foo.PNG foo.png.tmp && git mv foo.png.tmp foo.png
  ```
  (Two-step move because the FS may be case-insensitive.)
- Display size **52×52** via `.icon-img` (rounded square, gold border,
  cream bg)
- Icons that bake a gold border into the PNG visually appear thicker than
  ones that don't (double border). Aim for baked-in border for visual parity.

## Language toggle (uz / ar)

- `<html lang="uz" data-lang="uz">` is the default
- Pill switcher `.lang-switcher` with `<button class="lang-btn" data-lang="…">`
- Visibility rule: any element classed `lang-uz` is hidden when `data-lang=ar`
  and vice versa. The toggle buttons use `lang-btn` (not `lang-uz`/`lang-ar`)
  so they always show
- AR link buttons need `lang="ar" dir="rtl"` on the `<a>` so the icon flips
  to the right edge and the text flows RTL
- Selected language persists in `localStorage['havola-lang']`. A tiny
  head-level script applies the saved value before paint to avoid FOUC

## Link button pattern

```html
<a href="…" class="link-btn lang-uz" rel="noopener">
  <img class="icon icon-img" src="foo.png" alt="" width="52" height="52"
       loading="lazy" decoding="async">
  <span>Title — Description</span>
</a>
```

For Arabic add `lang="ar" dir="rtl"` to the `<a>`. Use **em-dash (—)** as
separator, never en-dash (–).

## Workflow for modifying the page

1. Branch is `claude/improve-file-UcKE1`
2. Sync before editing: `git stash && git pull origin main --rebase && git stash pop`
3. Edit / add files
4. Commit with a descriptive message ending with the Claude Code session URL
5. Push: `git push -u origin claude/improve-file-UcKE1 --force-with-lease`
   (force-with-lease needed because of the rebase)
6. Open PR via `mcp__github__create_pull_request` with a Summary + Test plan body
7. Squash-merge via `mcp__github__merge_pull_request` (user has been
   approving every change with this flow)
8. GitHub Pages picks up the change in 1–2 minutes

## Sandbox notes

- Live HTTP requests to `abuyahyo.github.io/*` are blocked — verify changes
  by inspecting repo contents (`mcp__github__get_file_contents`) instead
- Python with Pillow is installed; used for generating `apple-touch-icon.png`
  and inspecting PNG dimensions / sample pixels
