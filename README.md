# ROARemoteTrigger / ROARTrigger

**Created and directed by:** Babuni.dev  
**Repository:** [`babunidev/ROARemoteTrigger`](https://github.com/babunidev/ROARemoteTrigger)  
**Installable addon name:** **ROARTrigger**  
**Version:** **1.32**  
**Target client:** World of Warcraft **Wrath 3.3.5**, interface **30300**  
**Audit status (2026-10-26):** Git and Lua source reviewed; **no in-game tests** were run.

**ROARTrigger** adds playful, configurable emotes to action-bar usage. You decide which slots might trigger a `/roar` (or another native emote) and how frequently. It is designed for **character flavor and ROAR guild atmosphere**, not damage optimization, spell casting or combat analysis.

**An important distinction:** the addon observes attempts to use an action-bar slot. It does **not** confirm that a spell actually landed or that an ability successfully activated. An out-of-range or otherwise unsuccessful ability can still result in an emote.

The repository name, installed folder, addon title, and slash command are **not identical**. Keep these separate:

| Purpose | Name |
| --- | --- |
| GitHub repository | `ROARemoteTrigger` |
| AddOns folder | `ROARTrigger` |
| Lua source / TOC | `ROARTrigger.lua` / `ROARTrigger.toc` |
| Displayed addon title | `RoarTrigger` (from the actual TOC) |
| Slash command | `/rogu` |
| SavedVariables | `ROGUDB` |

The Lua's opening comment still says **“RoarGuild v1.32”**, reflecting the code's relationship to the earlier RoarGuild implementation. That historical string is not an invitation to rename this package or conflate it with the separate RoarGuild repository.

## Installation

Copy **`ROARTrigger.lua`** and **`ROARTrigger.toc`** into this folder:

```text
World of Warcraft/
└── Interface/
    └── AddOns/
        └── ROARTrigger/
            ├── ROARTrigger.toc
            └── ROARTrigger.lua
```

Restart the client or run `/reload`. Make sure the addon is enabled in the AddOns list. The TOC directly loads `ROARTrigger.lua`; installing the GitHub repository under its longer name without checking the folder structure may create confusion. `README.md` is optional for the game loader.

The manifest explicitly targets **interface 30300**. A Vanilla 1.12 addon environment is **not** covered by that declaration; test any different game client before advertising compatibility.

## Quick setup

Use `/rogu watch` to display action-bar slot IDs as you press them, then run it again to turn the diagnostic messages off. If the desired button reports slot 5:

```text
/rogu slot1 5
/rogu chance1 40
/rogu timer1 10
/rogu info
```

That creates **instance 1**, watching physical action slot **5**, with a **40% chance on an eligible attempt** and **10 seconds** between eligible attempts. The `1` suffix identifies the configuration instance, **not** the action slot number. Other instances can watch additional slots with their own probability and timer.

### Main slash commands

| Command | Source-defined purpose |
| --- | --- |
| `/rogu help` | Show available commands |
| `/rogu info` | Show version, current profile, emote registry, configured slots, fallback and statistics |
| `/rogu watch` | Toggle reports of physical action-slot IDs |
| `/rogu slotX <slot>` | Create/reassign instance X to a physical slot |
| `/rogu chanceX <0-100>` | Set chance percent for instance X |
| `/rogu timerX <seconds>` | Set minimum interval for instance X |
| `/rogu emote <TOKEN>` | Register an emote in the shared account-wide list |
| `/rogu emote list` | List known emote tokens and IDs |
| `/rogu emoteX <id>` | Add an ID to instance X's available choices |
| `/rogu emoteX -<id>` | Remove that ID from instance X |
| `/rogu emoteX clear` | Return instance X to emote ID 1 |
| `/rogu emoteX list` | Show instance X's current emote options |
| `/rogu fallback chance <0-1000>` | Global fallback chance measured in **per mille** |
| `/rogu fallback timer <seconds>` | Global fallback post-success cooldown |
| `/rogu roar` | **Always** perform built-in ROAR (not a random instance emote) |
| `/rogu invite <1-10>` | Manually send a random ROAR recruitment message to a numbered chat channel |
| `/rogu rexp` | Show rested XP in bubbles (capped at 30) |
| `/rogu resetcd` | Reset instance, fallback and session reminder time gates |
| `/rogu reset` | Delete the configured instances, preserving master emote definitions |
| `/rogu on` / `off` | Enable/disable automatic emote generation |

Commands are recognized without case sensitivity for the main command word. A configured instance must exist for `chanceX` and `timerX`; `emoteX` can create a placeholder instance with no physical slot yet.

**Emote tokens** are game-native (such as ROAR, CHEER or LAUGH); the addon does not validate a newly entered token against the game client's supported list. Check `/rogu emote list` before referring to an ID.

## Two kinds of automatic emote

**Configured instance:** Once its cooldown has elapsed, a watched-slot press checks that instance's chance. The instance's last-attempt time is updated **before** the chance roll, so a failed roll consumes the interval. Default instance configuration is **100%** and **six seconds**.

**Independent global fallback:** By default a valid action slot numbered **1–200** can also trigger a rare emote at **5/1000 = 0.5%**. It has a **two-second** gate, but unlike per-instance timing, its last time advances **only after a successful fallback**. A fallback and a configured instance can both emit on the same press.

Both automatic paths obey the overall `/rogu on/off` state. The manual `/rogu roar` command directly calls ROAR even when automatic triggering has been disabled.

## Profiles, stats and reminders

The TOC declares account-wide `ROGUDB`, but Lua stores **per-character configuration profiles** keyed by character name and realm. Those profiles contain instance slot settings, fallback, enabled state and event statistics. The master emote ID list is shared within the account-wide table. Old root-level settings have a one-time migration path.

The addon tracks emitted emote counts: total and a **rolling one-hour window**; it may print a summary when an hour has elapsed **and another action press occurs**. It can also issue a local “You have not roared in a while” reminder on later action activity after **420 seconds since its last recorded roar**, throttled to at least **73 seconds** between reminder messages. These are not autonomous background timers.

On `PLAYER_LOGIN`, saved profile state is loaded and instance/fallback cooldown timestamps reset, helping prevent previous-session timing values from blocking new activity.

## Guild identity and invitation text

The current Lua source includes **33** randomized `<ROAR>` recruiting messages. Their voice celebrates curiosity, friendship, stories, wandering and shared adventures instead of player performance. Sending is **manual** through `/rogu invite`; there is no automatic advertising loop.

**Check your actual channel numbers.** The source defaults to channel 1 when no value is given and clamps the supplied value to 1–10; it does not verify that channel 1 is General on your client.

## What this repository is—and is not

This is the **Wrath 3.3.5-focused ROARTrigger**, in the `ROARemoteTrigger` repository. It shares patterns and historical source naming with [RoarGuild](https://github.com/babunidev/RoarGuild), but it has a separate commit sequence, target client and package name. **It does not include the Roarboard message-board feature** present in the later 1.33 RoarGuild repo, and it does not contain the GodBod exercise system.

For source provenance and how the package evolved, see [the journal](docs/DEVLOG.md). This repository is released with an [MIT license](LICENSE) credited to Babuni.dev. During the 2026-10-26 documentation pass, **Lua, TOC and LICENSE were left untouched**.

## Development documentation

- [Creative concept](CONCEPT.md) · [Interaction design](docs/DESIGN.md) · [Guild content and emotes](docs/CONTENT.md)
- [Architecture](docs/ARCHITECTURE.md) · [Design and technical decisions](docs/DECISIONS.md) · [Development journal](docs/DEVLOG.md)
- [Verification roadmap](ROADMAP.md) · [Changelog](CHANGELOG.md)
- [AI collaboration and attribution](docs/AI_COLLABORATION.md) · [Development-agent instructions](AGENTS.md)
