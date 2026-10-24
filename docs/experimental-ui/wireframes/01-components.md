# 01 · Components

The building blocks the journey wireframes reuse. Defining each one once is what makes
**P1, "same item, same actions, everywhere"** ([principles](../03a-must-have-journeys.md#principles-every-journey-follows))
true by construction. Type codes (`BD`, `SH`, …) come from [00 §5](00-foundations.md#5-typography).

| ID | Component | Used in |
|---|---|---|
| [C1](#c1--item-row) | Item row (release, track, artist) | everywhere |
| [C2](#c2--release-tile) | Release tile | Discover, artist page, Library |
| [C3](#c3--item-menu) | Item menu (`⋯` and right-click) | every row, tile and header |
| [C4](#c4--page-header) | Page header | release, artist, playlist, Library lists |
| [C5](#c5--section-header-and-shelf) | Section header and shelf | Discover, artist page |
| [C6](#c6--selection-mode-and-action-bar) | Selection mode and action bar | every list |
| [C7](#c7--toast) | Toast (with Undo) | after any action |
| [C8](#c8--confirm-sheet) | Confirm sheet | go live, end show, delete files |
| [C9](#c9--inbox-item) | Inbox item | Inbox, check show |
| [C10](#c10--activity-popover) | Activity popover | toolbar `⤓` |
| [C11](#c11--checklist-row) | Checklist row | setup, check show, go-live progress |
| [C12](#c12--form-row-with-test) | Form row with Test | setup, settings |
| [C13](#c13--stage-bar) | Stage bar | Show |
| [C14](#c14--search-field-and-suggestions) | Search field and suggestions (⌘K) | toolbar, Search tab |
| [C15](#c15--empty-state) | Empty state | every list |
| [C16](#c16--show-accessory) | Show accessory | shell |
| [C17](#c17--transport) | Transport | Show, accessory |

---

## C1 · Item row

One row layout for releases, tracks and artists. Only the **inline action** changes, and it
follows the item's state (P3), never where the row is shown.

```
Expanded / medium (pointer, row.regular 44 pt)
┌──────────────────────────────────────────────────────────────────────────────────────┐
│ ▣  Title of the Thing                                   State   3:42   [Action]   ⋯  │  BD · C1 · BD(tabular)
│    Artist · Single · 2026                                                            │  SH (fg.secondary)
└──────────────────────────────────────────────────────────────────────────────────────┘
 art  ← one leading edge for art and text                 badge  dur.   inline     menu

Compact (touch, row.compact 56 pt)
┌────────────────────────────────────┐
│ ▣  Title of the Thing     [Action] │  BD
│    Artist · Single · 2026  ✓       │  SH + state glyph
└────────────────────────────────────┘
  (⋯ = long-press / swipe; swipe left = Drop, swipe right = + Show playlist)
```

**State → inline action** (the only thing that varies)

| State (P3) | Badge (icon + word, AX2) | Inline action | Accessibility label |
|---|---|---|---|
| Not in library | none | **[⤓ Get]** | "Get *Title*" |
| Getting | `◔ 42%` | **[✕]** if queued; progress ring if running | "Cancel getting *Title*" |
| Failed | `⚠ Failed` | **[↻ Retry]** | "Retry getting *Title*" |
| In library | `✓` (glyph only in rows; word in headers) | **[+ Episode 42]** when a show playlist is pinned, else none | "Add *Title* to Episode 42" |
| Needs attention | `⚠ Fix` | **[Fix]** (opens the fix for this problem) | "Fix: *route tag missing*" |
| Dropped | `Dropped` | **[Get Back]** | "Get *Title* back" |

- Artist rows: art is round (entity.artist), second line = "Artist · 7 releases · 3 in library". Inline action **[Get 4 Missing]** if any, else none.
- Track rows inside a release show the track number instead of art.
- Click/tap the row body → open the item's page. Double-click a track in a playlist → play (MH4).
- Hover (pointer) reveals `⋯`; it's always visible when focused (keyboard) and on touch rows via long-press.

## C2 · Release tile

```
┌────────────┐
│            │
│   ▓▓▓▓▓▓   │  art.tile, radius.art.sm, shadow.art
│   ▓▓▓▓▓▓   │
│        [⤓] │  ← inline action floats on the art corner (same rule as C1)
└────────────┘
 Title Wraps To Two   BD (2 lines max, then …)
 Artist · EP          C1 fg.secondary
```
Tiles fill a grid with `art.tile` min 150 pt. On compact the grid is 2 columns, or a scrolling shelf (C5).

## C3 · Item menu

Same order everywhere. Items that don't apply are **absent** (not greyed out). The context menu
(right-click / long-press) is the same list.

```
┌───────────────────────────────┐
│ ⤓  Get                     ⌘D │   (or Get Back / Retry / Cancel)
│ +  Add to Episode 42       ⌘E │   (only if a show playlist is pinned)
│ +  Add to Playlist        ›   │ ─┬─ Winter Mix
│ ───────────────────────────── │  ├─ Episode 41
│ ▶  Preview          (later)   │  └─ + New Playlist…
│ ───────────────────────────── │
│    Go to Artist               │
│    Go to Release              │   (on tracks)
│    Show Sources            ⌥⌘I│   (opens inspector: RSS · file · MusicBrainz)
│    Copy Link                  │
│ ───────────────────────────── │
│    Drop                    ⌫  │   destructive style, Undo toast (C7), no confirm
└───────────────────────────────┘
```

## C4 · Page header

```
Expanded
┌──────────────────────────────────────────────────────────────────────────────────────┐
│ ┌────────────┐  RELEASE · ALBUM                                            C1 caps   │
│ │            │  Album Title Can Wrap Onto Two Lines                       T1         │
│ │  ▓▓▓▓▓▓▓▓  │  Artist Name ›                                             HL link    │
│ │  ▓▓▓▓▓▓▓▓  │  2026 · 11 tracks · 44 min · ✓ In library                  SH         │
│ │            │                                                                       │
│ └────────────┘  [ Primary Action ]   [ Secondary ]   ⋯                    HL buttons │
│  art.header     Short description, two lines, then More…                  CO         │
└──────────────────────────────────────────────────────────────────────────────────────┘

Compact: art centred at 60% width, then the same lines centred, then actions full-width.
```
- **Eyebrow** (C1 caps) names the item kind with an `entity.*` dot. Hollow dot = unconfirmed artist.
- Primary action follows the state (C1 table, header wording): *Get Album* · *Getting 4 of 11* · *Add to Playlist* · *Get Back*.
- The state shows **as a word** in the header ("✓ In library", "4 of 11 in library").

## C5 · Section header and shelf

```
 Albums & EPs                                                       See All ›      T2 · HL link
 ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐  ›                          shelf of C2
```
A shelf scrolls horizontally inside itself. Its arrow buttons (pointer) and *See All* (always) mean it never relies on a scroll gesture.

## C6 · Selection mode and action bar

- Enter: **Select** button in the list header, ⌘-click / Shift-click, ⌘A, Space, or long-press (touch).
- Each row shows a checkbox. A click toggles; it doesn't open.
- Exit: **Done** or Esc.

```
┌──────────────────────────────────────────────────────────────────────────────────────┐
│ 7 Selected         [⤓ Get 5]   [+ Add to Playlist ▾]   [Drop 2]          [Done]      │  HL · buttons
└──────────────────────────────────────────────────────────────────────────────────────┘
  bottom of the list (expanded) · above the tab bar (compact)
```
Counts are per action ("Get 5" = 5 of the 7 can be got). Actions with a count of 0 are absent.

## C7 · Toast

```
┌──────────────────────────────────────────────────────┐
│ ✓  Dropped "Album Title". Files kept.      [Undo]    │  CO · HL button
└──────────────────────────────────────────────────────┘
 bottom-centre, above the accessory / tab bar · 6 s · pauses on hover/focus · announced to VoiceOver
```
Used for every action result (ST5). Failures stay until dismissed and include **[Retry]**.

## C8 · Confirm sheet

Only for actions that **can't be undone** or that **go public** (P5).

```
┌────────────────────────────────────────────────────┐
│ Go Live?                                           │  T3
│                                                    │
│ You'll be on air. The app will:                    │  CO
│   1  Use event "Episode 42"                        │  BD
│   2  Attach it to the publisher                    │
│   3  Start the now-playing producer                │
│   4  Connect the stream to radio.example           │
│                                                    │
│                     [ Cancel ]   [ Go Live ]       │  default button = state.live fill
└────────────────────────────────────────────────────┘
 Esc = Cancel · Return = confirm · confirm button names the action (never "OK")
```

## C9 · Inbox item

```
┌──────────────────────────────────────────────────────────────────────────────────────┐
│ ⚠  3 tracks can't receive payments                                 [Fix All]   ›     │  HL · button
│    Their route tag is missing. Episode 42 uses 2 of them.                            │  CO fg.secondary
│    Found by Check Show · 14:02                                                       │  FN
└──────────────────────────────────────────────────────────────────────────────────────┘
```
`›` opens the group (the list of affected items as C1 rows, each with its own Fix).
Kinds: **Blocker** (`⚠ state.danger`), **Needs a decision** (`⚠ state.warn`), **Update available** (`↻ state.info`), **Done** (`✓`, collapses after viewing).

## C10 · Activity popover

```
                                         [⤓ ◔]
┌───────────────────────────────────────────────┐
│ Activity                                      │  T3
│ Getting 3 of 9                     [Stop All] │  HL
│ ▣ Song Name — Artist         ▓▓▓▓░░  62%      │  BD · C1 tabular
│ ▣ Other Song — Artist        queued       ✕   │
│ ───────────────────────────────────────────── │
│ Earlier                                       │  SH semibold
│ ✓ Checked 24 feeds — 2 changed    14:02       │  CO · FN
│ ⚠ 1 download failed          [Retry]          │
└───────────────────────────────────────────────┘
```

## C11 · Checklist row

```
│ ✓  Audio player          mpv 0.38 found                               [Change]        │  BD · SH · button
│ ⚠  Stream encoder        butt isn't set up                             [Set Up]       │
│ ◔  Publisher             Checking…                                                    │
│ ○  Live event            Not started                                                  │
```
Status glyph + word in the detail line (AX2). One action per row, named for what it does.

## C12 · Form row with Test

```
 Publisher host                                                                      HL
 ┌───────────────────────────────────────────────┐  ( This computer ) ( Remote… )   segmented
 │ studio.local                                  │                                  BD field
 └───────────────────────────────────────────────┘
 The computer running musicindex-live-publisher.                                     FN help
 [ Test ]   ✓ Reached studio.local · publisher 0.11 active                           button · FN result
```
Labels above fields (works at every width). Help is always visible, never only a tooltip. The Test result shows inline.

## C13 · Stage bar

```
  ● Prepare ─────── ● Check ─────── ○ Live ─────── ○ Wrap Up          [ Primary For Stage ]
     done             current                                          HL button
```
Stages are text + glyph (AX2). Past stages can be revisited; future ones can't be skipped.
Compact: a single line "Stage 2 of 4 · Check" (SH) above the page title.

## C14 · Search field and suggestions

```
[⌕ new pl                                       ⌘K]
┌──────────────────────────────────────────────────┐
│ Actions                                          │  SH semibold
│   +  New Playlist                          ⌘N    │  BD · FN
│   ✓  Check Show                            ⇧⌘C   │
│ Music                                            │
│   ▣  New Plague — Some Artist · Single           │  C1 compact row
│   ▣  Newport Planet — Other · Album              │
│ ⏎ Search "new pl" in music                       │  FN
└──────────────────────────────────────────────────┘
```
⌘K and ⌘F both focus the field. Starting with `>` shows actions only. Every menu command and
shortcut is listed here, which is how Linux and web (no menu bar) get full discoverability (ST7).

## C15 · Empty state

```
                    ⊘ (role icon, 48 pt)
              No playlists yet                                                       T3
   Playlists are how you plan a show. Add tracks from anywhere.                      CO
                  [ New Playlist ]                                                   primary
```

## C16 · Show accessory

```
Expanded:  │ ▣ ▶  Song Title — Artist      ● LIVE  00:42:10   ⏮ ❚❚ ⏭   [Open Show] │
Compact:   │ ▣  Song Title          ● LIVE   ❚❚   │   (tap anywhere → Show)
```
Shown only while a show is active. The `● LIVE` badge uses `state.live` + word; "Playing" (not live) uses `fg.secondary`.

## C17 · Transport

```
   ⏮        ❚❚        ⏭        ■
 Previous  Pause     Next     Stop        ← labels visible on Show, tooltips in the accessory
 1:12 ━━━━━━━●──────────── -2:30           C1 tabular · scrubber (seek)
```
Buttons that can't act are **absent** (not just disabled): no Previous on the first track, no transport when nothing is loaded.
