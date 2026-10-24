# MH1 · Curate music: find, take, drop

Journey: [MH1](../03a-must-have-journeys.md#mh1--curate-music) · Principles P1 P2 P3 P4 P7 ·
Components: [C1](01-components.md#c1--item-row) [C2](01-components.md#c2--release-tile)
[C3](01-components.md#c3--item-menu) [C4](01-components.md#c4--page-header)
[C5](01-components.md#c5--section-header-and-shelf) [C6](01-components.md#c6--selection-mode-and-action-bar)
[C7](01-components.md#c7--toast) [C8](01-components.md#c8--confirm-sheet) [C14](01-components.md#c14--search-field-and-suggestions)

**Target:** 1 step to take anything from any row. **Today:** 2–9.

| Screen | Route | Journey step |
|---|---|---|
| [1.1 Discover](#11-discover) | `/discover` | Browse what's new → Get |
| [1.2 Search results](#12-search-results) | `/search?q=` | Find something specific → Get |
| [1.3 Release page](#13-release-page) | `/release/:guid` | Look closer → Get / Add / Drop |
| [1.4 Artist page](#14-artist-page) | `/artist/:guid` | Explore an artist → Get missing |
| [1.5 Library › Releases](#15-library--releases) | `/library/releases` | See what I own, what's dropped |
| [1.6 Drop and delete](#16-drop-and-delete-files) | (toast, sheet) | Drop → Undo / Delete files |

---

## 1.1 Discover

### Expanded
```
┌──────────────────────────────────────────────────────────────────────────────────────────────┐
│ ●●●  ◧  ‹ ›   Discover                         [⌕ Search music and actions   ⌘K]  [⤓]  [◨]   │ HL
├───────────────────┬──────────────────────────────────────────────────────────────────────────┤
│ Discover          │ Discover                                                                 │ LT
│ ▸ New Releases    │                                                                          │
│   bluegrass    3  │ New Releases                                    [Grid ▾]   See All ›     │ T2
│   lo-fi           │ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐         │
│ Library           │ │  ▓▓▓▓▓▓  │ │  ▓▓▓▓▓▓  │ │  ▓▓▓▓▓▓  │ │  ▓▓▓▓▓▓  │ │  ▓▓▓▓▓▓  │         │
│   Releases        │ │      [⤓] │ │      [✓] │ │      [⤓] │ │     [◔]  │ │      [⤓] │         │ C1 state
│   Artists         │ └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘         │
│   Tracks          │ Deathdreams  Way To Go    Bloodshot   Think EP     Jam With              │ BD
│ Playlists         │ Survival G…  Survival G…  The Doerf…  The Doerf…   Nate                  │ C1
│   ★ Episode 42    │ Album        Album        Single      EP           Album                 │ C1
│   Winter Mix      │                                                                          │
│   + New Playlist  │ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐         │
│ Show              │ │   …      │ │   …      │ │   …      │ │   …      │ │   …      │         │
│ ───────────────── │                                                                          │
│ Inbox          3  │ bluegrass · 3 new since Monday                             Open ›        │ T2 · HL
│ Settings          │ ┌──────────┐ ┌──────────┐ ┌──────────┐                       ›           │ shelf C5
│                   │ │  ▓▓ NEW  │ │  ▓▓ NEW  │ │  ▓▓ NEW  │                                   │
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘
```
- The page is **one scroll of sections**: New Releases (grid, infinite load with a *Load More* button
  at the end), then one shelf per saved search with a "new since" count (J06, later).
- The tile body opens the release page (1.3). The corner button is the C1 inline action, so Get is **1 step**.
- `[Grid ▾]` switches between grid and list (C1 rows), and the choice is remembered.

### Compact
```
┌────────────────────────────────────┐
│ 9:41                        ▮▮▮ ◔  │
│                   Inbox 3     ⋯    │
│ Discover                           │ LT
│ New Releases            See All ›  │ T2
│ ┌──────────────┐ ┌──────────────┐  │
│ │   ▓▓▓▓▓▓▓▓   │ │   ▓▓▓▓▓▓▓▓   │  │
│ │          [⤓] │ │          [✓] │  │
│ └──────────────┘ └──────────────┘  │
│ Deathdreams      Way To Go         │ BD
│ Survival Guide   Survival Guide    │ C1
│ ┌──────────────┐ ┌──────────────┐  │
│ │      …       │ │      …       │  │
├────────────────────────────────────┤
│ Discover  Library  Show  Search    │ C2
└────────────────────────────────────┘
```

**States:** loading = grey tile skeletons, 2 rows · offline = banner *"MusicIndex can't be reached. Your
library still works."* + Library shortcut · empty saved search = *"Nothing new since Monday."*

**HIG:** TY1–6 ✓ · LA1–5 ✓ (grid reflows: 5 → 3 → 2 columns) · ST1 ✓ ST2 ✓ ST3 ✓ (one primary per tile) ST5 ✓ ST8 ✓ · AX1 (tile = "Deathdreams, album by Survival Guide, not in library. Get.") AX2 ✓ (state = glyph + label)

---

## 1.2 Search results

### Expanded
```
┌──────────────────────────────────────────────────────────────────────────────────────────────┐
│ ●●●  ◧  ‹ ›   Search                          [⌕ doerfel                     ✕ ]  [⤓]  [◨]   │
├───────────────────┬──────────────────────────────────────────────────────────────────────────┤
│ (sidebar)         │ Results for "doerfel"                               [Save Search]        │ LT · button
│                   │ ( Top ) ( Releases 23 ) ( Artists 3 ) ( Tracks 40 )                      │ segmented, HL
│                   │                                                                          │
│                   │ Artists                                                    See All ›     │ T2
│                   │ ◉  The Doerfels          Artist · 1 of 4 in library   [Get 3 Missing]  ⋯ │ C1 artist
│                   │ ◉  Doerfel Family Blue…  Artist · 2 releases          [Get 2 Missing]  ⋯ │
│                   │                                                                          │
│                   │ Releases                                                   See All ›     │ T2
│                   │ ▣  Disco Swag – The Album   The Doerfels · Album · 2024  ✓          ⋯    │ C1
│                   │ ▣  Think EP LIVE!           The Doerfels · EP · 2025          [⤓ Get] ⋯  │
│                   │ ▣  Come Back To Me          The Doerfels · Single · 2023      [⤓ Get] ⋯  │
│                   │                                                                          │
│                   │ Tracks                                                     See All ›     │ T2
│                   │ ▣  Bloodshot Lies        The Doerfels · Single   3:36   [⤓ Get]       ⋯  │ C1
│                   │ ▣  3 Years               The Doerfels · Album    3:32   [+ Ep 42]     ⋯  │ owned → quick add
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘
```
- **Top** shows a few of each kind; the other segments show one kind in full.
- Every result is a C1 row with the **same** inline action and menu as anywhere else. Tracks can be
  got or added to a playlist straight from here, which today takes 9 steps.
- Typing in the field shows suggestions first (C14), including **actions**. Pressing Return shows this page.

### Compact
```
┌────────────────────────────────────┐
│ [⌕ doerfel                    ✕ ]  │
│ (Top)(Releases)(Artists)(Tracks)   │ segmented, scrolls if needed
│ Artists                            │ T2
│ ◉ The Doerfels      [Get 3]        │ C1
│   Artist · 1 of 4 in library       │
│ Releases                           │
│ ▣ Disco Swag – The…        ✓       │
│   The Doerfels · Album             │
│ ▣ Think EP LIVE!        [⤓ Get]    │
│   The Doerfels · EP                │
├────────────────────────────────────┤
│ Discover  Library  Show  Search    │
└────────────────────────────────────┘
```

**Steps:** ⌘F · type · Return · Get = **4** for one release; **+1 per extra**; artist "Get 3 Missing" = **4** total.

**HIG:** TY ✓ · LA ✓ · ST3 (Save Search is secondary, not filled) · ST7 (results keyboard-navigable; ⏎ opens, ⌘D gets) · AX1 ✓

---

## 1.3 Release page

### Expanded
```
┌──────────────────────────────────────────────────────────────────────────────────────────────┐
│ ●●●  ◧  ‹ ›   Deathdreams                      [⌕ Search music and actions   ⌘K]  [⤓]  [◨]   │
├───────────────────┬──────────────────────────────────────────────────────────────────────────┤
│ (sidebar)         │ ┌────────────┐  ● RELEASE · ALBUM                                        │ C1 caps
│                   │ │            │  Deathdreams                                              │ T1
│                   │ │  ▓▓▓▓▓▓▓▓  │  Survival Guide ›                                         │ HL link
│                   │ │  ▓▓▓▓▓▓▓▓  │  Dec 16, 2023 · 11 tracks · 44 min · Not in library       │ SH
│                   │ │            │                                                           │
│                   │ └────────────┘  [ ⤓ Get Album ]   [ + Add to Playlist ▾ ]   ⋯            │ primary · secondary
│                   │                 'deathdreams' was written and performed by Emily…  More  │ CO
│                   │                                                                          │
│                   │  #   Title                                         Time                  │ FN header
│                   │  1   Bad Little Seed                               4:09   [⤓ Get]   ⋯    │ C1 track
│                   │  2   Sour Sorrow                                   4:24   [⤓ Get]   ⋯    │
│                   │  3   Pie                                           4:07   [⤓ Get]   ⋯    │
│                   │  …                                                                       │
│                   │                                                                          │
│                   │ More by Survival Guide                                     See All ›     │ T2
│                   │ ┌──────┐ ┌──────┐ ┌──────┐                                               │ shelf C5
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘
```

**Header primary action by state** (C4):

| State | Primary | Secondary | `⋯` adds |
|---|---|---|---|
| Not in library | **⤓ Get Album** | + Add to Playlist ▾ (gets too) | Copy Link |
| Getting | **◔ Getting 4 of 11** (✕ to cancel queued) | + Add to Playlist ▾ | — |
| Partly in library | **⤓ Get 7 More** | + Add to Playlist ▾ | Drop… |
| In library | **+ Add to Playlist ▾** (or **+ Add to Episode 42** if pinned) | — | Show Sources, Look Up on MusicBrainz, Drop |
| Dropped | **Get Back** | — | Delete Files… |

- The **inspector** (◨) shows *Details* for the release: feed, publisher, payment routes, sources.
  This keeps provenance one click away without crowding the page (P3: details are an inspection layer).

### Compact
```
┌────────────────────────────────────┐
│ ‹ Discover                    ⋯    │
│         ┌──────────────┐           │
│         │  ▓▓▓▓▓▓▓▓▓▓  │           │ art 60%
│         │  ▓▓▓▓▓▓▓▓▓▓  │           │
│         └──────────────┘           │
│           Deathdreams              │ T1 centred
│         Survival Guide ›           │ HL
│   Album · 2023 · 11 tracks         │ SH
│ ┌────────────────────────────────┐ │
│ │          ⤓ Get Album           │ │ primary full width
│ └────────────────────────────────┘ │
│ ┌──────────────┐ ┌───────────────┐ │
│ │ + Playlist   │ │ ⋯ More        │ │
│ └──────────────┘ └───────────────┘ │
│ 1  Bad Little Seed   4:09  [⤓]     │ C1
│ 2  Sour Sorrow       4:24  [⤓]     │
├────────────────────────────────────┤
│ Discover  Library  Show  Search    │
└────────────────────────────────────┘
```

**HIG:** TY2 ✓ (T1 is the only title; toolbar repeats it as HL once scrolled) · TY5 ✓ (times) · LA4 ✓ · ST3 ✓ · ST4 ✓ (Drop undoable, no confirm) · AX5 ✓ (order: art, title, artist, meta, actions, tracks)

---

## 1.4 Artist page

### Expanded
```
┌───────────────────┬──────────────────────────────────────────────────────────────────────────┐
│ (sidebar)         │ ╭────────────╮  ● ARTIST                                                 │ C1 caps
│                   │ │   ▓▓  ▓▓   │  The Doerfels                                             │ T1
│                   │ │   ▓▓  ▓▓   │  12 releases · 4 in library                               │ SH
│                   │ ╰────────────╯  [ ⤓ Get 8 Missing ]   ⋯                                  │ primary
│                   │                                                                          │
│                   │ Albums & EPs                                               See All ›     │ T2
│                   │ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐                                      │ C2 tiles
│                   │ │ [✓]  │ │ [⤓]  │ │ [⤓]  │ │ [✓]  │                                      │
│                   │                                                                          │
│                   │ Singles · 7                                                See All ›     │ T2
│                   │ ▣  Bloodshot Lies        2025 · Single                [⤓ Get]   ⋯        │ C1
│                   │ ▣  Come Back To Me       2023 · Single                  ✓       ⋯        │
│                   │                                                                          │
│                   │ Appears On                                                 See All ›     │ T2
│                   │ Shares Releases With   ◉ Ben Doerfel   ◉ Nate Grower                     │ T2 · C1 artist
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘
```
- An unconfirmed artist (name only) uses the same page. The eyebrow reads **○ NAME ONLY** (hollow dot +
  word) with one line of CO: *"Grouped by name. Different artists may share it."*
- Compact: header centred like 1.3, sections stack, shelves scroll.

**HIG:** ST3 ✓ (one primary: Get 8 Missing) · AX2 ✓ (confirmation = dot shape + word, not colour)

---

## 1.5 Library › Releases

### Expanded
```
┌───────────────────┬──────────────────────────────────────────────────────────────────────────┐
│ Library           │ Releases                                    [Select]   [List ▾]          │ LT
│ ▸ Releases        │ ( All 214 ) ( Needs Attention 3 ) ( Dropped 12 )         Sort: Recent ▾  │ segmented
│   Artists         │                                                                          │
│   Tracks          │ ▣  Disco Swag – The Album   The Doerfels · Album · 13 tracks    ✓    ⋯   │ C1
│                   │ ▸ ▣ Singles by Haleen       7 singles · 5 in library        [Get 2]  ⋯   │ group row
│                   │ ▣  Deathdreams              Survival Guide · Album    ⚠ [Fix]       ⋯    │
│                   │ …                                                                        │
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘

Dropped filter:
│ ( All 214 ) ( Needs Attention 3 ) (• Dropped 12 )                        [Delete All Files…] │
│ Dropped releases keep their files until you delete them. 1.2 GB.                             │ CO
│ ▣  Old Album           Artist · Album · 320 MB                        [Get Back]     ⋯       │ ⋯ → Delete Files…
```
- **Dropped** replaces today's *Settings › Diagnostics › Cached files*: same data, now next to the library.
- Singles of one artist group into one row (expand ▸, or open the group page). The group row uses C1's inline action ("Get 2").

**HIG:** ST4 ✓ (Delete All Files… = destructive + confirm sheet, because it can't be undone) · ST8 ✓ (empty Dropped: *"Nothing dropped."*)

---

## 1.6 Drop and delete files

```
1. Row ⋯ → Drop (or ⌫, or swipe left on touch)

   ┌──────────────────────────────────────────────────────┐
   │ ✓  Dropped "Deathdreams". Files kept.      [Undo]    │   C7 toast
   └──────────────────────────────────────────────────────┘

2. Later, Dropped filter → ⋯ → Delete Files…

   ┌────────────────────────────────────────────────────┐
   │ Delete Files for "Deathdreams"?                    │  T3
   │ 11 files · 320 MB will be removed from disk.       │  CO
   │ You can get the release again from MusicIndex.     │
   │                       [ Cancel ]  [ Delete Files ] │  destructive
   └────────────────────────────────────────────────────┘
```
If a dropped release is in a playlist, the toast says so: *"Dropped. Still in 2 playlists."* **[Undo]**.

**Steps:** Drop = **1** (+ Undo) · free disk = **+2**. **Today:** ~8 across two sections, no confirm.

---

## Selection (applies to 1.1–1.5)
⌘-click / Select → C6 bar: **7 Selected · [⤓ Get 5] · [+ Add to Playlist ▾] · [Drop 2] · [Done]**.
Works the same on Discover, search, artist, release and Library lists.
