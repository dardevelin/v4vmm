# Research: Show section — current UI actions (v4vmm @ 38ecd9d)

Raw survey by a read-only code agent. Paths relative to `v4vmm/`.
VM = `src/view_models/show.rs`, Shell = `src/ui/shells/show.rs`, Panel = `src/ui/composites/show_detail_panel.rs`, App = `src/app/show.rs`.

## Corrections to docs/architecture/screen-inventory/show.md (2026-10-03)
- "Reset" runs `systemctl reset-failed` (`src/broadcast/control.rs:277`) → unit goes Inactive; user must then press Start.
- Event "More" menu holds only: "Copy feed tag", "Check again", "Detach" (VM:5840-5865). Create/Replace/Check/Attach/Retry config/Retry events are primary-button only.
- Event panel renders no target line; target/registration/check/summary/diagnostics are projected (VM:2306-2370) but only visible via the log pane.
- Encoder recording start/stop exists (`src/broadcast/encoder.rs:296-363`) with **no caller**. Recording is display-only.

## 0. Reaching Show
- Toolbar tab "Show" (`src/view_models/app_toolbar.rs:74`), shortcut `cmd/ctrl-2`, "Open Show" button in the Live Status Strip (only on Music/Settings, only while a session is active). App always launches on Music. No menu-bar item for Show/playback.

## 1. Dashboard
- Summary header (display only): Active = title/artist/duration/"Playing"|"Paused"; Idle = "No active show"; runtime down = "Show status unavailable … Use Check again in Background tools." (**no link** — dead end).
- Status line renders `settings_status` in danger red — including *success* text like "Playing …" after a playback command (`src/app/playback_bar.rs:213`).
- Cards (order Source, Live Metadata, Stream; 1–3 column grid):
  - Source: reachability badge, host, readiness count. **Disappears silently if `broadcast.hosts`/`selected_host` invalid.**
  - Live Metadata: "Ready"/"Not ready".
  - Stream: connection label (duplicated in body), listeners.
  - Setup failures overlay "Setup needs attention" on the mapped card.
- Click a card → opens side panel on that card; click again closes. No keyboard path found.

## 2. Side panel chrome
- Defaults to Cuelist. Closed rail: chevron "Open show side panel". Header: "Cuelist" back button (detail mode), title, close chevron.

## 3. Cuelist (`src/ui/shells/queue_now_playing.rs:97-220`)
- Display only; rows not clickable. Shows session playlist, now-playing marked. Empty: "Queue is empty".
- Text filter capability exists but is never set. **No cue / reorder / remove / play-from-here.** (ADR 0068 Proposed.)

## 4. Source detail
- Rows Host / Reachability / Readiness.
- "Open" (a11y "Open broadcast readiness issues in Music") — enabled only when "N tracks not ready"; navigates to Music › Needs attention. Label mismatch ("Open"/"N tracks not ready" vs "Needs attention").

## 5. Live Metadata detail
### Event block
- Badge (≈19 states: Loading, No event, Dead, Attached, Not attached, Target not set, …).
- Event picker: selecting writes DB selection and **auto-runs Check**; no confirmation; does not change publisher target.
- Primary button is one of: Create / Replace / Check / Retry check / Attach / Retry config / Retry events (+ progress labels).
  - **Create**: only when the event registry is empty → registers relay event with MusicIndex, writes token file under `<config>/broadcast/tokens`, selects, auto-checks. **Once any event exists (even Dead), Create disappears**; new events need Replace (Dead only) or CLI `v4vmm broadcast events create`. **"Forget" has no UI.**
  - **Attach**: needs event Live + publisher reachable + targets loaded & not attached + drop target set → `target add --replace` + restarts publisher.
- "Logs" toggles log pane with event report.
- "More": Copy feed tag (writes `<podcast:liveValue uri=… protocol="socket.io"/>` to clipboard, **no feedback**; user must paste into their RSS feed externally), Check again, Detach (**no confirmation**, marked non-destructive, restarts publisher). With no event: menu shows only "No actions".
- Hints: "Token file is missing locally; copy it to the publisher host.", etc.
- Dependency missing → enabled button relabelled "Repair action".
### Service rows: Producer (`mixxx-now-playing.service`), Publisher (`musicindex-live-publisher@<instance>`)
- Start (Inactive) / Stop (Active, **no confirmation even when live**) / Reset (Failed) / Logs.
- Publisher dependency missing → Start/Stop/Reset all read "Repair tool" (three identical buttons).

