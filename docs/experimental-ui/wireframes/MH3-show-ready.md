# MH3 · Get show-ready: updates, problems, pre-show check

Journey: [MH3](../03a-must-have-journeys.md#mh3--get-show-ready) · Principles P4 P8 ·
Components: [C1](01-components.md#c1--item-row) [C7](01-components.md#c7--toast)
[C8](01-components.md#c8--confirm-sheet) [C9](01-components.md#c9--inbox-item)
[C10](01-components.md#c10--activity-popover) [C11](01-components.md#c11--checklist-row)

**Target:** 1 check + 1 per fix. **Today:** spread over 3 places, and 4 of 6 problem types have no fix.

| Screen | Route | Journey step |
|---|---|---|
| [3.1 Inbox](#31-inbox) | `/inbox` | See everything that needs a decision |
| [3.2 Problem group](#32-problem-group) | `/inbox/:id` | Fix one or all |
| [3.3 Updates review](#33-updates-review) | `/inbox/:id` | Apply upstream changes |
| [3.4 Check Show](#34-check-show) | sheet / Show › Check | Is this playlist ready for air? |

---

## 3.1 Inbox

### Expanded
```
┌──────────────────────────────────────────────────────────────────────────────────────────────┐
│ ●●●  ◧  ‹ ›   Inbox                            [⌕ Search music and actions   ⌘K]  [⤓]  [◨]   │
├───────────────────┬──────────────────────────────────────────────────────────────────────────┤
│ (sidebar)         │ Inbox                                                                    │ LT
│                   │ 3 need you · last check 14:02                       [↻ Check Now]        │ SH · button
│ ───────────────── │                                                                          │
│ ▸ Inbox        3  │ Blocks Episode 42                                                        │ T2
│   Settings        │ ⚠  2 tracks can't receive payments                       [Fix All]   ›   │ C9 blocker
│                   │    Their route tag is missing.                                           │
│                   │ ⚠  1 track was removed from its feed                     [Review]    ›   │
│                   │    "Old Song" is no longer in Someone's RSS feed.                        │
│                   │                                                                          │
│                   │ Updates                                                                  │ T2
│                   │ ↻  4 releases changed upstream · 2 new tracks · 18 files to retag        │ C9 update
│                   │    Found by the automatic check · 14:02                [Review]    ›     │
│                   │                                                                          │
│                   │ Setup                                                                    │ T2
│                   │ ⚠  Stream encoder isn't set up                           [Set Up]    ›   │ → MH6 form
│                   │                                                                          │
│                   │ Done today                                                  Show ▾       │ T2 collapsed
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘
```
- **One place** replaces today's five: Needs attention · Apply updates (n) · Update n files · the capability notice · Background tools.
- Sections, in order of urgency: **Blocks <show playlist>** → **Needs a decision** → **Updates** → **Setup** → **Done**.
- Background checks write here (P8). The sidebar count = items that need you. "Done" items never count.

### Compact
```
┌────────────────────────────────────┐
│ ‹ Discover                         │
│ Inbox                              │ LT
│ 3 need you · 14:02       [↻]       │ SH
│ BLOCKS EPISODE 42                  │ SH semibold
│ ⚠ 2 tracks can't receive     ›     │ CO (wraps, never cut off)
│   payments                         │
│   [Fix All]                        │
│ ⚠ 1 track removed from feed  ›     │
│ UPDATES                            │
│ ↻ 4 releases changed         ›     │
│   [Review]                         │
├────────────────────────────────────┤
│ Discover  Library  Show  Search    │
└────────────────────────────────────┘
```
Reached from the **Inbox 3** button in each tab's nav bar.

**HIG:** TY3 ✓ (explanations wrap) · ST5 ✓ · ST8 ✓ (empty: *"All clear. Last checked 14:02."* ✓) · AX2 ✓ (⚠ / ↻ glyph + section words)

---

## 3.2 Problem group

```
┌───────────────────┬──────────────────────────────────────────────────────────────────────────┐
│ (sidebar)         │ ‹ Inbox                                                                  │
│                   │ 2 tracks can't receive payments                                          │ LT
│                   │ Listeners' payments go to the artist through a route tag in each file.   │ CO
│                   │ These files don't have one yet.                       [ Fix All ]        │ primary
│                   │                                                                          │
│                   │ ▣  Old Song           Someone · in Episode 42            [Fix]    ⋯      │ C1
│                   │ ▣  Other Track        Band · in Episode 42               [Fix]    ⋯      │
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘
```

**Every problem type has an action or a plain explanation** (journey MH3 rule):

| Problem (code name) | Title shown | Fix |
|---|---|---|
| NoRouteTag | Can't receive payments | **Fix** / **Fix All** (writes the route tag) |
| RouteOutOfDate | Payment info is out of date | **Update** / **Update All** |
| RemovedFromFeed | Removed from its feed | **Keep Anyway** · **Remove from Playlists** · **Drop** |
| FileMissing | File is missing | **Get Again** |
| NotDownloaded | Not downloaded | **Get** |
| NoRoutesUpstream | Artist hasn't set up payments | No fix here. *"The artist's feed has no payment info. It will play, but listeners can't send value."* **[Open Feed]** · **Keep Anyway** |

Fix result: the row turns ✓ **in place**, and the group collapses into "Done" when empty (ST5).

---

## 3.3 Updates review

```
┌───────────────────┬──────────────────────────────────────────────────────────────────────────┐
│ (sidebar)         │ ‹ Inbox                                                                  │
│                   │ Updates from 4 releases                                                  │ LT
│                   │ 2 new tracks · 6 changed details · 18 files to retag  [ Apply All ]      │ SH · primary
│                   │                                                                          │
│                   │ ▸ ▣ Disco Swag – The Album    1 new track · title changed      [Apply]   │ C1 expandable
│                   │ ▾ ▣ Deathdreams               description · 11 files to retag  [Apply]   │
│                   │       Description   "Old text…"  →  "New text…"                          │ FN · BD diff
│                   │       Files         11 files get the new description tag                 │
│                   │ ▸ ▣ …                                                                    │
└───────────────────┴──────────────────────────────────────────────────────────────────────────┘

Apply All →
┌────────────────────────────────────────────────────┐
│ Apply Updates from 4 Releases?                     │ T3
│ Gets 2 new tracks and rewrites tags in 18 files.   │ CO
│ Files playing on air right now are skipped.        │
│                       [ Cancel ]  [ Apply Updates ]│
└────────────────────────────────────────────────────┘
```
It's a confirm sheet because it **rewrites files**. Result: a C7 toast and an Activity entry,
*"Updated 4 releases, wrote 18 files · 14:05"*.

**Steps:** Review · Apply All · confirm = **3**, for what today is 4 separate buttons that never add new tracks.

---

## 3.4 Check Show

Opened from the playlist page (**✓ Check Show**), from Show › Check (MH4/MH5), or ⇧⌘C.

```
┌──────────────────────────────────────────────────────────────┐
│ Check Show · Episode 42                              Done    │ T3 · HL
│ Built-in player · going live                                 │ SH (mode, from MH6)
│ ──────────────────────────────────────────────────────────── │
│ Music                                                        │ T2-equivalent HL
│ ✓  14 tracks downloaded                                      │ C11
│ ⚠  2 tracks can't receive payments            [Fix All]      │
│ ✓  Feeds checked · nothing removed                           │
│ ◔  Checking 6 feeds for changes…                             │
│ Setup                                                        │
│ ✓  Audio player       mpv                                    │
│ ✓  Publisher          studio.local · active                  │
│ ⚠  Stream encoder     not set up                  [Set Up]   │
│ ✓  Live event         Episode 42 · live                      │
│ ──────────────────────────────────────────────────────────── │
│ 2 things to fix before going live.                           │ CO
│                      [ Check Again ]   [ Play Without Live ] │
└──────────────────────────────────────────────────────────────┘
 Expanded: sheet (or the Show page body at stage "Check") · Compact: full-screen sheet
```
- Runs **together**: playlist RSS check + readiness of every track + the setup checklist for the
  chosen mode (MH6). Today these are "Check RSS", Needs attention and the Show cards.
- The rows fill in as each check finishes (skeleton → result), so there's no blank wait.
- With zero ⚠ the footer reads **✓ Ready to go live** and the primary becomes **[ Go Live… ]** (MH5).
- **Mixxx mode** hides the "Music" group's download rows and shows *Mixxx producer* in Setup.

**HIG:** ST3 ✓ (one primary changes with the result) · ST6 ✓ (fixes open inline or in the inspector, never a sheet on a sheet: Set Up switches to the Setup page) · LA5 ✓ · AX2 ✓
