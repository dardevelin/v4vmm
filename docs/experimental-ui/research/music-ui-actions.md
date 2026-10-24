# Research: Music section — current UI actions (v4vmm @ 38ecd9d)

Raw survey by a read-only code agent. Paths relative to `v4vmm/`.

## 0. Where docs/architecture/music-flow-map.md is now wrong
- `IndexNameMatches`/`ArtistDetail` gone → name opens `ArtistName` (name-level page) or the feed page if exactly one Library artist feed matches (`src/view_models/workspace/nav.rs:106-144`).
- "+Playlist" label gone. "Delete playlist" now confirms ("Delete…").
- **Library tracks now download when added to a playlist** (`SubscribeThenAppendToPlaylist`, `src/library/app_impl.rs:1978-2120`); these downloads bypass the download queue → **no progress shown**.
- Search tabs are Releases / Artists / Tracks (no All/Library/Index chips).
- Selection mode exists on Discover, search, playlist, feed, name pages.
- Index track page still has no "Add to playlist"/"Download track".

## 1. Layout
- Opens on Discover › New releases. Tab "Music", `cmd/ctrl-1`.
- Three columns: sidebar / list / detail. Empty detail: "Select an item in the list."
- Discover's list column lists *views*; "New releases" renders in the *detail* column.
- Download progress strip above workspace on every section.

## 2. Sidebar
- Collapse/expand. Destinations: Discover, Releases, Artists, Tracks, Needs attention (count + mark), Playlists (count). Click = new path, clears selection.
- Spring-load: drag a track over "Playlists" ~500 ms opens it.
- Status line (feedback for adds/downloads/errors; expanded only).
- "Check all feeds" ↔ "Apply updates (n)" (+ "Update available"). "Update n files" (opens tag dialog). Tag report text after write. Feed status messages.
- **Collapsed sidebar hides feed + tag controls.**

## 3. List-column header
- Scope "Library"/"Everything" (Releases, Artists only; resets to Library at each start). "Everything" pages MusicIndex names with "Load more"/"Try again".
- View mode "Tiles"/"List" (Tracks only; saved).
- "Select"/"Done" (Releases, Tracks, Needs attention).

## 4. Destinations
### Discover (list column)
- "New releases" ("Newest first, from the Index"); "Saved: {query}" rows ("Library and Index"). No menu/rename.
### Discover › New releases (detail column)
- Tiles/List; click → release page (Library if known, else Index); selection (Ctrl/Shift/checkbox/Space/Ctrl+A, Select/Done); bar "Download n releases", "Add n releases to playlist" (Library only), "Remove n releases…", "Clear"; infinite scroll + "Load more".
- **No row menu, no download slot, no drag.**
### Releases
- Click → release page. Singles group row: body → "Singles by {artist}" page; chevron expand/collapse ("Show singles"); group checkbox selects all singles.
- Row menu (hover ⋯ "More actions", focus, right-click): "Download {album/EP/single}", "Add to playlist" (submenu), "Remove {kind}…". Group row has no menu. Hidden in selection mode.
- Bar: Download n releases / Add n releases to playlist / Remove n releases… / Clear.
### Artists
- Feed row → feed page; "{name} · by name" → name-level page; Index feed (Everything) → feed page.
- **No menu, no selection, no keyboard row focus.**
### Tracks
- Library track row → track page. Menu: Add to playlist, Download, Inspect sources, Remove…. Drag to playlist. Bar: Add n tracks to playlist, Remove n tracks…, Clear (download count hard-wired 0).
- **Dead end: not-downloaded rows are listed but click does nothing, no menu, no checkbox, no drag.**
### Needs attention
- Fix button: "Fix routes" (NoRouteTag), "Confirm" (RemovedFromFeed). **No fix for NoRoutesUpstream, NotDownloaded, FileMissing, RouteOutOfDate.**
- Menu: "Inspect sources", "Remove from Library…" (RemovedFromFeed only).
- **Row body click does nothing**; only route to track page is "Inspect sources".
- Bar offers "Add n tracks to playlist" but row menu doesn't.
### Playlists (list column)
- Sort "A–Z"/"Recent"/"Size". "+" toggles inline "New playlist name…" + "Add" — **Enter does not create**. Row click → playlist page; row is drop target. No row menu, no selection. `cmd-n` opens with field focused.

## 5. Search
- `cmd-f`/`cmd-alt-f`; placeholder "Search Library and Index"; Enter or "Search" → Music, results in list column, detail clears.
- Tabs Releases/Artists/Tracks (auto-jump to first non-empty).
- Release row → release page; Track → track page; Artist → feed page or name-level page.
- Select/Done on Releases and Tracks tabs; bar Download / Add (Library only) / Remove (Library only) / Clear.
- "Save search" → "Saved". Failure: "Show details", "Copy report".
- **No row menu, no download slot, no drag; Artists tab no selection; Index track can't go to a playlist from search.**

## 6. Release page (one shell, both origins)
| Origin/state | Primary | ⋯ menu |
|---|---|---|
| Index | "Download {kind}" | Copy feed URL |
| Library, none downloaded | "Download {kind}" | Copy feed URL, MusicBrainz lookup |
| Library, queued/downloading | "Download {kind}" disabled | same |
| Library, ≥1 downloaded | "Add to playlist" | Copy feed URL, MusicBrainz lookup, "Remove {kind}…" |
- Copy feed URL is silent. Library album open auto-runs staleness check.
- **Mismatch**: partly-downloaded album page has no "Download album" (row menu does); not-downloaded album page has no "Add to playlist" (row menu does).
- Name links (artist → feed/name page; "All n singles"); description toggle; contributors Website/Nostr (Library only).
- Track rows: body → track page; download slot; badges "New"/"Removing" (Library); MusicBrainz status icon (Library); menu Add/Download/Inspect sources/Remove… (Library) or Add/Download (Index); drag (Library); selection.
- Bar: Download n tracks / Add n tracks to playlist (**Library only** → Index page bar never offers Add though row menu does) / Remove n tracks… / Clear.

