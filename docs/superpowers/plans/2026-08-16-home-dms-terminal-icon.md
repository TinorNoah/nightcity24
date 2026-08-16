# Home/DMs Terminal Icon Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [x]`) syntax for tracking.

**Goal:** Show a cyberpunk terminal (`>_`) glyph on Discord’s Home/DMs button via CSS mask.

**Architecture:** system24’s `--custom-dms-icon` vars are documented but unimplemented in the build. Night City bakes midnight-style mask rules using the stable `guildsnav___home` attribute and an inline SVG data-URI.

**Tech Stack:** Single-file CSS theme (`nightcity24.theme.css`), CSS `mask-image`, system24 option vars.

## Global Constraints

- Self-contained theme file only (inline SVG data-URI, no external icon CDN).
- Home/DMs button only — no toolbar / user-panel icon swaps.
- Teal idle `#39c4b6`, cyan hover/selected `#54c1e6`.
- No hover spin animation (perf).
- Spec: `docs/superpowers/specs/2026-08-16-home-dms-terminal-icon-design.md`

---

## File map

| File | Role |
| --- | --- |
| `nightcity24.theme.css` | Set `--custom-dms-icon: custom` + SVG URL; add Home button mask + background rules |

---

### Task 1: Enable custom DMS icon vars + terminal SVG

**Files:** `nightcity24.theme.css` (body options block ~lines 41–50)

- [x] Change `--custom-dms-icon` from `hide` to `custom`
- [x] Set `--dms-icon-svg-url` to an inline monochrome terminal SVG data-URI suitable for masking (simple `>_` / terminal window glyph, black fills)
- [x] Keep `--dms-icon-svg-size: 90%`, teal/cyan color vars, `--custom-dms-background: color`, `--dms-background-color: #0A0A0A`

**Terminal SVG (inline, mask-friendly):** use a minimal 24×24 glyph, e.g. rounded rect terminal frame + chevron+underscore path, `fill="black"` (mask uses luminance/alpha).

### Task 2: Bake Home button CSS (mask + background)

**Files:** `nightcity24.theme.css` (new section near Equibop patches / after `#app-mount` icon vars)

- [x] Add resilient selectors (no hashed Discord classes):
  - `[data-list-item-id='guildsnav___home'] [class*='childWrapper']` (and/or direct child wrapper pattern)
- [x] Background: apply `background: var(--dms-background-color)` when custom background color is intended
- [x] Hide stock `svg` inside the Home button
- [x] `::before` mask host: absolute, ~65% size, `background: var(--dms-icon-color-before)`, `mask-image: var(--dms-icon-svg-url)`, `mask-size/position/repeat` from vars
- [x] Hover + selected: `background: var(--dms-icon-color-after)` on `::before` only (no rotate/scale)
- [x] Ensure childWrapper is `position: relative` / flex-centered enough for the mask to sit centered (match Discord’s existing layout)

### Task 3: Ship

- [x] Bump `@version` patch (e.g. 3.0.1 → 3.0.2) if description already at 3.0.1
- [x] Commit theme (+ this plan if not committed)
- [x] Push `main`
- [x] Manual check: reload theme in Equibop — Home shows teal terminal; cyan on hover; black pill remains

## Done when

Home/DMs shows the terminal glyph with Night City colors; no other icons changed; theme still one-file install.
