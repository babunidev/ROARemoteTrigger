# ROARemoteTrigger — the creative concept

**Creator and creative director:** Babuni.dev  
**Working package name:** ROARTrigger  
**Target:** Wrath 3.3.5 / TOC interface 30300.

## Give play an expressive afterbeat

ROARemoteTrigger brings the ROAR idea—making a character feel alive through game-native emotes—to another game-client environment. The player does not have to interrupt combat or roleplay to remember a manual emote. A button they already press can *occasionally* produce an additional gesture, selected using an independently configured probability and time gate.

The project is **flavor**, not automation. The ability can fail or be out of range and the emote may still happen because the hook registers **action usage**, not a proven combat result. That distinction is not a flaw to conceal; it is central to the intended interaction and explicitly documented in the source-era README.

The name ROARemoteTrigger suggests the broader family of “ROAR” expression experiments, but this code does not deliver a remote-control service. Here, `/rogu` is the local player's text configuration interface.

## The character and the guild

The first layer is **personal expression**: multiple numbered configurations, per-instance emote pools, and a small spontaneous fallback. The second layer is **community language**: 33 committed <ROAR>-prefixed recruitment messages that represent joy, curiosity and solidarity instead of a race to the top of damage meters.

The README preserves the project's broader statement:

> Characters are not loadouts. They are stories in motion.

The interaction uses the game client's built-in `DoEmote` vocabulary rather than custom animations, and sends guild invitation text only when the player asks.

## The distinct technical direction

The uploaded original file described itself as `RoarGuild v1.32-Epoch` with the Project Epoch/Wrath 3.3.5 target. The code used `hooksecurefunc("UseAction", ...)` where available and a post-delegation wrapper otherwise. Git later removed provider-specific labels, giving the repository its current `1.32` title and generic Wrath 3.3.5 description. These are real code/text evolutions, not a recovered conversation about why a client adaptation was chosen.

Unlike the separate later RoarGuild 1.33, there is **no Roarboard transport or interface in this repository**. That distinction matters for a portfolio that treats related projects as separate creative experiments.

## Creative boundaries

Do not transform the addon into an optimization tool, a spell-success parser, an automatic recruitment spammer, or a conventional UI dashboard without creator direction. The game action remains the player's own. AI assistance may propose and document technical approaches but cannot retroactively claim the author's creative choices.
