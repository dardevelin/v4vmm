# Research: Backend capability catalog (v4vmm @ 38ecd9d)

Raw survey by a read-only code agent of the command/query/service/runtime layers. Paths relative to `v4vmm/`.
Reach: **GUI** (live UI handler), **CLI**, **AUTO** (no user action), **NONE** (tests only).

Architecture: commands in `src/application/commands/*` (sync, run in background via `AsyncCommandRunner`); results as `ApplicationEvent`/`VmEvent`; long work in `src/runtime/` actors; CLI calls same services synchronously.

## A. Discovery / search
| Capability | Entry | Side effects | Reach |
|---|---|---|---|
| Search Index (releases/artists/tracks) merged with Library state | `FetchIndexSearchResults` `src/application/queries/search.rs:33` | `GET /v1/search` | GUI |
| Search local Library tracks | `search.rs:191` → `db::search_library_tracks` | DB read | GUI |
| Saved searches: save / list / re-run / delete (no rename, no "new results", no schedule) | `src/db/saved_searches.rs`, `src/library/saved_searches.rs` | DB `saved_searches` | GUI |
| New releases (Discover) | `FetchRecentFeedsPage` `queries/feed.rs:26` | `GET /v1/feeds/recent` | GUI |
| "Everything" scope: release names A–Z / artist names A–Z | `queries/name_lists.rs:56/282` | `/v1/feeds/by-name`, `/v1/artist-names` | GUI |
| Singles of one artist feed | `FetchFeedSingles` `name_lists.rs:198` | `/v1/feeds/recent?named_feed_guid=` | GUI |
| Name-level artist page | `FetchNameLevelReleases` `search.rs:74` | `/v1/feeds/by-name?artist_key=` | GUI |
| Index release / track detail | `search.rs:448/761` | `/v1/feeds/{g}`, `/v1/feeds/{f}/tracks/{t}` | GUI |
| Index / Library artist page | `queries/feed.rs:67`, `queries/library.rs:248` | MusicIndex + **DB writes** (relationships) | GUI |
| Store artist feed image | `StorePublisherFeedImage` `commands/feed.rs:671` | DB | GUI |
| Thumbnails, logo mosaic, cover colour | `queries/images.rs` | HTTP + disk cache | GUI |
| Tracks by artist | `api.rs:1140` | — | NONE |
| RSS podroll | `rss/enrich.rs:202` | — | NONE |

## B. Acquisition / download
| Capability | Entry | Side effects | Reach |
|---|---|---|---|
| Download a track | `SubscribeTrack` `commands/download.rs:108` → `subscribe_service.rs:90` | MusicIndex GET, RSS fetch+parse (upserts feed + **all** items), enclosure GET; writes `<music_dir>/artists/<artist>/<release>/<file>`; WAV→FLAC via flac/ffmpeg; writes tags from stored values; `is_in_library=1` | GUI |
| Download a release | `SubscribeFeed` `commands/feed.rs:541` | same per track | GUI |
| Download-then-append to playlist | `SubscribeThenAppendToPlaylist` `commands/download.rs:241` | download + playlist write | GUI |
| Download queue (max 3 in flight, 1 per release; in-memory; running jobs not cancellable) | `src/library/download_queue.rs`, `src/library/downloads.rs` | — | GUI |
| Conversion recovery: retry / redownload / discard (session-scoped) | `commands/download.rs:149/178`, `application/conversion_recovery.rs` | flac/ffmpeg, files | GUI |

**Not possible:** add a feed by pasted RSS URL; import/scan existing local audio. Acquisition always starts from a MusicIndex record.

## C. Library membership & local files
| Capability | Entry | Side effects | Reach |
|---|---|---|---|
| Plan a removal (playlist impact) | `application/library_removal.rs:116` | read | GUI |
| Remove track/release from Library (file kept → "cached") | `RemoveFromLibrary` `commands/library_removal.rs:44` | DB only | GUI |
| Remove track from all playlists | `commands/playlist.rs:424` | DB | GUI |
| List cached files | `queries/library.rs:91` | read | GUI (Settings) |
| Delete cached files | `RemoveCachedFiles` `commands/download.rs:502` | **deletes audio files** | GUI |
| Library tree / paged lists | `queries/library.rs:59`, `paged_track_list.rs` | read | GUI |
| Repair local file paths | `db::repair_local_file_paths` | DB | AUTO startup + CLI |
| Old membership/unsubscribe commands | `commands/download.rs:366/405/448`, `commands/feed.rs:602/633` | — | NONE |

Feed and track rows are never deleted.

