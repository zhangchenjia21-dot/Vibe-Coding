# G5 Player-Safe Runtime → UI Projection v0.1 Decision

Status: **FROZEN / CURRENT**  
Phase: **G5 World Semantics & GM Runtime**  
Capability: **G5-06 Runtime → UI Projection**  
First executable Work Item: **MW-009 Player-Safe Runtime Side Panels**  
Owner progression decision: **2026-09-05 — start G5-06 immediately; defer remaining G5-05 product UAT to later combined testing**

## 1. Product purpose

G5 has accumulated real durable Runtime truth: world consequences, actor knowledge provenance, independent actor actions, selective world evolution and Public-d20 mechanics. The next risk is no longer “can the backend represent a living world?” but:

> **Can the product expose useful Runtime state to the player without leaking omniscient GM truth, private NPC knowledge, or internal machinery?**

G5-06 is therefore the first real player-safe consumer of G5 Runtime semantics.

The purpose is not to build the full G6 RPG interface. It is to establish the minimum trustworthy projection boundary that later UI can depend on.

## 2. Core rule

```text
Runtime truth
!= GM-visible truth
!= actor-private knowledge
!= human-player-safe UI projection
```

The UI must not receive the whole omniscient `world_state` and then decide what not to show.

Protected rule:

> **Disclosure is owned by the projection boundary, not by presentation widgets.**

A UI widget may format an already-safe projection, but it must not inspect raw World Evolution, Agency, NPC Knowledge or GM-reference material and attempt to filter it itself.

## 3. First vertical

Use the already-existing left/right gameplay side panels as the first consumer:

```text
Player panel
→ safe Player Character identity

World panel
→ safe World / selected Entry identity
→ bounded recent post-T0 facts durably known by the Player Character
```

This is deliberately small. It proves that real Runtime truth can cross into UI under an explicit disclosure contract.

## 4. Why Player Character knowledge is the first dynamic slice

G5-02 explicitly deferred a separate human-player knowledge database until a real UI consumer existed. G5-06 is that consumer.

However v0.1 still does **not** create a new Player Knowledge database.

Instead, the first projection uses a conservative safe subset:

```text
Player Character durable post-T0 Knowledge Provenance
⊆ safe information that may be shown to the human player
```

This is not a claim that human-player disclosure and Player Character knowledge are universally identical. It is only a safe first consumer.

If a later UI needs omniscient cutaways, journaled player-only discoveries, map fog-of-war, quest knowledge or out-of-character disclosure, that later consumer must pull the next smallest capability rather than generalizing prematurely.

## 5. Safe static identity projection

The first UI may show only player-safe identity metadata already selected into the frozen Game:

### Player Character

- display name;
- selected profile display name if one exists and is already part of the selected local Character projection.

Do not expose:

- internal local IDs;
- Source generation fingerprints;
- raw `gm_reference` semantic sections;
- hidden/private Source material merely because it exists in the selected Character.

### World

- World display name;
- selected Entry display name if one exists.

Do not expose:

- Source asset IDs/fingerprints;
- future/unselected Entry material;
- raw World semantic sections;
- literary style reference text;
- GM instructions.

## 6. Dynamic known-fact projection

The first dynamic projection reads only current, valid Player Character knowledge provenance.

Required semantics:

```text
living_world.knowledge_turns_by_index
→ validate current living_world schema
→ validate knowledge record shape
→ require current accepted Conversation turn/hash match
→ require knower_id == current Player Character local_character_id
→ take bounded recent facts
→ return display-only fact strings
```

The projection must fail closed. Invalid, stale, unknown or ambiguous data produces fewer/no displayed facts rather than falling back to raw World truth.

A small bound such as the latest 6–8 facts is appropriate. Keep deterministic time ordering and avoid Provider summarization.

Exact duplicate fact strings may be de-duplicated conservatively, keeping the newest occurrence, if that improves readability. Do not invent semantic similarity or clustering.

The `basis` field is provenance metadata, not required player-facing copy in v0.1.

## 7. Explicit exclusions

The first player-safe projection must **not** directly expose:

