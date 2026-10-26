# ROARemoteTrigger — interaction and expression design

**Creative author:** Babuni.dev.

## A gesture attached to something the player already does

The core interaction is deliberately light. The player chooses which action-bar slots belong to which expression profiles; chance and a cooldown keep the resulting emotes varied rather than mechanically constant. An emote is a small performance of the character's personality, not part of the ability's success or damage outcome.

The source explicitly uses a **post-action** callback when possible. That design reflects the addon not replacing gameplay with its own rotation. The player makes the ordinary button press, and the addon may add a native WoW gesture.

## Making two numbers comprehensible

`/rogu slot1 5` means **instance 1** watches **physical action slot 5**. `/rogu chance1 40` adjusts that same instance's chance, and `timer1 10` its interval. This separation permits independent, reusable configurations and more than one watched slot.

The main accessibility affordance is `/rogu watch`: rather than requiring people to know internal action-slot IDs beforehand, it prints them as they press the intended buttons. There is no graphical configuration interface.

The emote vocabulary is partly user-controlled through an account-wide token registry. Instances select references to that list rather than shipping a fixed four-token set. Fresh lists start at ROAR, with new tokens entered by the user.

## Rare moments between configured actions

The global fallback uses its own probability, **0.5% by default**, on ordinary eligible action slots (1–200). Its success-only two-second throttle makes it a different sort of rhythm from instance cooldowns. The player can thus get a surprising roar even while pressing an action not individually assigned to a special configuration.

The two systems can respond simultaneously. That may be funny or noisy depending on how the creator wants the experience to feel; there is no hard-coded exclusivity.

## Guild voice

The 33 <ROAR>-tagged recruitment variations are part of the emotional design. They talk about wandering, laughter, shared tales, compassion, and resisting the urge to measure every moment. Recruitment is not automated; someone must issue `/rogu invite` with a channel number.

The source has no Roarboard or shared posting panel. Copying descriptions of RoarGuild 1.33 into this older client adaptation would falsify the user experience.

## Items for playtesting, not implied decisions

- How often is a failed ability followed by an emote, and does that feel delightful or distracting?
- What happens when both instance and fallback succeed at once?
- Are the ID-based `emoteX` commands understandable without a GUI?
- Does the guild copy fit in the selected server channel and its social etiquette?
- Is the occasional local reminder still welcome after longer sessions?

Keep these as questions until Babuni.dev tests and decides.
