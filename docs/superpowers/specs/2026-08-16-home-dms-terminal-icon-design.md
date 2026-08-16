# Home / DMs terminal icon

## Goal

Replace the Discord Home / DMs button glyph with a cyberpunk **terminal** icon (`>_` style), scoped to that button only.

## Context

- Night City is a single-file CSS theme (`nightcity24.theme.css`) on system24.
- system24 documents `--custom-dms-icon` / `--dms-icon-svg-url`, but the **implementation is not in the system24 build** (it exists in midnight). Setting vars alone does nothing today.
- Night City currently has `--custom-dms-icon: hide`, so the Home glyph is blank unless we add our own rules.

## Approach

Bake Home/DMs icon CSS into the theme (midnight-style mask), self-contained:

1. Set `--custom-dms-icon: custom`.
2. Set `--dms-icon-svg-url` to an **inline SVG data-URI** of a simple terminal / chevron glyph (monochrome, mask-friendly).
3. Keep existing colors: teal idle (`#39c4b6`), cyan hover/selected (`#54c1e6`).
4. Keep `--custom-dms-background: color` with Night City black pill; wire background with the same resilient selector so it actually applies.
5. **No spin animation** on hover (preserves the performance pass).

## Selectors

Use Discord’s stable attribute, not hashed class names:

- `[data-list-item-id='guildsnav___home']`
- Child wrapper via `[class*="childWrapper"]` (or direct child) for mask host
- Hide stock `svg` inside that button only

## Out of scope

- Toolbar, user panel, channel-type icons (later)
- External icon CDNs / cybercore package
- Changing other brand/chrome colors

## Success

Home / DMs shows a teal terminal glyph; cyan on hover/selected; black pill background still works; no other UI icons changed.
