# ROARemoteTrigger — verification and decisions roadmap

**Creator:** Babuni.dev  
**Not an implementation approval:** This document records test plans and options; no executable file was changed.

## Current implementation baseline

- Installable **ROARTrigger** 1.32, Wrath 3.3.5 / `Interface 30300`.
- Multiple numbered configurations with per-instance chance, cooldown and emote IDs.
- Shared master emote list with initial ROAR and per-character `ROGUDB.profiles[name-realm]`.
- `hooksecurefunc("UseAction", ...)` when available; fallback wrapper executes original action before addon reaction.
- Independent fallback: default **5/1000**, gated after success for **2s**, only for action slots 1–200.
- Reminder after **420s** since last recorded roar, **73s** repeat throttle, checked on action activity.
- Lifetime and last-hour emote statistics, with periodic action-triggered reporting.
- `/rogu invite` uses **33** guild-text alternatives; **no Roarboard** feature.
- No automated test suite or in-game verification recorded in this audit.

## Priority 0 — installation, action hook and compatibility

1. Install source in `Interface/AddOns/ROARTrigger/` under a genuine target Wrath 3.3.5 client, and test TOC recognition, login, reload and slash command registration.
2. Verify whether `hooksecurefunc` exists and is permitted for the exact client distribution; record which hook branch executes.
3. Confirm the normal `UseAction` still runs and that a failed spell attempt can trigger an emote; do not describe source inspection as proof that the hook works.
4. Check load order/interactions with other `UseAction` hooks (RoarGuild, ROARRunlimited and any installed action-based addons), including potential double responses.
5. Verify an action slot can produce **both** its configured instance emote and independent fallback on one press.

## Priority 1 — persistence and user commands

6. Run `/rogu watch`, `slot1`, `chance1`, `timer1`, `emote`, `emote1`, `info`, `resetcd`, `reset`, `on/off` and manual `roar`.
7. Test chance 0/100, fallback chance 0/1000, short/long timers, invalid or fractional slot/instance inputs, and multiple instances watching one slot.
8. Confirm that a **failed instance chance** consumes its interval, but a **failed fallback chance** does not advance `fallback.last`.
9. Test initial login timing gates and persisted configurations, plus profile switching across characters/realms and one-time migration of legacy root `ROGUDB` fields.
10. Verify hourly statistic reports, rolling counts, and the action-triggered seven-minute reminder without assuming background delivery.
11. Test manual `/rogu roar` and unsupported emote tokens. Check whether `/rogu off` leaves manual actions available as source suggests.
12. Verify that the `invite` numeric channel matches the intended chat before sending text; avoid broadcasting during test setup.

## Priority 2 — narrative and usability questions

13. Compare this client adaptation to the historically adjacent `RoarGuild` repo at source/commit level and document what is shared and what is different without inventing a merge history.
14. Decide whether per-instance slot discovery and emote-ID configuration are sufficiently learnable.
15. Review whether runtime state and last-hour reporting fit the non-performance character-expression direction.
16. Confirm the 33 recruitment messages are appropriate for target-server channel and character limits.

## Open choices for Babuni.dev, not approved changes

- Should both auto paths be allowed to emit on the same button press?
- Should a user have a dedicated toggle for fallback or reminders (currently defaulted/limited by commands)?
- Should per-instance emote IDs be easier to manage without a full GUI?
- Should the project ever merge back into RoarGuild, or stay a separate Wrath adaptation?
- What concrete client versions/servers should the user-facing README explicitly support after actual tests?

No milestones are declared “done” merely because the Lua source contains their intended implementation.
