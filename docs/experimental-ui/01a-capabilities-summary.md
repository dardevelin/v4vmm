# 01a · Capabilities (summary)

One line per capability. Detail in [01-capabilities.md](01-capabilities.md).

✅ works · 🟡 works but hard to reach / inconsistent · 🤖 automatic, unseen · ⌨️ CLI only · 🔧 built, no button · 📋 planned · ❌ missing

## D · Discover
- D1 Browse newest releases ✅
- D2 Search releases, artists, tracks (Index + Library) ✅
- D3 Browse all releases / artists A–Z 🟡
- D4 Artist page: releases by kind and role, related artists 🟡
- D5 Name-only artist page 🟡
- D6 Group an artist's singles ✅
- D7 Save and re-run a search ✅
- D8 Audition a track 📋
- D9 Browse other curators' playlists 📋

## C · Collect
- C1 Get a release ✅
- C2 Get a track 🟡
- C3 Get many at once ✅
- C4 Download queue with progress, stop queued ✅
- C5 Convert WAV→FLAC, retry / redownload 🟡
- C6 Add a release from a pasted RSS URL ❌
- C7 Import audio files I already have ❌

## L · Library
- L1 Drop from library (file kept) ✅
- L2 List and delete dropped files 🟡
- L3 Undo a drop 🔧
- L4 Library lists: releases, artists, tracks ✅
- L5 Repair file paths after a move 🤖 ⌨️

## M · Freshness & metadata
- M1 Check all feeds for upstream changes ✅
- M2 Apply upstream changes (doesn't add new tracks) 🟡
- M3 Check a feed when its page opens 🤖
- M4 Detect files whose tags are out of date 🤖
- M5 Write updated tags to files ✅
- M6 Compare tags (RSS vs file vs MusicBrainz), edit, write 🟡
- M7 MusicBrainz lookup, track or album 🟡

## R · Show readiness
- R1 Readiness report per track ✅
- R2 Fix one track's payment route ✅
- R3 Fix all payment routes ⌨️
- R4 Confirm a track removed from its feed ✅
- R5 Playlist RSS check ✅ 🤖
- R6 Pre-show check that depends on the show mode 📋

## P · Playlists
- P1 Create, rename, delete ✅
- P2 Add tracks / releases (gets missing ones) 🟡
- P3 Reorder ✅
- P4 Remove from one / all playlists ✅
- P5 Playlist description 🔧
- P6 Import / export 📋

## B · Playback
- B1 Play a playlist from a position 🟡
- B2 Pause, resume, next, previous ✅
- B3 Auto-advance, skip unplayable 🤖
- B4 Stop 🔧
- B5 Seek, play a single track 🔧
- B6 Real audio via mpv (default is silent) 🟡
- B7 Cue next / reorder from Show 📋

## V · Live
- V1 Create a live event 🟡
- V2 Select / check an event ✅
- V3 Forget an event ⌨️
- V4 Attach / detach event to publisher 🟡
- V5 Start / stop / reset publisher and producer ✅
- V6 Service logs ✅
- V7 Connect / disconnect stream (butt) 🟡
- V8 Start / stop recording 🔧
- V9 Copy liveValue feed tag (has a bug) 🟡
- V10 Live now-playing metadata 🤖
- V11 What listeners see, listener count ✅
- V12 Reserve a durable event 📋
- V13 Talk breaks / segments 📋
- V14 Package the episode after the show 📋

## S · Setup & app
- S1 Create config, database, music folder on first run 🤖
- S2 Theme and UI scale ✅
- S3 MusicIndex endpoint (no connection test) 🟡
- S4 Change music folder 🟡
- S5 Player, publisher, encoder settings (raw config only) 🟡
- S6 Test converter and paths ✅
- S7 Dependency status, retry failed actions 🟡
- S8 Database check, backup, restore, repair ✅
- S9 End session / recovery screen ✅
- S10 Settings presets 📋
- S11 Command palette ❌
- S12 CLI JSON tools ⌨️
