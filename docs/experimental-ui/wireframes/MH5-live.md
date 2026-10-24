# MH5 · Go live and come off air

Journey: [MH5](../03a-must-have-journeys.md#mh5--go-live-and-come-off-air) · Principles P5 P4 ·
Components: [C8](01-components.md#c8--confirm-sheet) [C11](01-components.md#c11--checklist-row)
[C13](01-components.md#c13--stage-bar) [C16](01-components.md#c16--show-accessory) [C17](01-components.md#c17--transport)

**Target:** 2 steps on, 2 off. **Today:** 6–10 independent clicks plus an RSS edit outside the app,
no confirmation; 6 to come off, and the show never actually ends.

| Screen | Where | Journey step |
|---|---|---|
| [5.1 Go Live sheet](#51-go-live-sheet) | sheet | Confirm going on air |
| [5.2 Going live](#52-going-live-sequence) | Show | Steps run in order |
| [5.3 First time: feed tag](#53-first-time-feed-tag) | inside 5.2 | Put the live tag in your RSS |
| [5.4 Live console](#54-live-console) | `/show` | Monitor while on air |
| [5.5 Live console, Mixxx](#55-live-console-mixxx-mode) | `/show` | Glance mode |
| [5.6 End Show and Wrap Up](#56-end-show-and-wrap-up) | sheet → Show | Come off air cleanly |

---

## 5.1 Go Live sheet

Opened by **● Go Live…** (Show, Check Show sheet, ⇧⌘L). Available only when Check Show has no blockers.

```
┌────────────────────────────────────────────────────┐
│ Go Live?                                           │ T3
│                                                    │
│ You'll be on air. The app will:                    │ CO
│  1  Use event       Episode 42 (live)   Change ▾   │ BD · menu: other events, New Event, Forget…
│  2  Attach it to    the publisher on studio.local  │
│  3  Start           the now-playing producer       │
│  4  Connect stream  to radio.example:8000          │
│  ☐  Start recording                                │ BD checkbox (butt; built, unwired today)
│                                                    │
│                       [ Cancel ]   [ ● Go Live ]   │ default = state.live
└────────────────────────────────────────────────────┘
```
- Steps that are already done show `✓ already attached` and are skipped.
- **Events:** reuse, create (**New Event**, allowed any time: today only when none exist), or **Forget…**
  an old one (today CLI only). Forget confirms because it deletes the token.

**HIG:** ST4 ✓ ST6 ✓ (confirm names the action; Return = Go Live, Esc = Cancel) · AX2 ✓ (live = ● + word)

---

## 5.2 Going-live sequence

```
┌───────────────────┬──────────────────────────────────────────────────────────────────────────┐
│ (sidebar)         │ Show                                                                     │ LT
│                   │ ● Prepare ─── ● Check ─── ◔ Going Live ─── ○ Wrap Up                     │ C13
│                   │                                                                          │
│                   │ Going live…                                                              │ T2
│                   │ ✓  Event "Episode 42" is live                                    14:59:58│ C11 · FN tabular
│                   │ ✓  Attached to publisher on studio.local                         15:00:01│
│                   │ ◔  Starting the now-playing producer…                                    │
│                   │ ○  Connect stream to radio.example                                       │
│                   │                                                                          │
│                   │                                                     [ Cancel ]           │
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘

If a step fails, the sequence stops there:
│ ⚠  Couldn't start the now-playing producer                                                   │ C11 danger
│    mixxx-now-playing.service failed to start.             [ Try Again ]  [ Logs ]  [ Skip ]  │ CO · buttons
│    Nothing is on air yet. Steps 1–2 are kept.                                                │ CO
```
- **Cancel** stops before the stream connects, so nothing goes on air. Already-done steps stay done (and are listed).
- On success the stage becomes **● Live** and the page becomes the Live console (5.4).

---

## 5.3 First time: feed tag

Shown inside 5.2 the first time an event is used, before the stream connects.

```
│ ⚠  Add the live tag to your podcast's RSS feed                                             │ C11
│    Listeners' apps find the live stream through this line. Paste it inside <channel>:      │ CO
│    ┌───────────────────────────────────────────────────────────────────────────┐           │
│    │ <podcast:liveValue uri="https://…/liveitems/abc123" protocol="socket.io"/> │ [Copy]   │ BD mono · button
│    └───────────────────────────────────────────────────────────────────────────┘           │
│    Your feed: https://myshow.example/rss              [ Check My Feed ]                    │ FN · button
│    ✓ Found the tag in your feed · 15:00:20                                                 │ result
```
- **Check My Feed** fetches the show's RSS and confirms the tag is there (new; today the app copies
  silently, and a known bug writes a bare id instead of a URL).
- Copy gives a toast, *"Copied live tag."*
- The feed URL is asked once (MH6) and remembered.

---

## 5.4 Live console

### Expanded
```
┌──────────────────────────────────────────────────────────────────────────────────────────────┐
│ ●●●  ◧  ‹ ›   Show · ● LIVE 00:42:10           [⌕ Search music and actions   ⌘K]  [⤓]  [◨]   │ HL
├───────────────────┬──────────────────────────────────────────────────────────────────────────┤
│ (sidebar)         │ Episode 42 · ● LIVE                                     [ End Show… ]    │ LT · primary
│                   │ ● Prepare ─── ● Check ─── ● Live 00:42:10 ─── ○ Wrap Up                  │ C13 tabular
│                   │                                                                          │
│                   │ What listeners see                     │ Health                          │ T2 / T2
│                   │ ┌──────────┐ Bloodshot Lies            │ ✓ Stream     connected · audio  │ T1 / C11
│                   │ │ ▓▓▓▓▓▓▓▓ │ The Doerfels              │ ✓ Publisher  sending · 2 s ago  │ HL
│                   │ │ ▓▓▓▓▓▓▓▓ │ Value: 90% artist · 10% … │ ✓ Producer   active             │ SH
│                   │ └──────────┘                           │ ✓ Event      live               │
│                   │ 1:12 ━━━━━●─────── -2:24               │ ● Recording  00:41:55           │ C17
│                   │   ⏮     ❚❚     ⏭                       │ ◍ Listeners  37                 │
│                   │                                        │                                 │
│                   │ Up Next                     Edit       │ Recent                          │ T2 / T2
│                   │ ≡ 3 Old Song                           │ 15:38 Bad Little Seed           │ C1 / FN tabular
│                   │ ≡ 4 Sour Sorrow                        │ 15:34 Intro (talk)              │
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘
```
- **No clicks needed to see** what's going out, the health of each service, listeners and the recording.
- A failing service turns its row ⚠ **in place**, with one action: *Restart Publisher*, *Reconnect Stream*, *Logs* (MH7).
- **Stop is absent while live**, so you can't cut the music by accident. Pause asks first:
  *"Pausing clears what listeners see. Pause anyway?"* (today, pausing silently stops the live metadata).
- Compact: what listeners see → a one-line health summary (*"All 5 healthy"*, tap for the rows) → Up Next. **End Show…** stays at the bottom.


---

## 5.5 Live console, Mixxx mode

```
│ Episode 42 · ● LIVE 00:42:10                                            [ End Show… ]       │ LT · primary
│ ┌──────────────┐  What listeners see                                                        │ T2
│ │   ▓▓▓▓▓▓▓▓   │  Bloodshot Lies — The Doerfels                                             │ T1
│ │   ▓▓▓▓▓▓▓▓   │  from Mixxx · 2 s ago                                                      │ SH
│ └──────────────┘                                                                            │
│ ✓ Stream   ✓ Publisher   ✓ Mixxx producer   ✓ Event   ● Recording 00:41:55   ◍ 37           │ C11 inline
│ Recent: 15:38 Bad Little Seed · 15:34 Sour Sorrow · 15:30 Pie                               │ FN
```
Glance view: bigger now-playing, health on one line, no transport or Up Next (Mixxx does that).
It works well on a second monitor.

---

## 5.6 End Show and Wrap Up

```
End Show… →
┌────────────────────────────────────────────────────┐
│ End the Show?                                      │ T3
│ Listeners will lose the stream.                    │ CO
│  ✓  Disconnect the stream                          │ BD
│  ✓  Stop playback                                  │
│  ✓  Stop recording                                 │
│  ☐  Also stop the publisher and producer           │ off by default
│                       [ Cancel ]  [ End Show ]     │ destructive
└────────────────────────────────────────────────────┘

→ Wrap Up
┌───────────────────┬──────────────────────────────────────────────────────────────────────────┐
│ (sidebar)         │ Episode 42 is off air                                                    │ LT
│                   │ ● Prepare ─── ● Check ─── ● Live ─── ● Wrap Up                           │ C13
│                   │ 15:00 – 15:58 · 58 min · 14 tracks · peak 41 listeners                   │ SH tabular
│                   │                                                                          │
│                   │ Recording                                                                │ T2
│                   │ ▣ episode-42.mp3 · 54 MB               [ Show in Folder ]   [ Copy Path ]│ BD · buttons
│                   │                                                                          │
│                   │ What played                                                              │ T2
│                   │ 15:00  Intro (talk)                                                      │ FN · BD
│                   │ 15:02  Bad Little Seed — Survival Guide                                  │
│                   │ …                                                                        │
│                   │ Issues during the show: 1 publisher restart at 15:21           Details › │ CO
│                   │                                                                          │
│                   │ [ Package Episode (later) ]                        [ Done ]              │ secondary · primary
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘
```
**Done** resets Show to Prepare. **Package Episode** is planned (episode export, from the project's own plans).

**Steps:** on = Go Live… · Go Live = **2** · off = End Show… · End Show = **2**.

**HIG:** ST4 ✓ (End Show destructive + confirm) · ST5 ✓ · TY5 ✓ (all times tabular) · AX2 ✓ · AX5 ✓ (VoiceOver reads listener view, then health, then queue)