- `semantic_turns_by_index` raw World consequences unless separately represented as Player Character knowledge;
- `agency_cycles_by_source_turn` or independent actor private actions;
- `world_evolution_events_by_turn`;
- NPC knowledge provenance for any other actor;
- omniscient GM Context blocks;
- Style Primer / literary reference;
- Public-d20 internal proposal/control payloads;
- durable mutation IDs, hashes, local IDs, fingerprints or persistence metadata.

This matters even if the current UI would “probably not render” those fields. They must not be present in the player-safe projection object.

## 8. Currentness / timeline semantics

Player-safe UI must reflect the same current timeline as the rest of the Game.

Therefore dynamic facts must be validated against current accepted Conversation hashes, following the same currentness principle already used by G5 World Context projection.

Required behavior:

```text
accepted turn + valid Player Character knowledge event
→ fact may display

Regenerate/replacement making the old source hash stale
→ old fact must not display

Restore to a pre-fact Save
→ restored-away fact must disappear

reopen current Game
→ same safe projection reconstructs from durable state
```

Do not delete stale physical history merely for UI. Projection decides currentness.

## 9. Refresh seam

Do not create a generic reactive-state platform unless required.

For v0.1, refresh only at real lifecycle points already present:

- initial Game activation / reopen;
- successful G5-01 semantic lane terminal, because Player Character knowledge can change there;
- successful Restore/progress switch;
- explicit full side-panel redraw if the existing Shell lifecycle already performs one.

Current G5-03 Agency and G5-04 World Evolution mutations are intentionally **not** direct refresh-worthy disclosure events because those truths are not automatically player-safe.

If implementation evidence proves the existing lifecycle cannot refresh safely without a small Runtime signal, add the narrowest signal that represents “current world snapshot changed durably”; do not build a generic observable store.

## 10. UI consumer shape

Use the existing gameplay side panels rather than creating a new screen.

Recommended v0.1 content:

```text
左侧「主角」
<角色显示名>
<已选身份/档案名，可选>

右侧「世界」
<世界显示名>
<当前 Entry 显示名，可选>

主角所知
- fact 1
- fact 2
...
```

When no post-T0 Player Character knowledge exists, use a quiet empty-state sentence rather than exposing internal absence details.

Do not add sorting controls, tabs, filters, search, pinning, journal editing or a general ViewModel framework in this vertical.

## 11. Presentation vs authority

UI text is a projection only.

```text
player-safe projection
!= new World truth
!= new Knowledge truth
!= new persistence owner
```

No UI action may mutate knowledge in v0.1.

No Provider call is needed to build the projection.

## 12. First reality proof

The first implementation must prove a disclosure asymmetry with real current Runtime structures:

```text
World contains:
- one Player Character knowledge fact A
- one NPC-only knowledge fact B
- one independent actor action C
- one World Evolution event D
- one raw durable world consequence E

player-safe side panel projection
→ shows A
→ does not contain B / C / D / E unless a separate Player Character knowledge event explicitly carries that information
```

Then prove:

```text
Restore to before A
→ A disappears

Reopen after A
→ A returns consistently
```

This is the core G5-06 privacy/value proof.

## 13. Non-scope

Do not implement in G5-06 v0.1:

- full G6 Character Sheet;
- Inventory, Relationship, Faction, Quest, Journal or Map systems;
- visual asset runtime / portraits / scene art / authored-map resolution;
- a universal Player Knowledge database;
- a universal ViewModel/event-bus framework;
- a World/Entity/Knowledge Graph;
- editable player journal;
- omniscient world dashboard;
- raw NPC/world-event debug panels in production UI;
- model-generated summaries;
- new persistence schema/tables;
- long-session retrieval/summarization (G7).

## 14. Route

```text
MW-009 Player-Safe Runtime Side Panels
→ GPT Independent Review
→ bounded Owner/UAT checkpoint if needed
→ G5-06 closeout or one additional consumer only if the first real UI proves a missing abstraction
→ G5-07 World Product Tests
→ G5-GATE
```

Vertical before platform remains mandatory. Do not use G5-06 to pre-build the full G6 presentation architecture.
