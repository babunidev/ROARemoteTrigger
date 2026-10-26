# ROARemoteTrigger — creative and technical development journal

**Creative author:** Babuni.dev.  
**Source base:** Repository README, Lua, TOC and dated GitHub commit diffs inspected 2026-10-26. This is a **retrospective reconstruction**, not a transcript of every human–AI conversation.

## An expressive idea moving between client environments

Across the ROAR addon experiments, the creator's recurring goal is to add life to World of Warcraft play through natural, small gestures. ROARemoteTrigger is a distinct installment: it keeps the familiar pattern of emotes connected to action-bar use and presents that experience for **Wrath 3.3.5** rather than the Vanilla 1.12 environment of some earlier repositories.

The most important distinction is visible in the README: the addon responds to **the player's button attempt**, not to a guaranteed successful spell. A combat ability may fail, and a roar can still appear. In a performance-oriented tool that would be the wrong signal. Here the user is expressing character presence, not logging efficiency. The player can decide which buttons feel expressive and adjust how often each one responds.

This reading is grounded in code and public text. It is not evidence that we have recovered the creator's original private brainstorming about the move to Wrath.

## 27 April 2026 — a large first committed adaptation

The repository was opened on **2026-04-27** (`b1077ec`). The first uploaded `ROARTrigger.lua` (`c1cce7d`) was already a substantial program rather than a small prototype. Its header identified itself as **“RoarGuild v1.32-Epoch”** for **Project Epoch / Wrath 3.3.5** and declared `ROGUDB` as saved data. There were profile-aware emote instances, a low-chance global fallback, manual roar, recruitment messages, rested XP and roar statistics.

That original header is important as historical evidence. The repository name **ROARemoteTrigger** and installed addon name **ROARTrigger** do not erase the close relationship to earlier RoarGuild ideas and source structure. At the same time, sharing logic and a name does not prove a documented cross-repository cherry-pick, merge, or exact collaborator contribution; none was verified in the inspected commit graph.

The port-style implementation has a concrete architectural distinction: if `hooksecurefunc` is present it attaches `ROGU_OnUseAction` after `UseAction); otherwise it wraps the function, calls the original first and then invokes the addon callback. This preserves the original action rather than replacing it with new spell-casting behavior. It still requires target-client verification to know which branch runs and whether it coexists with other hooks.

The manifest was created separately the same day (`378e511`) with **`Interface 30300`** and a `1.32-Epoch` version. A quick revision (`6a6f8f5`) changed the displayed TOC title from **RoarGuild** to **RoarTrigger**. That is a visible naming decision in the committed files, even though the Lua comment remained aligned with its historical family name.

## The system at the center

A player creates numbered configurations. Instance 1 may watch physical action slot 5, instance 2 another button, and each can select an emote from a shared registry using its own chance and timer. A failed chance roll still consumes an instance's cooldown, producing bursts that are limited without tying them to actual spell completion.

A second independent system makes rare spontaneous responses possible: **five successful outcomes per thousand probability rolls by default**, subject to a two-second throttle after success. Those fallback attempts occur for action-slot numbers from 1 to 200, whether a special configured instance matched or not. It is therefore possible for a single input to generate both a chosen-slot emote and a fallback emote.

The code also holds a few playful utility features: `/rogu rexp` reports rested XP, hourly activity summaries count emotes, and a reminder can note when the player has not roared in a while. They are checked during player activity, not from a background scheduler.

The creative text is as important as the implementation. A pool of ROAR recruitment lines frames the guild as a home for wandering, stories, kindness, laughter and shared adventures rather than a competitive efficiency machine. The current source includes **33** such lines. They are sent only when the player asks with `/rogu invite`.

## 13 May 2026 — explaining the tool in public

On **May 13, 2026**, a major README rewrite (`5672063`) transformed a brief repository introduction into a lengthy installation and troubleshooting guide. This document repeatedly clarified the most easily misunderstood behavior: **“It detects action bar usage.”** It did not claim to parse damage logs, verify targets or automatically cast spells.

The same date's source-side work was comparatively about maintaining the recruitment prose, not evidence of a new core feature. The result was a developer-facing and player-facing package whose explanation began to match the mechanics already in Lua.

A strong portfolio record should include this public-facing work as part of development, not just the initial code: making the addon usable by others required explaining its commands, folder layout, chance rules and limitations.

## 18 September 2026 — removing provider-specific language

The **September 18** commit `37130e3` changed README, Lua header and TOC to describe the addon as targeting **Wrath 3.3.5** generically, instead of labeling it `1.32-Epoch` and naming a specific provider. The package continued declaring **30300**. This demonstrates a deliberate text/metadata change in source history; the Git diff does **not** prove the addon was newly tested on every Wrath server.

The project still retains the internal Lua string `RoarGuild v1.32`. The current installable title is RoarTrigger. Keeping those separate reveals the work's lineage rather than sanding away its history.

Notably, this standalone source **does not include Roarboard**. Roarboard was developed in the separate RoarGuild v1.33 repository; backfilling it into this earlier target client's description would misrepresent what users actually receive.

## October 26 — documentation becomes part of the artifact

Earlier October 2026 commits standardized Babuni.dev author credit and put down a short documentation scaffold. This pass inspected the current Lua, TOC and public README, compared the original creation, May documentation work and September renaming diffs, and rebuilt a portfolio-ready record.

**Creator contribution:** original project vision, character/guild expressive goals and decisions embodied in the committed addon. **AI contribution during this pass:** technical inspection, narrative reconstruction, code/user-guide reconciliation, 33-line content inventory and eleven Markdown documentation files. This session included **no game-client execution, no Lua implementation changes and no new approved design decision**.

The questions that remain concern real behavior in the target client: secure hook support, multiple-addon composition, cooldown/fallback interactions, correct profile migration, and whether the original expressive intensity feels appropriate in the game. The portfolio should leave those visible until tested rather than pretending the finished source is proof of a successful runtime release.
