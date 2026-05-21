# CLAUDE.md

## Project

Personal Islamic-resources links page (Linktree-style). Static single-file
site (`index.html`) deployed via GitHub Pages.

- **Live**: https://abuyahyo.github.io/havola/
- **Repo**: `abuyahyo/havola` (only repo in the MCP scope)

## Files at repo root

- `index.html` — entire site (HTML + inline CSS + inline JS)
- `apple-touch-icon.png` — 180×180 iOS home-screen tile
- `avatar.jpeg` — Masjid an-Nabawi photo. Not shown on the page; only
  referenced by `og:image` / `twitter:image` for rich link previews.
- `*.png` / `*.svg` — legacy link-button icons. Not rendered by the
  current editorial design but kept on disk (some are referenced by the
  individual sub-sites linked from here).

## Design

Editorial minimalism — typography and hairlines do the work. No cards,
no glass, no icons.

- **Background**: deep midnight gradient (`#0a1024 → #050816`) with one
  soft radial nebula at the top. Very faint static starfield, no
  shooting star, no animation. `theme-color` meta is `#0a1024`.
- **Palette**:
  - `--ink` `#ece4cc` — primary text (warm cream)
  - `--ink-soft` `#b8b09a` — secondary text
  - `--ink-muted` `#837d6a` — captions, uppercase labels
  - `--gold` `#caa647` — hairlines, chevrons, lang-pill underline
  - `--gold-bright` `#e9c97a` — bismillah, hover highlight, active pill
  - `--hairline` `rgba(202, 166, 71, 0.18)` — list dividers
- **Fonts**: Cormorant Garamond (display serif, Cyrillic-ext covers
  Uzbek), Nunito (sans, captions, lang switcher), Scheherazade New
  (Arabic). All loaded from Google Fonts with `display=swap`.
- **Structure** (top to bottom):
  1. Masthead — bismillah → 44×1px gold rule → name (`<h1>`) → role
  2. Lang switcher — three text buttons separated by `·`, active gets
     a gold underline
  3. Entries — vertical list of `<a class="entry">` rows separated by
     gold hairlines (1px). Each row: serif title + optional uppercase
     sans subtitle + gold `›` chevron
  4. Colophon — small italic dua, centred
- **Hover**: only on `.entry` rows. Title becomes `--gold-bright`,
  chevron nudges 5px and brightens. Disabled by `prefers-reduced-motion`.

## Entry pattern

```html
<a href="…" class="entry lang-uz" rel="noopener">
  <span class="entry-body">
    <span class="entry-title">Қуръони Карим</span>
    <span class="entry-sub">Ўзбекча таржима</span>
  </span>
  <span class="entry-arrow" aria-hidden="true">›</span>
</a>
```

- `entry-sub` is optional (some titles stand alone).
- For the title/subhead split use the two spans, not an em-dash inside
  a single field.
- Arabic entries get `lang="ar" dir="rtl"` on the `<a>`. The chevron
  stays `›` in HTML and is flipped via `transform: scaleX(-1)`.

## Section toggle (uz / ar / windows)

- `<html lang="uz" data-lang="uz">` is the default
- `.lang-switcher` holds three `<button class="lang-btn" data-lang="…">`
- Modes: `uz`, `ar`, `windows`. The toggle uses class `lang-btn` (not
  `lang-uz`/`lang-ar`) so the buttons themselves always show.
- Visibility rule: any element classed `.lang-X` is hidden when
  `data-lang ≠ X`. In `windows` mode that hides the bismillah and
  footer dua too — by design.
- Selected mode persists in `localStorage['havola-lang']`. Allowlist is
  `['uz', 'ar', 'windows']` — anything else falls back to `uz`. A tiny
  head-level script applies the saved value before paint to avoid FOUC.

## Workflow for modifying the page

1. Branch is `claude/liquid-glass-design-Kw0EE`
2. Sync before editing: `git stash && git pull origin main --rebase && git stash pop`
3. Edit / add files
4. Commit with a descriptive message ending with the Claude Code session URL
5. Push: `git push -u origin claude/liquid-glass-design-Kw0EE`
6. Open PR via `mcp__github__create_pull_request` with a Summary + Test plan body
7. Squash-merge via `mcp__github__merge_pull_request` (user has been
   approving every change with this flow)
8. GitHub Pages picks up the change in 1–2 minutes

## Sandbox notes

- Live HTTP requests to `abuyahyo.github.io/*` are blocked — verify
  changes by inspecting repo contents (`mcp__github__get_file_contents`)
  instead.
- Python with Pillow is installed; used in the past for generating
  `apple-touch-icon.png` and inspecting PNG dimensions / sample pixels.
