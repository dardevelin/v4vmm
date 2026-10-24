# 02 · As-is baseline

How the capabilities in [01](01-capabilities.md) are reached **today**. This is a yardstick for
measuring the redesign, not a template for it. Counts are from the code at `38ecd9d`, using the app's
own method: one click/key = 1 step, typing a phrase = 1 step, picking from a menu = 1 step, waiting = 0.

## Step counts today

| # | Job | Start | Steps today | Notes |
|---|---|---|---|---|
| 1 | First launch → hear real audio | fresh install | **not possible without raw config editing** | Default player is silent; no hint |
| 2 | Take a new release from Discover | Music open | **2** | Row → "Download {kind}" |
| 3 | Search an artist, take 3 chosen singles | Music open | **7** | ⌘F, type, Enter, 3× Ctrl+click, Download |
| 4 | Take all missing singles of a known artist | Music open | **3** | Only if it's already in Library |
| 5 | Add an Index track found by search to a playlist | Music open | **9** | Track page has no Add → detour via album page; feedback invisible |
| 6 | Create a playlist and add an album | Music open | **6** | Enter doesn't submit the name |
| 7 | Add 3 tracks of a release to a playlist | Music open | **7** | 6 with Shift+click, 11 with per-row menus |
| 8 | Check feeds → apply → write tags | Music open | **4** | +1 if sidebar collapsed |
| 9 | Fix a "Needs attention" item | Music open | **2** / **4** / **no fix** | Depends on problem type |
| 10 | Drop a release and free its disk space | Music open | **~8** | Remove in Music, then Settings › Diagnostics › Cached files › Delete (no confirm) |
| 11 | Play a playlist | app open | **3** (+1 to see Show) | Hover a row → play; no "play from top" |
| 12 | Go live (no event yet, music playing) | app open | **~10** + edit RSS outside app | No preflight, no confirmation |
| 13 | Go live (event already attached) | app open | **6** | |
| 14 | Cue a different next track | Show open | **3–5** | Must go back to Music to reorder |
| 15 | End the show | Show open | **6** and it never fully ends | No Stop; session stays Paused |
| 16 | Configure a stream encoder | anywhere | **~6** + raw TOML | Discovered via "Repair encoder" → notice → raw field |

## Cross-cutting friction (the real problems)

These matter more than any single step count, because they hit every journey.

1. **F1 · Same item, different powers depending on where you found it.** An Index track page can't be
   added to a playlist, but its row on the album page can. Discover rows have no menu, Releases rows do.
   Needs attention rows can't be clicked. Tracks-list rows that aren't downloaded do nothing at all.
   *The user can't build a reliable mental model.*
2. **F2 · One verb, many words.** Remove has 5 labels, Download has 4, "new playlist" has 4,
   "MusicBrainz lookup" means two different scopes.
3. **F3 · Feedback lands where the user isn't.** Many results and errors go to a status line that
   renders only in Settings or Show. The sidebar status line hides when collapsed.
4. **F4 · Problems are scattered across five places.** Needs attention (sidebar), "Apply updates (n)"
   (sidebar footer), "Update n files" (sidebar footer), capability notice (top banner), Background
   tools (Settings › Diagnostics). Plus cached files in Settings.
5. **F5 · Show has no shape.** It's a dashboard of independent switches (event, attach, producer,
   publisher, stream) with no order, no preflight, no confirmation for going on air, and no way to end.
6. **F6 · Setup is a raw config editor.** The settings a show needs are TOML keys
   (`broadcast.hosts`, `playback.driver`). No onboarding; the default player is silent.
7. **F7 · Hidden automation.** Feed checks, RSS checks and tag scans run on their own, and the user only
   learns about them when a button label changes.
8. **F8 · Low discoverability on Linux.** No menu bar; shortcuts exist but nothing lists them;
   right-click and hover menus vary by row type.
9. **F9 · Built features with no button.** Stop, seek, single-track play, recording, forget event.
