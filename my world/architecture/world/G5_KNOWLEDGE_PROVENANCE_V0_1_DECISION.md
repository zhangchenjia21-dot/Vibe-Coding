# G5 Knowledge Provenance v0.1 Decision

Status: **FROZEN / PRE-IMPLEMENTATION ALIGNMENT**  
Phase: **G5 World Semantics & GM Runtime**  
First consumer: **G5-02 Knowledge Provenance**

## 1. Product purpose

G5-01 made accepted Narrative consequences durable. G5-02 now establishes a different semantic boundary:

```text
Game / World Truth
!= what a particular actor knows
!= what the human player has been shown
!= what the omniscient GM model may reference
```

The immediate product problem is simple:

> The GM model may need broad world truth to author the game, but an NPC must not behave as though it automatically knows every fact that the GM knows.

Protected principle:

> **GM omniscience must not become actor omniscience.**

G5-02 v0.1 is not a universal epistemic simulator.

## 2. Existing truth entering G5-02

Current Game-local setup already contains stable identities for:

- the Player Character (`local_character_id`);
- any selected Guaranteed NPCs (`local_character_id`).

Current Source semantic sections carry `disclosure` metadata, but current Game Context projects those sections to the GM as authoritative reference material. `gm_reference` means GM/reference visibility; it does **not** mean every actor automatically knows the section contents.

Current G5-01 also provides:

- durable accepted Conversation;
- `living_world.v0.1` turn consequences;
- one separate machine-analysis lane after accepted ordinary turns;
- one atomic world-mutation seam;
- matching bounded Context projection.

G5-02 must extend those seams rather than introducing a second world database or independent transcript.

## 3. v0.1 actor scope

Knowledge provenance v0.1 only assigns post-T0 knowledge to actors that already have a stable Game-local identity:

```text
Player Character
+
Guaranteed NPCs materialized in this Game
```

Do not create durable knowledge identity for:

- incidental unnamed people;
- newly improvised NPCs without a stable local actor identity;
- Factions;
- settlements/groups;
- arbitrary entities.

Those require later consumers (especially G5-03 actor/faction ownership) before generalization.

## 4. Starting knowledge versus post-T0 acquisition

G5-02M1 does **not** re-author or normalize the entire T0 Source into a knowledge graph.

Starting character/world Source remains GM reference and authored character grounding. G5-02 v0.1 first makes **post-T0 knowledge acquisition** durable.

Examples of post-T0 acquisition:

- an actor directly witnesses an event;
- an actor is explicitly told a fact;
- an actor discovers/reads/inspects evidence;
- an actor participates in an action whose outcome is established in the accepted Narrative.

Do not infer knowledge merely because:

- the fact exists in World truth;
- the GM knows it;
- the actor is a Guaranteed NPC;
- the actor is plausibly nearby but the Narrative did not establish awareness;
- external/source future canon contains it.

## 5. One analysis request, not a second Provider tax

G5-02M1 extends the existing independent G5-01 semantic-analysis request rather than adding a second Provider request per accepted turn.

Target machine shape is conceptually:

```json
{
  "changes": ["durable world consequence"],
  "knowledge_events": [
    {
      "knower_id": "local-character-id",
      "fact": "concise fact the actor now has grounds to know",
      "basis": "witnessed|told|discovered|participated"
    }
  ]
}
```

The exact implementation may use equivalent bounded names, but semantics are frozen:

- one auxiliary request per newly accepted ordinary turn version;
- `changes` retains G5-01 semantics;
- `knowledge_events` is post-T0 actor knowledge acquisition only;
- machine output remains non-player-visible;
- no raw prompt/response/reasoning is persisted.

## 6. Backward compatibility / failure isolation

G5-02 must not make G5-01 less reliable.

Required rule:

```text
knowledge parsing / validation failure
must not invalidate an otherwise valid G5-01 changes result
```

A valid `changes` payload must still be able to commit even if knowledge data is absent or invalid.

Conversely, knowledge extraction is best-effort in v0.1. Failure to materialize knowledge must not fail or regenerate the accepted Narrative.

No automatic recovery loop and no cross-provider fallback.

Existing historical turns are not retroactively re-analysed merely because G5-02 was installed.

## 7. Durable knowledge provenance record

Knowledge is stored inside the current game-local World document under `living_world`, adjacent to but distinct from G5-01 consequences.

Conceptual shape:

```text
living_world
  schema_version = living_world.v0.1

  semantic_turns_by_index
    ... existing G5-01 consequence records ...

  knowledge_turns_by_index
    <conversation_turn_index>
      knowledge_turn_id
      source_turn_index
      source_gm_sha256
      events[]
        knower_id
        fact
        basis
```