## D. Metadata, tags & provenance
| Capability | Entry | Side effects | Reach |
|---|---|---|---|
| Stored-value projection (RSS hold → MusicIndex fact → column) | `queries/stored_values.rs` | read | GUI/AUTO |
| Library track detail with local fallback | `queries/library.rs:119` | MusicIndex + RSS, observation writes | GUI |
| Album identity hydration | `queries/library.rs:160` | **DB writes** | AUTO on album open |
| Three-way compare (file tags vs source vs MusicBrainz) | `CompareLibraryTrack` `queries/library.rs:204` | reads file | GUI |
| Write chosen tag edits to one file | `ApplyTrackId3Edits` `commands/metadata.rs:146` | **writes tags** (MP3 ID3v2.4; FLAC/Ogg/Opus/MP4 via lofty; WAV refused) | GUI |
| Tag update scan (which files differ) | `queries/tag_update.rs:258`, `runtime/tag_update.rs` | reads | AUTO + GUI |
| Tag update write ("Update n files") | `commands/tag_update.rs:80` | **writes tags** incl. payment routes; skips files in use by show | GUI |
| Provider observation/evidence store | `provider_observation.rs`, `metadata_*` tables | DB | AUTO |

Frames written: `TXXX:MusicIndex Feed Guid / Track Guid / Image / Value Routes`, WOAF, WOAR, album description, Nostr key.

## E. MusicBrainz (optional, no fingerprinting)
- Track lookup `LookupMusicBrainzTrack` `commands/metadata.rs:23` — GUI.
- Album lookup saga (release search, per-track stage) — GUI.
- **Results not persisted**; staged edits in memory; durable only via per-track tag write.

