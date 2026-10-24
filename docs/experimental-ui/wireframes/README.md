# Wireframes

Text wireframes for the [7 must-have journeys](../03a-must-have-journeys.md). They show **structure only**:
layout, hierarchy, type roles and components, for desktop, web and phone. No visual style yet, so any
theme can be applied later ([visual ideas](visual-ideas-later.md)).

## Read these first
| File | What |
|---|---|
| [00-foundations.md](00-foundations.md) | Platforms, navigation, URLs, the shell in 3 widths, type roles, spacing, theme tokens, icons, states, **HIG checklist** |
| [01-components.md](01-components.md) | 17 shared components (C1–C17): item row, tile, menu, header, toast, sheets… |

## Journeys
| Journey | Wireframes | Screens |
|---|---|---|
| MH1 Curate music | [MH1-curate.md](MH1-curate.md) | Discover · Search · Release · Artist · Library › Releases · Drop |
| MH2 Build a show playlist | [MH2-playlist.md](MH2-playlist.md) | New playlist · Playlist page · Adding from anywhere · Empty |
| MH3 Get show-ready | [MH3-show-ready.md](MH3-show-ready.md) | Inbox · Problem group · Updates review · Check Show |
| MH4 Play the show | [MH4-play.md](MH4-play.md) | Show › Prepare · Playing · Accessory · End of playlist |
| MH5 Go live and come off air | [MH5-live.md](MH5-live.md) | Go Live sheet · Sequence · Feed tag · Live console · Mixxx · End Show / Wrap Up |
| MH6 Get set up | [MH6-setup.md](MH6-setup.md) | Welcome · Fix an item · Pick a mode · Show checklist · Settings |
| MH7 Recover | [MH7-recover.md](MH7-recover.md) | Problem in place · Inbox · More info · Can't start |

## How to read a wireframe
- **Right margin** = type role of that line (`LT` Large Title … `C2` Caption 2; [00 §5](00-foundations.md#5-typography)) or component (`C1`…).
- **Single-character icons** stand in for role icons ([00 §8](00-foundations.md#8-icons)).
- **`[Button]`** = a button; the filled/primary one is named in the margin. `▾` = menu, `›` = navigates, `⋯` = item menu (C3).
- **Expanded** = desktop and wide web · **Compact** = phone and narrow web. Medium follows the
  [shell rules](00-foundations.md#4-the-shell-in-three-widths) and is drawn only where it differs.
- **HIG:** lines list the checks from [00 §10](00-foundations.md#10-hig-checklist-each-screen-is-checked-against-these-ids) worth noting for that screen. Every screen is meant to pass all of them; these are design intent, still to be verified in a prototype.

## Clickable prototype
The structure of all seven journeys, clickable with sample data: [prototype/index.html](../prototype/index.html) · [live link](https://claude.ai/artifact/RVYWMTsL9ApFnr5XHFHabD).

## Not wireframed yet
Should-have journeys (follow a search, inspect metadata, back up / move library), audition and
cue (planned upstream), record-only mode, episode packaging.
