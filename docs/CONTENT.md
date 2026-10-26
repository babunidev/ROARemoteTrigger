# ROARemoteTrigger — in-world content and vocabulary

**Creative owner:** Babuni.dev.  
**Canonical writing and emote lookup:** `ROARTrigger.lua`.

## Guild recruitment language

The current source contains **33** English-language invitation variants in its `inviteText` array. Each begins with the visible **<ROAR>** prefix. The messages are randomized by `/rogu invite` and sent only when a user issues that command. The text repeatedly invokes:

| Motif | What it communicates |
| --- | --- |
| Campfires and roads | Adventure and patient exploration |
| Laughter, ale and unexpected mistakes | Fellowship rather than perfection |
| Curiosity and making things | A welcoming place for unusual interests |
| Respect and steady courage | Shared character rather than leaderboard pressure |
| A collective ROAR | A recognizable ritual of guild belonging |

A representative example of the current voice is **“We hunt for stories not for speed”**—a small phrase in the embedded writing, not a statement from the game client.

The original April 27 file had a larger set of long and short invitation pieces. A September 18, 2026 source diff revised/condensed the variants to the current 33. Git proves the text changed; it does not record which individual lines were originally written by the creator and which, if any, were AI-generated during earlier development.

## Emotes, not custom recordings

The fresh account-wide emote registry begins with **ROAR**. The user can register further game-native emote tokens and refer to them by numeric ID in individual instance lists. Potential examples in the README include CHEER, LAUGH, FLEX and CHARGE, but the source does **not** validate game-client support at time of registration. Those tokens cause `DoEmote` to execute, not a prerecorded sound shipped by this repository.

The fallback also has an `emoteIDs` array, defaulting to ID 1. The manual `/rogu roar` command always issues ROAR.

## Other text

The interface prints simple local chat diagnostics and configuration values such as `pressed slot 5`, the profile key, cooldowns and the message **“You have not roared in a while.”** The rested XP command reports bubbles and points, and the hourly statistics summary counts emotes. No custom GUI illustrations, language packs, quest dialogue or Roarboard messages exist in this repository.

## Authorship / use in a public portfolio

Credit the ROAR-themed writing direction to Babuni.dev and distinguish native game emote tokens from project-authored recruitment text. A docs-only reconstruction must not invent which collaborator first drafted each phrase. Do not copy player chat logs or confidential guild exchanges into the portfolio.
