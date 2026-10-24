# Visual ideas for later (from musicindex.org)

**Not applied yet.** The wireframes use roles only ([00 §7](00-foundations.md#7-theme-tokens-names-only)).
This page records what musicindex.org's live app (`search.html`) does, so a first theme can map
roles to it. Source: `v4vmm/docs/architecture/screen-inventory/design-system.md` §1.

| Idea from musicindex.org | Maps to our token / component |
|---|---|
| One brand accent, blue (`#2d7bff` dark / `#0a5bd6` light) | `accent`, `focus.ring` |
| **One hue per entity kind**: release tan, track teal, playlist mint, artist pink, business roles violet | `entity.*`. Already kept separate from status colours |
| **Red is reserved for live** | `state.live`. Matches our ● LIVE rule |
| Solid dot = confirmed, hollow ring = inferred | Eyebrow dot in [C4](01-components.md#c4--page-header), artist "name only" (MH1 1.4) |
| Entity colour only as dots, rings and placeholder tints, **never coloured text or filled badges** | Satisfies AX2 / AX3 by design |
| Missing art = flat entity tint + two-letter monogram (no emoji, no gradient) | `▣` placeholder in [00 §8](00-foundations.md#8-icons) |
| **Shadow only on artwork**, scaled by size. Chrome uses hairlines | `shadow.art`; chrome uses `separator` |
| Selection "gloss": accent gradient + inset highlight | `bg.selected` as a material, not a flat fill |
| Hero: blurred, saturated art glow + film grain + scrim fading into the page | Page header (C4) backdrop. Must turn off with Reduce Transparency / Reduce Motion |
| Blurred, content-aware top bar (transparent when there's no title) | `material.toolbar` |
| Pills: radius = height ÷ 2; frosted pill buttons on the hero | `radius.pill`; secondary buttons on C4 |
| Primary hero action inverted to solid text colour | Primary button style option |
| Radii in use: 4 · 6 · 8 · 10 · 12 · 22 | `radius.control` 6–8, `radius.art.sm` 10, `radius.art.lg` 12 |
| Tiles min 150 px; rows 52–64 px tall | `art.tile` 150 ✓; our `row.compact` 56 ✓ |
| Figtree variable font as brand face; SF on Apple devices first | Theme font family per role (LT/T1 display face) |
| Tight display tracking on hero titles (−0.035 em), uppercase eyebrow (+0.08 em) | LT/T1 and eyebrow letter-spacing in theme |
| Phone: two panes stack, the inspector becomes a full-screen overlay with Back | Same as our compact shell |
| Compare table with "differs" cells tinted by accent | Sources inspector (C3 → Show Sources) |

**Order to apply:** colours (accent, entity, live) → artwork treatment (shadow, placeholders) →
hero backdrop → materials → font. Each step is a theme change only; no wireframe moves.