Each record is provenance-linked to the accepted GM Narrative version using the same source turn/hash principle as World Turn materialization.

A knowledge event is not a universal truth object. It means only:

> this stable actor has durable grounds to know this fact after this accepted turn.

No confidence score, belief probability, truth-maintenance graph, rumor network or inference engine in v0.1.

## 8. Stable actor validation

`knower_id` must resolve to the current Game's existing stable actor roster:

```text
player_character.local_character_id
or
guaranteed_npcs[*].local_character_id
```

Unknown/non-roster IDs are discarded from knowledge materialization and never become durable authority.

Do not match actors by free-form display name when a local ID exists.

## 9. Atomic persistence

A single accepted turn may produce:

- world changes only;
- knowledge events only;
- both;
- neither.

If there is material to persist, update the current world document and use the existing atomic `commit_world_mutation_durably(...)` seam.

One accepted semantic-analysis result creates at most one world mutation.

Do not introduce a new SQLite table/schema or second persistence owner.

## 10. Context consumer

The first real consumer is later GM continuation Context.

The GM may still receive broad Game-local World/Source truth needed to author the world. G5-02 does not try to make the GM itself non-omniscient.

Add a compact, bounded `Actor Knowledge Provenance` projection that communicates which post-T0 facts have durable provenance for the stable actors.

The GM instruction semantics are:

```text
You may know the full world as GM.
Do not make an actor speak, plan, react or decide from a post-T0 fact solely because it is present in GM/world context.
Use durable actor knowledge provenance, or knowledge newly established by the current scene, as the basis for actor awareness.
```

This is a narrative/semantic instruction, not an output validator.

Do not reject/retry Narrative because an automated classifier thinks an NPC knew too much.

## 11. Context bounds

Knowledge projection must be bounded from the first implementation.

A small recent-turn limit and character cap is acceptable. Do not linearly dump the entire knowledge history.

G7 owns long-session retrieval/working-set optimization.

## 12. Player Knowledge is not yet a separate UI database

The roadmap distinction `Character/NPC Knowledge != Player Knowledge` remains architecturally true, but G5-02M1 has no independent player-information UI consumer yet.

Therefore v0.1 does not create a separate "human player knowledge database" merely for completeness.

Player-safe disclosure/UI projection will be pulled by later Runtime→UI/G6 consumers.

## 13. Deception / false belief / uncertainty deferred

Do not implement yet:

- false beliefs;
- lies as believed facts;
- confidence/probability;
- source reliability scoring;
- rumor propagation;
- contradictory belief resolution;
- inference closure;
- Faction/shared knowledge;
- arbitrary relationship-based sharing.

When real G5-03/G5-04 consumers require them, extend from proven provenance records.

## 14. Provider validation

Real Provider validation remains bounded and pre-authorized under:

`architecture/foundation/REAL_PROVIDER_VALIDATION_STANDING_AUTHORIZATION.md`

For G5-02M1, after offline/integration gates are green, at most one task-owned real selected-Provider attempt is sufficient to inspect the combined machine response and later Context behavior.

If the selected Provider is externally unavailable, engineering reviewability is not blocked; record the real vertical as pending and push reviewable work/evidence.

No hidden Provider switch or fallback.

## 15. First reality proof

The first implementation should prove a simple asymmetry with stable existing actors, for example:

```text
Player Character and Guaranteed NPC A are present in the Game.
A private fact is established/discovered by the Player Character only.
→ durable knowledge provenance records Player Character as knower
→ NPC A receives no such provenance
→ later GM Context contains the asymmetric actor-knowledge boundary
→ Save/reopen preserves it
```

Then a later accepted Narrative may explicitly tell NPC A the fact:

```text
NPC A is told the fact
→ new knowledge event for NPC A
→ later Context now includes NPC A as a knower
```

The proof must not rely on keyword/style/output gates.

## 16. Explicit non-scope

G5-02M1 must not implement:

- general Entity/Knowledge Graph;
- emergent NPC identity platform;
- Faction knowledge;
- NPC/Faction autonomous Agency (G5-03);
- Event/Priority world evolution (G5-04);
- separate Player Knowledge UI;
- long-session retrieval platform (G7);
- Source schema migration;
- visual runtime/G6 surfaces.

## 17. Route

```text
G5-02M1 Known-Actor Knowledge Provenance Spine
→ GPT Independent Review
→ focused product/reality checkpoint only if needed
→ G5-02 closeout
→ G5-03 NPC / Faction Agency
```