## F. Feed freshness & updates
| Capability | Entry | Side effects | Reach |
|---|---|---|---|
| One feed stale? | `CheckFeedStaleness` `commands/feed.rs:75` | MusicIndex GET | AUTO on album open |
| Check all feeds + repair missing route tags | `CheckFeedsAndRepairRoutes` `commands/feed.rs:393` | **writes route tags** | GUI |
| Apply MusicIndex updates (DB only, **doesn't add new tracks**) | `ApplyFeedUpdates` `commands/feed.rs:177` | DB | GUI |
| Playlist RSS check (holds, differences, removed-from-feed, routes) | `CheckPlaylistRss` `commands/playlist.rs:344`, `runtime/playlist_rss_check.rs` | RSS GETs, DB | GUI + AUTO on playback start |
| Confirm removed-from-feed track | `commands/playlist.rs:385` | DB | GUI |

## G. Playlists
- List/create/rename/delete; remove at position; reorder; move many; remove many — GUI.
- Append local tracks directly — NONE (GUI always uses download-then-append).
- Playlist tracks/covers — GUI + CLI.
- Description — column exists, never set (NONE).
- Import/export (musicL) — not built, deferred.

## H. Readiness / attention
- Readiness report per track: Ready, NoRouteTag, NoRoutesUpstream, NotDownloaded, FileMissing, RouteOutOfDate, RemovedFromFeed — `queries/broadcast.rs:181`; GUI (Needs attention, Show) + CLI.
- Repair route tag for one track — GUI row + CLI.
- Repair all — CLI; GUI only inside "Check all feeds".
- Removed-from-feed is advisory; playback doesn't enforce it.

## I. Playback
- Play playlist at position `PlayPlaylistAt` → mpv JSON IPC + **drop file** (outward-facing: drives live payments) + AUTO RSS check — GUI.
- Next/prev/pause/resume — GUI + keys. Auto-advance 1 Hz polling — AUTO.
- **PlayTrack, SeekPlayback, StopPlayback — NONE.**
- CLI playback is DB-only simulation.
- Drivers: Null (default, silent) or Mpv.

## J. Broadcast / live show / recording (chain runs without app)
| Capability | Side effects | Reach |
|---|---|---|
| Create/Replace relay live event | `POST /v1/liveitems`, token file 0600, DB | GUI + CLI |
| Select event | DB | GUI |
| Check event live/dead | relay GET | GUI + CLI |
| Watch what listeners receive | relay polling | AUTO; CLI `liveitem latest` |
| Forget event | deletes row + token | **CLI only** |
| Publisher targets list / attach / detach | `musicindex-live-publisher target …` local/ssh + restart unit | GUI (+CLI list/attach) |
| Start/stop/reset publisher & producer units | `systemctl --user` local/ssh | GUI |
| Service status watch, unit logs | systemctl/journalctl | GUI |
| butt connect/disconnect/status | `butt -s/-d/-S` | GUI |
| butt start/stop recording | `butt -r/-t` | **NONE** |
| Copy `<podcast:liveValue>` tag | clipboard (**defect: bare id, not URL**) | GUI |
| Read remote drop file; daemon-reload | — | NONE |
| Relay health | `GET /health` | CLI |

## K. Configuration, maintenance, recovery
- Config load/validate/test/save (revisioned, backed up) — GUI.
- App settings / layout prefs — GUI.
- DB check / backup / preserve / review restore / restore / upgrade / repair migration 11 — GUI.
- Startup admission + migrations to v21 — AUTO.
- Capability diagnostics + retry of retained actions (BackgroundRuntime, Thumbnails, MusicIndex, Playback, Publisher, Producer, Encoder, Converter, Presentation, LibraryPaths, Configuration) — GUI.

## L. CLI-only surface
`now-playing`, `liveitem health|latest`, `broadcast events list|create|forget|check`, `broadcast targets list|attach`, `broadcast readiness`, `broadcast repair-routes [id]`, `playlists list`, `playlist tracks`, `library tracks`, `library repair-paths`, `player ping`, `track inspect`, `playlist play`, `playback …` (simulated). No search/download/metadata/playlist-mutation in CLI.

## Reachability summary
- **No GUI caller:** forget event, standalone batch route repair, liveitem health/latest, JSON dumps, CLI playback, repair-paths.
- **No caller at all:** PlayTrack, SeekPlayback, StopPlayback, AppendTracksToPlaylist, old membership/unsubscribe commands, playlist description, butt recording start/stop, read_drop_file, daemon_reload, fetch_tracks_by_artist, fetch_feed_podroll.
- **AUTO side effects:** album open → staleness check + identity hydration (network + DB); playback start → RSS check; tag scans after downloads/updates/playback stop; startup path repair; auto-advance; relay/service/readiness watches.
- **Queries that write DB:** HydrateAlbumIdentity, FetchLibraryPublisherPage, FetchIndexPublisherPage.

## Domain objects & lifecycles
- **Release/feed**: Index-only → stored (when any track downloaded; all items upserted) → `is_subscribed` derived (all tracks in Library) → stale (MusicIndex newer) → updated. RSS holds, payment routes, publisher relationships. Never deleted.
- **Track**: Index hit → stored row → queued → downloading → [WAV retained / conversion failed / redownload] → **In Library** (file + tags) → tags stale → updated → not show-ready (NoRouteTag / NoRoutesUpstream / RouteOutOfDate / FileMissing / NotDownloaded / RemovedFromFeed→confirmed) → removed from Library → **cached** (file kept) → cached file deleted (row stays forever). In parallel: "in use by show" blocks tag writes.
- **Artist**: publisher feed GUID named by albums; levels Artist / Unverified artist / Name only. No artist table.
- **Local file**: relative path, size; path repairs logged.
- **Playlist**: unique name, unused description, ordered positions; RSS check runs + differences persist.
- **Saved search**: query, start tab, created_at.
- **Playback session** (single "default"): stopped/playing/paused, track, position.
- **Broadcast event**: created (token) → selected → attached to publisher target → checked live/dead → replaced/forgotten.
- **Session-scoped (lost on restart)**: download queue, conversion recovery, MusicBrainz staged edits, request cache.

## External dependencies
- **MusicIndex** (`api.musicindex.org`): search, Discover, Index pages, downloads, staleness, route fallback. Without it Library/playlists/RSS check still work.
- **Relay** (`/v1/liveitems`, same base URL, not in MusicIndex contract): event create/check/observe.
- **RSS hosts**: download, RSS check, enrichment.
- **mpv**: audio (Null driver simulates). Known mpv IPC error.
- **flac/ffmpeg**: WAV→FLAC only.
- **musicindex-live-publisher** (systemd user unit, local/ssh): attach/detach/service control.
- **mixxx-now-playing**: start/stop/status.
- **butt**: stream connect/disconnect/status.
- **ssh**: remote hosts. **Token files**: unrecoverable if lost.
- **MusicBrainz / Cover Art Archive**: lookup only.
- **SQLite DB + music_dir**: core.

## Half-built / proposed / blocked
- ADR 0068 (Proposed): Show cue, explicit Show transport, audition isolation.
- No GUI stop/seek/single-track play.
- Recording control unwired.
- Episode export (requested, no ADR). Live segments/talk breaks (requested). Reserved durable live events (not callable).
- liveValue tag writes bare id (fix Ready).
- ADR 0069 tasks 002–005 (guarded editor, live metadata setup, presets, audio settings) not started.
- ADR 0084 packets implemented, visual checks open.
- Apostrophe search → HTTP 400 upstream.
- MP4 `TYER` false difference.
- Playlist import (musicL) deferred.
- `value_time_splits` decoded; consumer UNCONFIRMED.
