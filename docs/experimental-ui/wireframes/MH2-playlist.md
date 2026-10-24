# MH2 · Build a show playlist

Journey: [MH2](../03a-must-have-journeys.md#mh2--build-a-show-playlist) · Principles P1 P2 P4 ·
Components: [C1](01-components.md#c1--item-row) [C3](01-components.md#c3--item-menu)
[C4](01-components.md#c4--page-header) [C6](01-components.md#c6--selection-mode-and-action-bar)
[C7](01-components.md#c7--toast) [C15](01-components.md#c15--empty-state)

**Target:** 3 steps to create, 1 per add. **Today:** 6–9 per add path.

| Screen | Route | Journey step |
|---|---|---|
| [2.1 New playlist](#21-new-playlist) | sidebar / sheet | ⌘N → name → Return |
| [2.2 Playlist page](#22-playlist-page) | `/playlists/:id` | See, order, check, play |
| [2.3 Adding from anywhere](#23-adding-from-anywhere) | any row | + Add to Episode 42 |
| [2.4 Empty playlist](#24-empty-playlist) | `/playlists/:id` | First add |

---

## 2.1 New playlist

### Expanded: inline in the sidebar
```
│ Playlists          │
│   ★ Episode 41     │
│   Winter Mix       │
│ ┌────────────────┐ │
│ │ Episode 42█    │ │  ← ⌘N (or "+ New Playlist") puts a field here, focused
│ └────────────────┘ │     Return = create · Esc = cancel
│ ☑ Make it the show playlist                    ← checked by default (FN)
```
Return creates it, opens its page (2.2) and pins it as the **show playlist** (★) if the box is checked.
Only one show playlist at a time; pinning another moves the ★.

### Compact: sheet
```
┌────────────────────────────────────┐
│ Cancel     New Playlist     Create │ T3 · HL buttons (Create disabled until named)
│ ┌────────────────────────────────┐ │
│ │ Episode 42                     │ │ BD
│ └────────────────────────────────┘ │
│ Show playlist                 [◉]  │ BD · switch
│ Every track gets a one-tap add     │ FN
│ button while this is on.           │
└────────────────────────────────────┘
```
**Steps:** ⌘N · type · Return = **3**. **Today:** Return does nothing; 4 different wordings.

---

## 2.2 Playlist page

### Expanded
```
┌──────────────────────────────────────────────────────────────────────────────────────────────┐
│ ●●●  ◧  ‹ ›   Episode 42                       [⌕ Search music and actions   ⌘K]  [⤓]  [◨]   │
├───────────────────┬──────────────────────────────────────────────────────────────────────────┤
│ (sidebar)         │ ╭────────────╮  ★ SHOW PLAYLIST                                          │ C1 caps
│                   │ │  ▓▓   ▓▓   │  Episode 42                                       Rename  │ T1 · link
│                   │ │  ▓▓   ▓▓   │  14 tracks · 58 min · 12 ready · ⚠ 2 need attention       │ SH
│                   │ ╰────────────╯  [ ▶ Play ]   [ ✓ Check Show ]   ⋯                        │ primary · secondary
│                   │                 Add a note for this episode…                             │ CO placeholder
│                   │                                                                          │
│                   │  #   Title                    Artist             Time                    │ FN
│                   │ ≡ 1  Bad Little Seed          Survival Guide     4:09    ✓         ⋯     │ C1 + handle
│                   │ ≡ 2  Bloodshot Lies           The Doerfels       3:36    ◔ 40%     ⋯     │ getting
│                   │ ≡ 3  Old Song                 Someone            2:58  ⚠ [Fix]     ⋯     │
│                   │ ≡ 4  Sour Sorrow              Survival Guide     4:24    ✓         ⋯     │
│                   │ …                                                                        │
│                   │                                       Total 58:12 · ends at ≈ 21:58      │ FN tabular
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘
```
- **Primary is Play** (MH4); **Check Show** (MH3) is secondary. `⋯`: Unpin Show Playlist, Duplicate, Export (later), Delete Playlist….
- Reorder: drag `≡`, ⌥↑ / ⌥↓, or select rows → **Move to Top / Bottom**. Double-click a row plays from there.
- Track `⋯` adds **Remove from Playlist** (no confirm; toast with Undo) above the standard C3 items.
- The note field uses the playlist description that already exists in code but has no UI (P5 in [01a](../01a-capabilities-summary.md)).
- **Inspector** (◨) shows the **Up Next / readiness** summary for this playlist.

### Compact
```
┌────────────────────────────────────┐
│ ‹ Library             Edit    ⋯    │ Edit = reorder / delete mode
│ ★ SHOW PLAYLIST                    │ C1
│ Episode 42                         │ LT
│ 14 tracks · 58 min · ⚠ 2           │ SH
│ ┌──────────────┐ ┌───────────────┐ │
│ │   ▶ Play     │ │ ✓ Check Show  │ │
│ └──────────────┘ └───────────────┘ │
│ 1 Bad Little Seed       4:09  ✓    │ C1
│   Survival Guide                   │
│ 2 Bloodshot Lies        3:36  ◔    │
│   The Doerfels                     │
│ 3 Old Song          ⚠ [Fix]        │
├────────────────────────────────────┤
│ ▶ Bad Little Seed     Playing  ›   │ accessory (if playing)
├────────────────────────────────────┤
│ Discover  Library  Show  Search    │
└────────────────────────────────────┘
```

**HIG:** TY5 ✓ (times, total, end time) · LA3 ✓ (handles 44 pt on touch) · ST3 ✓ · ST4 ✓ (Delete Playlist… confirms; remove-from-playlist is undoable) · ST7 ✓ (⌥↑/⌥↓, ⌫) · AX1 (handle = "Reorder Old Song") · AX2 ✓

---

## 2.3 Adding from anywhere

With a show playlist pinned, **every owned item's inline action** becomes `[+ Episode 42]`
(C1 table). Not-owned items keep `[⤓ Get]`, and their menu offers **Get and Add to Episode 42**.

```
Release page row      │ 3  Pie                 4:07   [+ Ep 42]   ⋯ │
Search track result   │ ▣ 3 Years   The Doerfels   3:32   [+ Ep 42] ⋯ │
Not owned             │ ▣ Bloodshot Lies  …  3:36   [⤓ Get]   ⋯    │ ⋯ → "Get and Add to Episode 42 ⌘E"
Already in it         │ ▣ Sour Sorrow …      4:24   ★ In Ep 42  ⋯  │ C1 caption, no button
```
Result: **toast** *"Added 'Pie' to Episode 42."* **[Undo]** (C7), and the sidebar count updates in place.

Other ways (all produce the same toast):
- `⋯` → Add to Playlist › *any playlist* / New Playlist…
- Drag rows onto a sidebar playlist (hold over "Playlists" to expand it).
- Select → C6 bar → **+ Add to Playlist ▾**.
- ⌘E = add the focused/selected items to the show playlist.

**Steps:** **1** per item from anywhere. **Today:** 6–9 (and impossible from an Index track page).

---

## 2.4 Empty playlist

```
│ ★ SHOW PLAYLIST                                                              │
│ Episode 42                                                                   │ T1
│ No tracks yet                                                                │ SH
│                                                                              │
│                         ⊘                                                    │
│                 Your playlist is empty                                       │ T3
│  Every track now has a "+ Episode 42" button. Start in Discover or Search.   │ CO
│              [ Go to Discover ]    [ Search ]                                │ primary · secondary
```
**HIG:** ST8 ✓ (empty state teaches the next action, P7).
