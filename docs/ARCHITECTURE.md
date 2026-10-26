# ROARemoteTrigger — code architecture (ROARTrigger 1.32)

**Examined source:** `ROARTrigger.lua`, `ROARTrigger.toc` at the beginning of the 2026-10-26 audit. Source reading; **no client execution**.

## Runtime entry points and hook strategy

```text
WoW UseAction(slot, checkCursor, onSelf)
    |-- if hooksecurefunc exists:
    |      hooksecurefunc("UseAction", ROGU_OnUseAction)
    |      addon callback runs as a post-hook
    |
    '-- otherwise:
           capture _Orig_UseAction
           call _Orig_UseAction(slot, checkCursor, onSelf)
           call ROGU_OnUseAction(slot, checkCursor, onSelf)
           return original function's result

ROGU_OnUseAction(slot,...)
    -> ROGU_LoadOnce()
    -> normalize pressed slot with tonumber()
    -> optional watch-mode chat report
    -> iterate profile instance configurations for matching slot
       -> cooldown gate -> cfg.last = now -> chance roll -> DoEmote
    -> independent fallback chance for slots 1..200
       -> success -> DoEmote and fb.last = now
    -> reminder gate
    -> on-activity stats prune/report gate
```

The code deliberately reacts to **action attempts, not successful combat events**. The `hooksecurefunc` branch and fallback are both architected to call addon logic **after** the normal button function. Compatibility and secure execution details are dependent on the actual Wrath client.

## Saved data and profile boundaries

```text
ROGUDB    (account-wide TOC SavedVariables)
├── emotes = { [1]={emote="ROAR"}, ... }
└── profiles
    └── "<character>-<realm>"
        ├── enabled
        ├── slots = {
        │   [1] = { slot, chance, cd, last, emoteIDs },
        │   ...
        │ }
        ├── fallback = { enabled, chancePermille, cd, last, emoteIDs }
        ├── stats = { total, stamps, head, lastReport }
        └── _migrated
```

The initial shared registry is ID **1 = ROAR**, with safeguards to recreate it when missing/invalid. Existing valid ID-1 entries are uppercased but **not guaranteed overwritten as ROAR**. `ROGU_SanitizeEmoteIDs` removes duplicate/out-of-range references and defaults an empty list to ID 1.

`ROGU_MigrateLegacyRootToProfile` copies old `ROGUDB.slots`, `enabled` and `fallback` to the initializing profile and clears the root fields. Test first-character migration and subsequent characters before asserting perfect migration across accounts. `PLAYER_LOGIN` invokes `ROGU_LoadOnce`, resets each instance and fallback cooldown timestamp, resets session `lastRoar` and `lastReminder`, then syncs profile.

## Probability and time details

| Trigger | Default chance | Default interval | When is time gate consumed? |
| --- | --- | --- | --- |
| Configured instance | **100 / 100** | **6 seconds** | After eligible attempt, **before** chance roll |
| Global fallback | **5 / 1000 = 0.5%** | **2 seconds** | **Only when** fallback succeeds |

The independent fallback is considered on any pressed slot where `slot` is a number from 1 to 200, even if a configured instance already emitted. Both are suppressed when global `ROGU.enabled` is false. Two configured instances can share a physical slot, causing two independent checks.

`ROGU_MaybeReminder` reports no recent roar after **420 seconds** since `ROGU.lastRoar`, and rate-limits subsequent reminders with **73 seconds** of spacing. These checks are made on action use; there is no `OnUpdate` alarm.

## Statistics and user-facing commands

Each emote produced by an instance, fallback, or manual `/rogu roar` increments the profile's lifetime total and appends a timestamp. Stats prune events older than **3600s**, then derive the last-hour count and average per minute. An hourly report is checked on later action activity; it is not autonomously scheduled.

`/rogu` handlers include `watch`, `slotX`, `chanceX`, `timerX`, `emote`, `emoteX`, `fallback chance`, `fallback timer`, `info`, `on/off`, `reset`, `resetcd`, `roar`, `rexp`, and `invite`. They use Wrath-compatible `string.match` and length operator `#`, in contrast to the older Vanilla Lua 5.0 source style.

`/rogu invite` selects from **33** embedded guild messages. Its destination is a numeric channel index 1–10 (default 1). The source does not verify the channel's human-readable name.

## Explicit absence of later RoarGuild features

The separate RoarGuild **1.33** source includes Roarboard, but `ROARTrigger.lua` in this repo **does not** register `/roarboard`, process `CHAT_MSG_CHANNEL`, send `RB1` packets, or store `ROGUDB.roarboard`. Its only registered frame events are `PLAYER_LOGIN` and `PLAYER_LOGOUT`.

## Client, packaging and edge cases

`ROARTrigger.toc` declares `Interface: 30300`, title **RoarTrigger**, version **1.32**, author Babuni.dev, account-wide SavedVariables `ROGUDB` and Lua file `ROARTrigger.lua`. Repository name is **ROARemoteTrigger**, which is distinct from installation folder. No additional media, sound pack, or GUI files were observed.

Static considerations requiring runtime tests: availability and permissibility of the secure hook; hook composition with other addons; behavior on invalid or fractional slots; duplicate instance triggers; whether saved data migrates cleanly for every character; and whether the particular Wrath distribution accepts all emote tokens and chat calls. None is claimed to be a reproduced bug.
