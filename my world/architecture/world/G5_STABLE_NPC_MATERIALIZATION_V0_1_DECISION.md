# G5 Stable NPC Materialization v0.1 Decision

Status: **FROZEN / CURRENT CANONICAL**  
Date: 2026-09-04  
Stage: G5-03M2

## 1. Product problem

G5-03M1 proved multi-actor Agency, but its bootstrap candidate pool is `guaranteed_npcs[*]`. That is not a permanent product boundary. Important world actors such as Cao Cao or Sun Quan may need independent Agency even when the player did not explicitly choose them as Guaranteed NPCs.

G5-03M2 expands the Game-local stable NPC pool without introducing display-name identity, model-authored authority, or runtime dependence on mutable Source Library `current`.

## 2. Core rule

A stable NPC is a **Game-local materialized actor with Program-owned identity and frozen exact Source/T0 material**.

Reuse the existing Final Create identity/materialization pattern already used by Guaranteed NPCs:

```text
Program-owned local_character_id
+ exact Source provenance
+ frozen Character T0 source_projection
```

`Guaranteed NPC` remains a distinct product role. Stable NPC expansion must not relabel automatically available world actors as guaranteed.

## 3. Creation-time snapshot policy

On the **first creation-intent build**, before the first durable Game side effect:

1. Resolve the selected exact World generation and Entry as today.
2. Read the validated current Source inventory once for this new Game creation.
3. Consider Character Card generations only.
4. For each Character Card, call the existing exact T0 projection contract against the selected World asset ID + Entry ID.
5. Materialize as an automatic stable NPC only when `compatibility_state == exact_profile`.
6. Exclude Character asset IDs already used by the Player Character or explicit Guaranteed NPCs.
7. Deterministically sort remaining records by exact Source identity before assigning Game-local IDs.
8. Freeze exact provenance + T0 projection into the creation intent's `initial_setup`.

Do not auto-materialize:

- `temporal_incompatible` Character Cards;
- `no_world_coverage` Character Cards;
- arbitrary Source files discovered by name/prose;
- model-suggested names without an exact materialized Source identity.

The automatic registry has no arbitrary world-semantic cardinality cap. The existing Agency selector output safety ceiling (0..4 actors per evaluation) remains separate.

## 4. Retry / resume authority

Once a creation intent exists, its frozen `initial_setup` is authoritative for that creation ID.

```text
first creation attempt
→ capture stable NPC snapshot
→ persist creation intent

later retry/resume of same creation_id
→ reuse exact initial_setup
→ DO NOT rescan current Source Library
```

A Source Library update after that snapshot must not silently alter the in-flight or created Game.

## 5. Durable Game-local shape

Keep `game_local_setup.v0.1` and add an optional additive field; no new SQLite table/schema is required:

```text
game_local_setup.v0.1
  player_character
  guaranteed_npcs[]
  stable_npcs[]        # optional additive G5-03M2 field
  world
  expansions[]
  living_world
```

Each `stable_npcs[]` record conceptually contains:

```json
{
  "local_character_id": "program-owned Game-local ID",
  "role": "stable_npc",
  "provenance": {
    "asset_id": "...",
    "asset_type": "character_card",
    "version": "...",
    "generation_fingerprint": "..."
  },
  "source_projection": {
    "identity": {},
    "display_name": "...",
    "selected_profile": {},
    "semantic_sections": []
  }
}
```

Equivalent additive representation is acceptable if it preserves these semantics.

## 6. Existing Game compatibility

For an existing Game with no `stable_npcs` field:

```text
missing stable_npcs
→ treat as []
→ no error
→ no Source lookup
→ no retrofit
→ existing player + Guaranteed NPC behavior remains exact
```

**Never patch an existing Game by reading mutable Source Library current.**

## 7. Unified stable actor consumption

Introduce/reuse one Program-owned helper equivalent to:

```text
stable_npc_records(world_state)
= guaranteed_npcs + stable_npcs
```

with stable local IDs and duplicate rejection.

Consumers:

- `actor_roster(world_state)` = Player Character + all stable NPC records;
- G5-02 `Allowed Stable Actors` therefore expands to the new stable NPCs;
- Agency selector eligible pool = all stable NPC records, Player excluded;
- Agency actor Source lookup = all stable NPC records;
- own Knowledge / own Agency History isolation remains unchanged.

Registry membership grants identity/material, **not knowledge** and **not an action**. The selector still decides whether an actor is relevant enough to evaluate.

## 8. Protected boundaries

M2 MUST NOT:

- use free-form display-name matching as authoritative identity;
- let the model mint authoritative actor IDs;
- read Source Library current during ordinary gameplay, Continue, Save/Restore, replay, selector, or actor execution;
- merge automatic stable NPCs into `guaranteed_npcs`;
- introduce a universal entity/actor ontology;
- create a new SQLite table or migration solely for the registry;
- implement faction agency or G5-04 world evolution;
- grant stable NPCs knowledge merely because they exist in the registry.

## 9. Validation policy

M2 is deterministic identity/materialization work. **Zero real Provider calls are required.**

Focused proofs must cover:

1. new Game creation snapshots extra exact-profile Character Cards while excluding Player/Guaranteed duplicates, temporal-incompatible and no-world-coverage cards;
2. exact provenance + T0 projection are frozen;
3. retry/resume of the same creation ID does not rescan changed Source current;
4. old Game without `stable_npcs` remains valid and does not consult Source;
5. unified roster exposes Player + Guaranteed + stable NPCs to Knowledge Provenance while Agency excludes Player;
6. a selected stable NPC can use the existing actor-scoped Agency execution path with its own frozen Source/Knowledge/history;
7. unknown/unmaterialized IDs remain rejected;
8. Save/reopen/Restore preserve the registry as normal Game-local world state.

Use focused-first, risk-based regression; do not turn M2 into a full-project test gate.

## 10. Next boundary

After G5-03M2 Independent Review, GPT decides whether a remaining Faction agency slice is needed before closing G5-03. Do not start G5-04 early.
