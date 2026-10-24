# Research: First run, Settings, global chrome, CLI (v4vmm @ 38ecd9d)

Raw survey by a read-only code agent. Paths relative to `v4vmm/`.

## A. First run / startup / recovery

### Launch
- No args → desktop app; any args → CLI (`src/main.rs`).
- Bootstrap: maintenance worker, shortcuts + menu, Dark theme, one window with startup screen running the Initial check (`src/app/bootstrap.rs:39-129`, `src/app/startup.rs:81-170`).
- Closing the window quits, no confirmation.

### Brand-new user (no config/db/music dir/network/mpv/flac)
1. Startup screen briefly: "Checking startup requirements", buttons "Checking…", "Open app" (disabled), "Copy startup report", "Quit", page menu Startup/Configuration/Database.
2. Config auto-created (`~/.config/v4vmm/config.toml` on Linux; macOS path via ProjectDirs `xyz.HeyCitizen.v4vmm` UNCONFIRMED). Defaults: `music_dir=~/V4Vmusic`, db in data dir, endpoint `https://api.musicindex.org`, theme dark. Playback/broadcast/encoder only commented out. stderr says "Edit it if needed, then re-run." — misleading.
3. First-run prep creates music folder, DB, `<music_dir>/artists`. No failure → recovery screen skipped.
4. **Lands on Music › Discover › New releases.** Needs MusicIndex; no network → "Recent music unavailable".
5. **No onboarding** (no welcome/hint/get started). Startup notices go to `settings_status`, which only shows on Settings General/Library and Show — **never in Music**.
6. What works:
   - Search works (Library always; Index fails offline).
   - **Audio: default `playback.driver=null` → silent simulated playback; Play looks available; nothing says there's no audio.** Real audio needs Settings › Diagnostics › Configuration › Edit configuration › raw `playback.driver` field.
   - flac: nothing reported until a WAV download.
   - "Check endpoint" only validates URL syntax — **no connectivity test exists**.

### First-run trap: CLI first
- CLI creates config + DB but not the music folder → next GUI launch is not "first run" → missing `~/V4Vmusic` is **blocking**. Recovery has no folder picker and doesn't create folders.

### Recovery screen
- Reached on blocking check, after "End app session", during Restore.
- Title "App needs attention before it can open" / "Ready to open the app".
- Page menu "Choose recovery page": Startup / Configuration / Database.
- Actions: "Check again", "Open app", "Copy startup report", "Quit".
- Stale text: "Full in-app correction is delivered by a later recovery packet."
- Session drain screen: "App session", "Retry drain", "Copy report", "Quit"; after drain user must "Open app" again (except verified Restore).

## B. Settings
Reached by tab, Cmd/Ctrl+3, Cmd/Ctrl+, ("Preferences..."), notice buttons. Group menu: General / Library / Diagnostics (Diagnostics adds page menu).

### General
- "UI scale" XS–XL, "Theme" System/Dark/Light/HC Dark/HC Light — apply immediately, **lost on relaunch unless Save**; no unsaved indicator.
- "Save" (General+Library together). Fails if any optional config field invalid.
- "Use Defaults" — resets endpoint, flac, scale M, theme **Dark** (not System), saves. **No confirmation.**

### Library
- "MusicIndex endpoint" text box.
- "Music directory" read-only ("Open Configuration repair … after ending this session").
- "Audio converter (optional)" → "Converter setup" → Diagnostics › Configuration on `flac_path`.
- "Configuration repair" button. Hidden inputs for music_dir/flac_path also saved.