## 6. Stream detail
- Rows: Server, Connection, Signal, Recording, Listeners, optional elapsed/file/song.
- "Connect" → `butt -s` (**goes on air, no confirmation**). "Disconnect" → `butt -d` (no confirmation). Absent if encoder not configured. Missing dependency → both read "Repair encoder".
- No record start/stop control.

## 7. Log pane
- Opened from service/event "Logs". Close, Copy feed tag (event only), drag-resize 200–600, "Go to latest", Following/Paused. One source at a time.

## 8. Transport deck
- Icon-only Previous / Play-Pause / Next. All disabled when nothing playing.
- Pause also clears the now-playing drop file (live metadata stops while paused).
- Shortcuts `cmd-alt-p`, `cmd-alt-right`, `cmd-alt-left` — global, **ignore disabled state** → produce "Playback error" and a retained failure.
- **No Stop, seek, elapsed, volume.** `StopPlayback`, `SeekPlayback`, `PlayTrack` commands exist with no caller; stop only via CLI `v4vmm playback stop`.
- End of last track → "Playback error: advance playlist after driver EOF"; session stays "playing". **A show cannot be ended from the UI.**

## 9. Live Status Strip (Music/Settings while session active)
- "Live status", "Artist - Title", health badge always "Health unknown" (`broadcast_active` always false), recording badge never shown. "Open Show" button (same a11y label as the toolbar tab).

## 10. Starting playback — only from Music
- Playlist track row Play (hover; enabled if track in Library with local file) → `PlayPlaylistAt` + auto playlist RSS check. Does not switch to Show.
- No album/track/"play playlist" button anywhere else.

## 11. Prerequisites set elsewhere
| Prerequisite | Where | How Show signals it |
|---|---|---|
| `playback.driver="mpv"` + `mpv_path` — **default is null driver (silent)** | config / Configuration repair | **Nothing** |
| `broadcast.drop_directory`, `drop_file_target` | config / Configuration repair | Source "Setup needs attention" |
| `broadcast.hosts`, `selected_host` | config / Configuration repair | Source card missing + status text |
| systemd user units, SSH for remote | outside app | "Not installed"/"Not reachable" |
| MusicIndex endpoint | Settings › General | Create/Replace disabled |
| Token file on remote host | manual copy | hint line |
| RSS `podcast:liveValue` tag | external feed edit | **Nothing** (chain "succeeds" with no listeners) |
| `butt` via `broadcast.encoder` | config / Configuration repair | "Not installed" / "Repair encoder" |
| Background runtime | Settings › Diagnostics › Background tools | text only |
- Failed actions are retained and re-offered in Settings › Background tools ("Start publisher", "Connect stream", "Create event", …).
- Inconsistent repair labels: "Repair tool", "Repair action", "Repair encoder", "Repair playback".

## 12. Confirmations / dead ends / unrendered
- **No confirmation anywhere in Show** (Connect = on air, Stop, Reset, Attach, Detach, event switch, Create, Replace).
- Dead ends: More→"No actions"; unlinked Background tools text; inert cuelist rows; Source card vanishing; no end-show; no second event; no Forget.
- Projected but unrendered: event target/registration/check/summary, play_pause_label, health "On air"/"Needs attention", recording badge, queue filter, encoder record, Stop/Seek/PlayTrack.

## 13. Step counts (from launch; app opens on Music)
1. Play a playlist: 3 clicks (Playlists → playlist → hover row Play), +1 to see Show. No "play from top".
2. Go live (services configured, no event, music playing): ~10 clicks + external RSS edit (Show, Live Metadata, Create, Attach, Producer Start, Publisher Start, More, Copy feed tag, Stream, Connect). With an existing attached event: 6.
3. Live metadata: automatic (0); Next: 1; "cue a different next": no cue — reorder in Music, ~3–5 actions.
4. End show: 6 clicks (Stream, Disconnect, Live Metadata, Publisher Stop, Producer Stop, Pause) and it never fully ends — session stays Paused; only CLI clears it.
