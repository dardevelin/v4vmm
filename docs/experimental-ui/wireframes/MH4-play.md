# MH4 · Play the show (built-in player)

Journey: [MH4](../03a-must-have-journeys.md#mh4--play-the-show) · Principles P5 ·
Components: [C1](01-components.md#c1--item-row) [C13](01-components.md#c13--stage-bar)
[C16](01-components.md#c16--show-accessory) [C17](01-components.md#c17--transport)

**Target:** 1 step to start. **Today:** 3 (hover a playlist row); no stop; read-only cuelist; the last track ends in an error.

| Screen | Route | Journey step |
|---|---|---|
| [4.1 Show › Prepare](#41-show--prepare) | `/show` | Pick what to play |
| [4.2 Show › Playing](#42-show--playing) | `/show` | Steer the music |
| [4.3 Show accessory](#43-show-accessory) | shell | Control from anywhere |
| [4.4 End of playlist](#44-end-of-playlist) | `/show` | Finish cleanly |

Show stages (from [03 · Show stages](../03-journeys.md#show-stages-applies-to-j14j17)):
**Prepare → Check → Live → Wrap Up**. Playing without going live ("rehearse") is allowed from Prepare or Check.

---

## 4.1 Show › Prepare

### Expanded
```
┌──────────────────────────────────────────────────────────────────────────────────────────────┐
│ ●●●  ◧  ‹ ›   Show                             [⌕ Search music and actions   ⌘K]  [⤓]  [◨]   │
├───────────────────┬──────────────────────────────────────────────────────────────────────────┤
│ (sidebar)         │ Show                                                                     │ LT
│                   │ ● Prepare ─────── ○ Check ─────── ○ Live ─────── ○ Wrap Up               │ C13
│                   │                                                                          │
│                   │ Playlist                                                                 │ T2
│                   │ ╭──────╮  ★ Episode 42                                       Change ▾    │ HL · menu
│                   │ │ ▓▓▓▓ │  14 tracks · 58 min · ⚠ 2 need attention                        │ SH
│                   │ ╰──────╯                                                                 │
│                   │ How you're running it                                                    │ T2
│                   │ ( ◉ Built-in Player )  ( ○ Mixxx )                        Set Up ›       │ segmented · link (MH6)
│                   │                                                                          │
│                   │                                                                          │
│                   │            [ ✓ Check Show ]        [ ▶ Play Without Going Live ]         │ primary · secondary
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘
```
- Opens with the **show playlist** already picked. *Change ▾* lists all playlists.
- The mode decides what Check and Live do (MH6). Mixxx mode hides playlist and transport and goes straight to Check.
- **First visit with nothing set up:** the body is the MH6 mode picker instead (see [MH6 6.3](MH6-setup.md#63-show-setup-pick-a-mode)).

### Compact
```
┌────────────────────────────────────┐
│                   Inbox 3     ⋯    │
│ Stage 1 of 4 · Prepare             │ SH
│ Show                               │ LT
│ ╭────╮ ★ Episode 42    Change ›    │ HL
│ ╰────╯ 14 tracks · ⚠ 2             │ SH
│ Built-in Player · Mixxx            │ segmented
│ ┌────────────────────────────────┐ │
│ │         ✓ Check Show           │ │
│ └────────────────────────────────┘ │
│     ▶ Play Without Going Live      │ secondary (text button)
├────────────────────────────────────┤
│ Discover  Library  Show  Search    │
└────────────────────────────────────┘
```

**HIG:** ST3 ✓ (Check Show primary; playing is secondary) · TY6 ✓ · AX2 ✓ (stage = glyph + word)

---

## 4.2 Show › Playing

### Expanded
```
┌───────────────────┬──────────────────────────────────────────────────────────────────────────┐
│ (sidebar)         │ Show                                                  Playing · not live │ LT · SH
│                   │ ● Prepare ─── ● Check ─── ○ Live ─── ○ Wrap Up        [ ● Go Live… ]     │ C13 · primary
│                   │                                                                          │
│                   │ ┌──────────────┐   Now Playing                       Up Next       Edit  │ T2 · T2
│                   │ │              │   Bloodshot Lies                    ≡ 3 Old Song     ⚠  │ T1 · C1
│                   │ │   ▓▓▓▓▓▓▓▓   │   The Doerfels ›                    ≡ 4 Sour Sorrow     │ HL
│                   │ │   ▓▓▓▓▓▓▓▓   │   Bloodshot Lies · Single           ≡ 5 Pie             │ SH
│                   │ │              │                                     ≡ 6 …               │
│                   │ └──────────────┘   1:12 ━━━━━━●──────── -2:24        ─────────────       │ C17 tabular
│                   │                                                     Played               │ T2
│                   │      ⏮       ❚❚       ⏭       ■                     1 Bad Little Seed    │
│                   │   Previous  Pause    Next    Stop                   21:04  ✓             │ C17 · FN
│                   │                                                                          │
│                   │ Listeners hear: "Bloodshot Lies — The Doerfels"  (not live)              │ FN
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘
```
- **Up Next is interactive:** double-click = play now · drag `≡` = reorder · `⋯` → *Play Next* ·
  *Remove from Show* · standard C3 items. Edits change the playlist itself, with a toast + Undo.
- **Stop** ends playback and returns the stage to Prepare (Stop exists in code today; it just has no button).
- **Seek** by dragging the scrubber or ←/→ (5 s) when it's focused.
- The "Listeners hear" line previews the now-playing metadata (P5: what goes out is visible).
- A ⚠ track in Up Next shows its problem; a track that can't play is skipped, with a toast saying why.

### Compact
```
┌────────────────────────────────────┐
│ Stage 2 of 4 · Playing (not live)  │ SH
│          ┌──────────────┐          │
│          │   ▓▓▓▓▓▓▓▓   │          │
│          └──────────────┘          │
│          Bloodshot Lies            │ T1
│          The Doerfels              │ HL
│ 1:12 ━━━━━●─────────────── -2:24   │ C17
│      ⏮      ❚❚      ⏭      ■       │ 44 pt targets
│ ┌────────────────────────────────┐ │
│ │          ● Go Live…            │ │ primary (state.live)
│ └────────────────────────────────┘ │
│ Up Next                     Edit   │ T2
│ 3 Old Song                  ⚠      │ C1
│ 4 Sour Sorrow                      │
├────────────────────────────────────┤
│ Discover  Library  Show  Search    │
└────────────────────────────────────┘
```

**Keyboard:** Space play/pause · ⌘→ next · ⌘← previous · ⌘. stop. Available from any section and listed in ⌘K.
When nothing is playing these commands are **absent** from ⌘K and the shortcuts do nothing (today they raise errors).

**HIG:** TY5 ✓ · LA3 ✓ · ST3 ✓ (Go Live… is the only filled button) · ST7 ✓ · AX1 (scrubber "Playback position, 1 minute 12 of 3 minutes 36") · AX4 ✓ (no animated art when Reduce Motion is on)

---

## 4.3 Show accessory

```
Expanded (bottom of every section while playing):
│ ▣ ▶  Bloodshot Lies — The Doerfels        Playing   1:12 / 3:36   ⏮ ❚❚ ⏭   [Open Show] │
Compact (above the tab bar):
│ ▣  Bloodshot Lies        Playing  ❚❚  │
```
It appears when playback starts and disappears on Stop. You never have to visit Show to pause.

---

## 4.4 End of playlist

```
│ Now Playing                                                                  │ T2
│ That's the end of Episode 42                                                 │ T1
│ 14 tracks played · 58:12                                                     │ SH
│        [ Wrap Up ]        [ Play Again from Top ]                            │ primary · secondary
```
The last track finishing is a **normal finish**, not an error. If the show is live, MH5 takes over
and offers **End Show…** instead.