## 7. Track page
| Origin/state | Filled | Plain | ⋯ |
|---|---|---|---|
| Index | "Download release" | — | Copy feed URL |
| Library, not downloaded | "Download track" | "Add to playlist" | Copy feed URL |
| Library, downloaded | "Add to playlist" | — | Copy feed URL, MusicBrainz lookup, "Remove track…" |
- Index "Copy feed URL" feedback goes to `settings_status` → **invisible in Music**.
- Inspect sources (Library only): "Compare ID3"/"Hide Compare"; staged edits auto-filled; "Apply tags (n)", "Discard staged"; conflicts block; "Re-read", "Re-download"; MusicBrainz panel (opened via menu only, **no hide**); expandable cells.
- **Dead end: Index track page has no Add to playlist / Download track.**

## 8. Other pages
### Singles group
- "Singles by {artist}"; main action "Download n singles" (any missing) else "Add to playlist". Library single rows: shared track row (click → release page; "Inspect sources" → track page). Index single rows: click + checkbox only. "Try again" on error.
- **While any single missing, no "Add to playlist"** except via Select.
### Playlist page
- "Rename" (Enter saves), "Delete…" (dialog), "Check RSS".
- Row: body → track page; hover Play (starts playback + auto RSS check); drag handle reorder / drop on another playlist; menu Add to playlist, Download, Inspect sources, Move up, Move down, "Remove from playlist" (**no confirm**), "Remove from Library…".
- Removed-from-feed notice: "Remove from playlist", "Remove from all playlists" (dialog). RSS report: "Open podping.me", "Copy feed URL", "Download" (added tracks).
- Bar: Remove n from playlist (no confirm), Download n tracks, Add n to playlist, Move to top/bottom, Remove from Library…, Clear.
- Empty: "Empty — add tracks from the library or search".
### Saved-search page
- Same as search; "Delete…" (dialog). No rename.

## 9. Selection mode
- Start: Select, Ctrl/Cmd/Shift+click, checkbox, Space, Ctrl+A. End: Done, Escape. Column acts only with focus.
- Bar: "Select items" or "{n} … selected" + Download / Add / Remove… / Clear (zero-count actions hidden).
- Pages with selection: release, singles group, feed, name, New releases, playlist, saved search, + Releases/Tracks/Needs attention lists, search Releases/Tracks.
- **No selection:** Artists list, Playlists list, track page, search Artists tab.

## 10. Feed-level artist page
- Sections "Albums and EPs", "Singles", "Matched by name", "As {roles}", "Role not confirmed"; rows → release page, release row menu, selection. "Shares albums with" rows → other feed page.
- **No page actions (no download-all), no retry on failure.**

## 11. Name-level page
- "Releases that name {name}", level "Name only"; rows like feed page. Notice "Open the artist page" when a feed now matches. ">1,000" text, no load more. No retry.

## 12. Playlist popover
- Playlist rows or "No playlists yet", "New Playlist" → "Playlist name", "Back", "Create & Add". **Enter has no handler.**

## 13. Confirmations
- Remove track/album/n from Library (dialog; always says "Album" even for single/EP).
- "Update n files?" → "Write Tags" (not destructive style).
- Delete playlist, delete saved search, remove from all playlists.
- **No confirm:** remove from playlist, move, add-to-playlist (which downloads).

## 14. Download progress strip
- "Downloading k of n", current item, "n failed", "Stop queued" (in-flight continues). Finished: "Downloaded d of n…, Finished HH:MM", failures, "Dismiss". Excludes add-to-playlist downloads.

## 15. Navigation & keys
- Back/Forward buttons + location breadcrumb in detail chrome; restore scroll and toolbar query. List row replaces pages under destination; links push deeper.
- Keys: cmd-1/2/3, cmd-f, cmd-n, cmd-r, Esc (ends selection; else clears detail without moving history), arrows/PgUp/PgDn/Home/End, Enter, Space, cmd-a, Right/Left.

## 16. Naming inconsistencies
- "MusicBrainz lookup" = one track (track page) vs whole album (release page).
- Remove: "Remove…", "Remove track…", "Remove from Library…", "Remove {kind}…", "Remove n tracks…"; dialog Title Case.
- Download: Index release "Download album" vs Index track "Download release"; "Download n singles" vs "Download n releases".
- New playlist: "New Playlist", "+"/"Add", "New playlist name…", "Playlist name".
- Enter submits rename but not create.
- "In Library" vs "In library", "Not downloaded", "New".

## 17. Step counts (from Music open, New releases showing)
1. Download a new release from Discover: **2** (row, Download). Bulk: N+1.
2. Search artist, download 3 chosen singles: **7** (cmd-f, type, Enter, 3× Ctrl+click, Download). Via Artists tab: 9. Library singles group "download all": 3.
3. Add 3 tracks of a Library release to "000": **7** (Releases, row, 3× Ctrl+click, Add, 000). Row menus: 11. Drag: 6.
4. Create playlist + add album: **6** (Releases, ⋯, Add to playlist, New Playlist, type, Create & Add).
5. Check feeds, apply, write tags: **4** (+1 if sidebar collapsed).
6. Fix a Needs attention item: **2** (Fix routes/Confirm); NotDownloaded: 4; other states: no fix.
7. Add an Index track from search to a playlist: **9** (cmd-f, type, Enter, Tracks, row, album link, ⋯, Add to playlist, pick); feedback invisible.