### Diagnostics (default page: Database)
- **Database tools**: tasks Check / Backup / Preserve files / Restore / Repair upgrade; path fields; Review → Restore two-step (no dialog). Restore ends session automatically.
- **Configuration ("Configuration repair")**: "Edit configuration" raw textarea editor; field buttons labelled with **raw TOML keys** (`music_dir`, `db_path`, `playback.driver`, `broadcast.hosts`, `broadcast.encoder`, …); "Test converters", "Test draft and paths", "Save correction", "Copy draft (redacted)", "Reload file", "End app session" (needed for core path changes). Label mismatch: "Configuration" / "Configuration repair" / "Converter setup".
- **Background tools**: rows Background runtime, Thumbnail maintenance, MusicIndex, Built-in playback, Publisher host, Drop-file publication, Stream encoder, Audio converter, Presentation settings + issues + retained actions. "Edit …" (→ Configuration repair field), "Check …", "Run original action" variants, "Dismiss", "Copy report".
- **App session**: "End app session" (no confirmation) → drain screen.
- **Cached files**: "Cached files (N)" by artist; per-row "Delete", "Delete All Cached" — **delete audio from disk with no confirmation**; result message goes to an invisible status. Stale "above" text.

### Presets
Not implemented (ADR 0069 future).

## C. Global chrome
- Toolbar: logo (inert), tabs Music/Show/Settings (tooltips "Open Music"/"Open Show"/"Show Settings"), search "Search Library and Index" + always-enabled "Search" button; submitting from Show/Settings jumps to Music.
- Window layers: toolbar → capability notice (Music/Show, only on failure: "N setup issue(s); N retained action(s)", "View tools in Settings", per-row Edit/Check/Dismiss) → live status strip (Music/Settings when show active) → download progress (all sections) → content → dialogs (Music only).
- macOS app menu "Application": Preferences, Services, Hide, Hide Others, Show All, Quit (Cmd+Q quits with **no confirmation even during live show**). **Linux has no menu bar.**

### Keyboard shortcuts (cmd→ctrl on Linux)
| Keys | Action |
|---|---|
| Cmd+, | Settings |
| Cmd+Q | Quit |
| Cmd+[ / Alt+Left, Cmd+] / Alt+Right | Back / Forward (Music history; not scoped to tab) |
| Cmd+Alt+P / Right / Left | Play-Pause / Next / Previous, any section, ignore availability |
| Cmd+F, Cmd+Alt+F | Focus search |
| Cmd+N | New playlist (→ Music › Playlists, focus name input) |
| Cmd+1/2/3 | Music / Show / Settings |
| Cmd+R | Refresh Library (Music only) |
| Esc | Music: clear selection or close detail (doesn't move history) |
| Arrows/PgUp/PgDn/Home/End | Move focus in Music lists |
| Enter | Open focused row |
| Space | Select focused row |
| Cmd+A | Select all |
| Right/Left | Open/close singles group; Discover tile focus |

### Background issues
- Blocked command → "App cannot run this action because {X} is unavailable. Open its report in Settings…" → goes to `settings_status`, **invisible in Music**.
- Show depends on raw-config-only settings: `broadcast.hosts`/`selected_host`, `broadcast.drop_directory`, `broadcast.encoder`, `playback.driver`. Discovery path: Show buttons become "Repair …" → click stores retained action → notice offers "Edit … settings" → raw TOML field.
- Broadcaster tokens aren't a setting; created by event registration under `<config>/broadcast/tokens/`.
- Pressing Cmd+Alt+Right with no session → retained Playback failure → notice "Edit player settings" (misleading cause).

### Other
- Back/Forward not scoped to tab (effect in Show UNCONFIRMED). README and settings screen inventory stale.

## D. CLI (`src/cli.rs`)
Creates config + DB if missing. Commands:
- `help`; `now-playing --json`
- `liveitem health|latest <event-id> --json` (`create` retired)
- `broadcast events list|create [--label]|forget <id>|check <id>`; `broadcast targets list|attach <id> --target <name>`; `broadcast readiness --json`; `broadcast repair-routes [<track-id>]`
- `playlists list`, `playlist tracks <id>`, `library tracks`, `library repair-paths`, `track inspect <id>`
- `player ping`
- `playlist play <id> [--position N] [--dry-run]`, `playback set-track|position|next|previous|pause|resume|stop` — **database-only, no audio control**
- `startup-fixture` (debug only)
