# MH7 · Recover when something breaks

Journey: [MH7](../03a-must-have-journeys.md#mh7--recover-when-something-breaks) · Principles P4 P6 ·
Components: [C7](01-components.md#c7--toast) [C9](01-components.md#c9--inbox-item)
[C11](01-components.md#c11--checklist-row) [C12](01-components.md#c12--form-row-with-test)

**Target:** 1 step to the right fix. **Today:** a button becomes "Repair …" → a notice row appears → it opens a raw config field in Settings › Diagnostics.

| Screen | Where | Journey step |
|---|---|---|
| [7.1 Problem in place](#71-problem-in-place) | where it happened | See it, fix it |
| [7.2 Problem in the inbox](#72-problem-in-the-inbox) | Inbox | It also waits for you |
| [7.3 More info](#73-more-info-logs-and-report) | inspector / sheet | Logs and a copyable report |
| [7.4 Can't start](#74-cant-start) | startup | Recovery when the app can't open |

**Rule:** a problem shows **where you are** first, the inbox second, and Settings never as the only place.

---

## 7.1 Problem in place

```
Live console health row (MH5):
│ ⚠ Publisher   stopped sending · 1 min ago        [ Restart Publisher ]  [ Logs ] │ C11 danger

Row action failed (MH1):
│ ▣ Bloodshot Lies   The Doerfels · Single        ⚠ Failed  [↻ Retry]   ⋯          │ C1
│   Couldn't download: the server returned 404.                                    │ FN inline

Toast for a background failure (any screen):
┌─────────────────────────────────────────────────────────────────┐
│ ⚠  Couldn't reach MusicIndex. Your library still works. [Retry] │ C7, stays until dismissed
└─────────────────────────────────────────────────────────────────┘

Setting problem (anywhere):
│ ⚠ Stream encoder isn't responding.      [ Fix… ]  → opens the MH6 encoder form │
```
- **Fix…** opens the exact form from MH6, focused on the field at fault, with **Test**.
- One wording per fix. No "Repair tool" / "Repair action" / "Repair encoder" variants.
- On phone/web, a fix that needs the host reads *"Open v4vmm on studio.local to fix this."*

---

## 7.2 Problem in the inbox

```
│ Setup                                                                                      │ T2
│ ⚠  Stream encoder isn't responding                                     [ Fix… ]    ›       │ C9
│    butt didn't answer at 127.0.0.1:1256. Going live will stop at "Connect stream".         │ CO
│    First seen 14:58 · tried again 15:00                                                    │ FN
```
The inbox entry clears **by itself** when the next check passes. A failed action can be re-run
from here with **Run Again** (the "retained actions" that exist in code today).

---

## 7.3 More info: logs and report

```
Inspector (expanded) / sheet (compact)
┌──────────────────────────────────────┐
│ Publisher                     ✕      │ T3
│ musicindex-live-publisher@mixxx      │ FN mono
│ Failed · since 15:21                 │ SH
│ ──────────────────────────────────── │
│ What happened                        │ HL
│ The service exited with code 1       │ CO
│ after the target changed.            │
│ ──────────────────────────────────── │
│ Log                    Following ●   │ HL · state
│ 15:21:03 target add ok               │ FN mono, selectable
│ 15:21:04 restart                     │
│ 15:21:05 error: token not found      │
│ ──────────────────────────────────── │
│ [ Restart ]  [ Copy Report ]         │ buttons
└──────────────────────────────────────┘
```
Plain explanation first, technical detail after. That matches the project's own "write reports for the operator" rule.

---

## 7.4 Can't start

Shown instead of the app when a core check fails (missing music folder, unreadable database).

```
┌──────────────────────────────────────────────────────────────────────┐
│                     The app can't open yet                           │ LT
│   Your music folder ~/V4Vmusic is missing.                           │ CO
│                                                                      │
│   ⚠  Music folder      not found                                     │ C11
│      [ Create It ]   [ Choose Another Folder… ]                      │ buttons (today: no folder picker)
│   ✓  Database          ok                                            │
│   ✓  Configuration     ok                                            │
│                                                                      │
│   More Tools ▾  (Back Up Database · Restore… · Show Config File)     │ disclosure
│                                [ Quit ]   [ Open App ]               │ primary disabled until ✓
└──────────────────────────────────────────────────────────────────────┘
```
- The common case is fixed with one button. Today it means creating the folder outside the app.
- Database tools stay available but behind *More Tools*: rare and expert-only.

**HIG:** ST4 ✓ · ST5 ✓ · ST8 ✓ · AX2 ✓ · TY6 ✓
