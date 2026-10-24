# MH6 · Get set up: first run and show setup

Journey: [MH6](../03a-must-have-journeys.md#mh6--get-set-up) · Principles P6 P7 ·
Components: [C11](01-components.md#c11--checklist-row) [C12](01-components.md#c12--form-row-with-test)
[C15](01-components.md#c15--empty-state)

**Target:** 1 step per item (form + Test). **Today:** raw config text (`broadcast.hosts`, `playback.driver`), silent default player, no onboarding.

| Screen | Where | Journey step |
|---|---|---|
| [6.1 Welcome](#61-welcome-first-launch) | first launch | App checks itself |
| [6.2 Fix an item](#62-fix-an-item) | sheet | One form, one Test |
| [6.3 Show setup: pick a mode](#63-show-setup-pick-a-mode) | Show, first visit | How you run your show |
| [6.4 Show setup checklist](#64-show-setup-checklist) | `/settings/show` | Each piece goes green |
| [6.5 Settings structure](#65-settings-structure) | ⌘, / `/settings` | Where everything lives |

---

## 6.1 Welcome (first launch)

```
┌──────────────────────────────────────────────────────────────────────┐
│                                                                      │
│                              ◎ (app mark)                            │
│                    Welcome to V4V Music Manager                      │ LT
│      Find Value4Value music, keep it ready, and run your show.       │ CO
│                                                                      │
│   ✓  Music folder        ~/V4Vmusic (created)              Change    │ C11
│   ✓  MusicIndex          reached api.musicindex.org                  │
│   ✓  Audio player        mpv found · sound is on                     │
│   ⚠  WAV converter       flac not found             How to Install   │
│      Only needed for the few releases that ship WAV files.           │ FN
│                                                                      │
│                                              [ Continue ]            │ primary
└──────────────────────────────────────────────────────────────────────┘
 Compact: same list, full screen, Continue pinned to the bottom.
```
- Runs the checks **by itself** and shows them filling in. Nothing to type for the common case.
- mpv found → the real player is turned on (today the silent player is the default and nothing says so).
- **MusicIndex unreachable** → ⚠ with **Try Again** and *"You can still continue. Discover needs a connection."*
- **Continue** → Discover, with a one-time hint on the first tile's Get button, and an Inbox checklist:
  *Get a release · Make a playlist · Set up for shows (optional)*.

**HIG:** ST3 ✓ · ST8 ✓ (offline path) · AX2 ✓ · TY2 ✓ (one LT)

---

## 6.2 Fix an item

Every ⚠ anywhere (welcome, Check Show, Inbox, a Show health row) opens **the same form for that item**.

```
┌────────────────────────────────────────────────────┐
│ Cancel          Audio Player              Save     │ T3 · buttons
│ ────────────────────────────────────────────────── │
│ Player                                             │ HL
│ ( ◉ mpv )  ( ○ No sound — testing only )           │ segmented
│                                                    │
│ mpv location                                       │ HL
│ ┌──────────────────────────────────┐ [Choose…]     │ BD · file picker
│ │ /usr/bin/mpv                     │               │
│ └──────────────────────────────────┘               │
│ Found automatically. Change only if you use a      │ FN
│ different build.                                   │
│ [ Test ]  ✓ Played a test tone · mpv 0.38          │ button · result
└────────────────────────────────────────────────────┘
```
- Labels in words, never config keys. Advanced users get **Show Config File** in Settings › Advanced.
- **Save** is disabled while the draft is invalid, and the reason shows under the field (FN, `state.danger` + ⚠).
- A change that needs a restart says so on the Save button: **Save and Restart Session**.

---

## 6.3 Show setup: pick a mode

The Show page body on first visit (replaces 4.1 until set up).

```
┌───────────────────┬──────────────────────────────────────────────────────────────────────────┐
│ (sidebar)         │ Show                                                                     │ LT
│                   │ How do you run your show?                                                │ T2
│                   │                                                                          │
│                   │ ┌──────────────────────┐ ┌──────────────────────┐ ┌─────────────────────┐│
│                   │ │ ▶ Built-in Player    │ │ ◎ Mixxx              │ │ ● Record Only       ││ T3
│                   │ │ Play playlists from  │ │ DJ in Mixxx. This    │ │ Capture a show to   ││ CO
│                   │ │ this app and go live.│ │ app sends now-       │ │ publish later.      ││
│                   │ │                      │ │ playing and watches  │ │                     ││
│                   │ │                      │ │ health.              │ │ Coming later        ││ C1
│                   │ └──────────────────────┘ └──────────────────────┘ └─────────────────────┘│
│                   │ You can change this any time in Settings › Show.                         │ FN
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘
 Compact: three stacked cards.
```

---

## 6.4 Show setup checklist

Same page in Settings › Show and in the Show page until complete. **MH3's Check Show reads this list.**

```
┌───────────────────┬──────────────────────────────────────────────────────────────────────────┐
│ Settings          │ Show                                                                     │ LT
│   General         │ Built-in Player · going live                              Change Mode ▾  │ SH · menu
│   Library         │                                                                          │
│ ▸ Show            │ ✓  Audio player       mpv · sound on                        [Change]     │ C11
│   Advanced        │ ✓  Publisher          studio.local · active                 [Change]     │
│                   │ ⚠  Now-playing folder not chosen                            [Set Up]     │
│                   │ ⚠  Stream encoder     butt isn't set up                     [Set Up]     │
│                   │ ○  Live event         none yet · created when you go live                │
│                   │ ○  Your show's feed   needed once to check the live tag     [Add]        │
│                   │                                                                          │
│                   │ 2 of 6 left. You can play without these. Going live needs them.          │ CO
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘
```
Each **Set Up / Change** opens a 6.2-style form. For example, the stream encoder:

```
 Stream encoder                                                                        T3
 butt location            [ /usr/bin/butt           ] [Choose…]                        C12
 Server                   [ radio.example           ]  Port [ 8000 ]
 Control address          ( ◉ This computer ) ( ○ Another computer: [        ] )
 [ Test ]   ✓ butt 1.42 answered · not connected                                       result
```

Mixxx mode swaps **Audio player** for **Mixxx producer** (`mixxx-now-playing`) and drops the playlist requirement.
On phone/web, items that control local services read **Requires desktop** ([00 §1](00-foundations.md#1-platforms-and-assumptions)).

---

## 6.5 Settings structure

macOS: a separate Settings window (⌘,) with toolbar tabs. Linux / web / tablet: the sidebar "Settings" item with these sections.
Phone: a grouped list.

| Section | Contains |
|---|---|
| **General** | Appearance (System / Light / Dark / Increased Contrast) · Text size · applies **and saves** instantly |
| **Library** | Music folder (Move…) · MusicIndex server (Test) · WAV converter (Test) · Automatic update checks (on, every 6 h) |
| **Show** | 6.4 checklist · mode |
| **Advanced** | Background services status · Database (Back Up, Restore…, Check) · Show Config File · Copy Diagnostics Report |

- No separate Save button in General (today, changes are lost on relaunch unless you press Save).
- "Use Defaults" becomes **Reset Section…** with a confirm sheet (today it acts without asking).

**HIG:** ST1 ✓ (platform-native settings) · ST4 ✓ · LA2 ✓ · TY6 ✓
