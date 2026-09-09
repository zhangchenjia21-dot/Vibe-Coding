---
title: my world｜Model-curated Surface Visibility Preference
status: OWNER-APPROVED / DEFERRED
version: 1.0
created: 2026-09-09
updated: 2026-09-09
phase: G6 future information surfaces / Internal Dynamic UI convergence
owner: Owner + GPT
parent:
  - architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md
  - architecture/ui/G6_PEOPLE_CARD_VISIBILITY_PREFERENCE_DEFERRED_DECISION.md
implementation_authorization: deferred
---

# Model-curated Surface Visibility Preference｜玩家展示隐藏权

## 1. Product decision

Owner broadens the previously approved People-card hide concept into a general rule for model-curated information surfaces.

Frozen product principle:

> **Model decides what information is semantically worth retaining and presenting; Player has final control over which retained information units are actually visible in their own interface.**

Equivalently:

```text
semantic inclusion / retention
!=
presentation visibility
```

This separation is intentional. It reduces pressure to over-tune the model merely to match one player's preferred information density, while preserving model freedom and the player's ability to declutter their own UI.

## 2. Why this exists

The product should not solve every disagreement between model curation and player preference by adding more prompt rules, importance thresholds, classifiers or Program heuristics.

A model may reasonably judge an item worth retaining while the Player personally does not want to see it in the sidebar.

The correct resolution is often:

```text
Model keeps semantic judgment
+
Program keeps durable/current information
+
Player hides the presentation unit
```

This is a pressure-release valve for presentation preference, not permission for the model to overproduce low-value information. The model must still make its best semantic judgment; hide state must not be fed back as a signal that broad over-inclusion is acceptable.

## 3. Applicable surface class

This capability applies by default to **model-curated, player-facing information units** that are retained beyond the immediate Narrative and shown in information surfaces.

Examples include, where a stable renderable unit exists:

- People cards;
- Important Experience / milestone entries;
- future Open Threads / affairs entries;
- future Organization / Faction cards;
- future World Chronicle entries;
- other future model-curated cards/items using the same retention-and-presentation pattern.

Character may participate only at a sensible stable presentation granularity once the UI host has a real item/group identity model. Do not invent fragile text-based hide identity merely to force this feature into the current Character snapshot.

This decision does **not automatically apply** to authoritative live mechanics or factual operational state whose visibility is necessary for understanding play, such as Public d20 result cards, mandatory System state, factual Inventory state, blocking errors, or Debug diagnostics. Those require separate product decisions if hideability is ever desired.

Ephemeral recommendations are also not part of this retained-information visibility preference.

## 4. Hide semantics

When the Player hides a model-curated information unit:

- underlying semantic information remains intact;
- currentness / Timeline semantics remain intact;
- the model may continue to update the hidden information when later accepted play warrants it;
- hidden information may continue to be used by the model wherever that information was already a legitimate input;
- only ordinary player-facing rendering suppresses that unit.

Hide MUST NOT:

- delete/tombstone semantic information;
- erase World/People/Character/Thread/Chronicle truth or player-known state;
- remove stable actors or other domain entities;
- become negative importance evidence;
- be inserted into curation prompts as "the Player thinks this is unimportant";
- alter model semantic authority;
- trigger Provider calls;
- rewrite accepted Conversation or Timeline history.

## 5. Player authority and model freedom

Authority split becomes:

```text
Model
= semantic interpretation / curation / retention judgment

Program
= normalized storage / currentness / presentation plumbing

Player
= optional visibility preference in their own UI
```

A hidden item remains hidden until the Player explicitly restores it. A later model update, higher model-assessed importance, new information, or re-render must not automatically unhide it.

The hidden state is not evidence that the protagonist forgot, rejected, disliked, or considers the underlying information unimportant in-world.

## 6. Persistence / Timeline semantics

Visibility preference is a **Game-local presentation preference**:

- survives normal reopen / Continue;
- is not ordinary Timeline truth;
- should not rewind merely because the Player Restores an older Save;
- should not become displaced-future semantic state;
- remains independent from Regenerate/correction unless the underlying semantic item itself ceases to exist in current history.

If an underlying current item disappears because Timeline currentness removes it, no card is rendered regardless of hide state. If the same stable semantic item later becomes current again, the Player's visibility preference should remain associated with that stable item where a legitimate stable presentation identity exists.

Do not bind hide state to mutable display text or content hashes such that a normal model update accidentally creates a visibly "new" item and bypasses the Player's choice.

## 7. Recoverability

Every surface that supports hiding must also provide a clear bounded recovery path, such as:

- `已隐藏`;
- `显示已隐藏`;
- a surface-level hidden-items drawer/filter.

Exact UI is surface-specific and deferred, but hiding must never make an item permanently unreachable through the interface.

A useful default pattern is:

```text
visible card/item → 隐藏
surface header/menu → 已隐藏 (N)
hidden item → 恢复显示
```

## 8. Stable presentation identity

Hide preference requires a stable Program-owned presentation key tied to an existing legitimate semantic identity where available.

Examples:

- People → stable Game-local actor identity;
- Open Thread → stable thread identity when that domain exists;
- other cards → their legitimate Program/domain identity.

Do not use display-name equality or full rendered text as authoritative hide identity.

If a future model-curated surface has no stable item identity yet, solve that surface's real identity/currentness design first or implement hide at a coarser stable granularity. Do not create a parallel semantic truth system solely for visibility preferences.

## 9. Implementation placement

Do not interrupt active Package 2 Owner UAT or current core-closure work for this capability.

Preferred implementation timing: **Package 6 Internal Dynamic UI Host / information-surface convergence**, because this is naturally a cross-surface presentation primitive and should be implemented once rather than independently in each bespoke renderer.

At that stage, prefer one small presentation-preference owner used by eligible model-curated surfaces, without becoming a generic semantic EventBus, new curation engine, or universal UI platform beyond proven consumers.

An earlier bounded implementation is authorized only if real UAT shows information clutter itself is blocking core play.

## 10. Product acceptance direction

Future Owner acceptance should prove across at least two distinct model-curated surfaces:

- a visible item can be hidden;
- hide removes only presentation, not underlying semantic information;
- hidden information can continue updating without auto-unhide;
- hide survives reopen;
- Restore does not incorrectly rewind presentation preference;
- hidden items are recoverable;
- restoring visibility shows the then-current information, not a stale copy frozen at hide time;
- hide state never changes model curation behavior or causes extra Provider calls;
- authoritative mechanics / blocking state are not accidentally hidden by this generic mechanism.

## 11. Relationship to People-specific decision

`G6_PEOPLE_CARD_VISIBILITY_PREFERENCE_DEFERRED_DECISION.md` remains the People-specific specialization and first concrete example.

Where that file expresses generic visibility semantics, this cross-surface decision is now the broader authority. People-specific actor identity and People UI details remain governed by the People decision.
