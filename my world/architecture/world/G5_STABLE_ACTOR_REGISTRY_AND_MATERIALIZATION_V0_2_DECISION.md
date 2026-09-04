# G5 Stable Actor Registry + Materialization v0.2 Decision

Status: **FROZEN / CURRENT CANONICAL**  
Date: 2026-09-04  
Stage: G5-03M2  
Supersedes: `G5_STABLE_NPC_MATERIALIZATION_V0_1_DECISION.md`

## 1. Owner product correction

A character must not need a pre-authored Character Card in order to become a durable NPC in the current Game.

The protected product rule is:

> **A character becomes a stable NPC because the Game has established that individual as a persistent actor, not because the Source Library happened to contain a Character Card.**

G5-03M2 therefore owns a Game-local Stable Actor Registry with multiple ingress paths.

## 2. Required NPC origins

The same Game-local stable-actor system must be able to represent all of these:

1. **Guaranteed Source NPC** — existing explicit Guaranteed NPC behavior; already supported and remains distinct as a product role.
2. **Automatic Source-backed NPC** — a Character Card compatible with the selected exact World+Entry even when the player did not explicitly add it as Guaranteed.
3. **Creation-authored Game-local NPC** — a character explicitly created/enabled for this Game without any Character Card.
4. **Runtime Narrative-materialized NPC** — a distinct character established during accepted play who becomes continuity-relevant and is promoted from incidental narrative presence into a stable Game-local actor.

A Character Card is therefore one source of actor material, not a prerequisite for actor identity.

## 3. Unified stable actor semantics

All non-player stable NPC origins expose one Program-owned runtime view:

```text
stable_npc_records(current world + current Conversation identities)
= Guaranteed NPCs
+ current creation-time stable_npcs
+ current runtime-materialized stable_npcs

actor_roster
= Player Character + stable_npc_records

Agency eligible pool
= stable_npc_records only
```

Registry membership grants:

- stable Game-local identity;
- durable actor material;
- eligibility to receive Knowledge Provenance events;
- eligibility for Agency selection.

Registry membership does **not** grant:

- automatic knowledge of world facts;
- automatic action;
- Player disclosure;
- faction membership or world-simulation semantics.

## 4. Program-owned identity

Every stable NPC receives a Program-owned `local_character_id`.

Never use display name as authoritative identity. Two characters may share a name. A model may describe or propose a new actor, but it never mints the authoritative ID.

### 4.1 Source-backed identity/material

Source-backed records keep:

```text
local_character_id
+ exact Source provenance
+ frozen Character T0 source_projection
```

The existing Guaranteed-NPC pattern remains the template.

### 4.2 Game-local identity/material

A no-Card actor stores bounded Game-local character material instead of fake Source provenance:

```json
{
  "local_character_id": "Program-owned ID",
  "role": "stable_npc",
  "origin": {
    "kind": "creation_authored | runtime_narrative"
  },
  "game_local_material": {
    "display_name": "...",
    "profile_text": "bounded established character material"
  }
}
```

Runtime-origin records additionally bind their origin to the accepted Narrative version:

```text
source_turn_index
source_gm_sha256
```

Do not manufacture fake `asset_id`, `generation_fingerprint`, or Source provenance for Game-local actors.

## 5. Game-local storage

Keep `game_local_setup.v0.1`; no new SQLite table/schema is required.

Use the optional additive top-level collection:

```text
stable_npcs[]
```

It may contain Source-backed and Game-local stable NPC records. `guaranteed_npcs[]` remains separate because Guaranteed is a product role.

Existing Games with no `stable_npcs` remain valid:

```text
missing stable_npcs
→ []
→ no Source lookup
→ no retrofit
→ no failure
```

## 6. Creation-time ingress

### 6.1 Automatic Source-backed NPCs

On the first creation-intent build:

- inspect validated Character inventory once;
- use the existing exact T0 projection contract against selected exact World+Entry;
- materialize only `exact_profile` Character Cards;
- exclude Player and explicit Guaranteed Character asset IDs;
- deterministically order the snapshot;
- assign Program-owned Game-local IDs;
- freeze exact provenance + T0 projection into `initial_setup.stable_npcs`.

Same `creation_id` retry/resume reuses the frozen intent and must not rescan later Source current.

### 6.2 Creation-authored no-Card NPCs

Final Create gains an **optional additive** Game-local NPC input. Missing input means `[]` and must preserve all existing Composition callers.

The backend contract only needs bounded character material sufficient for continuity, conceptually:

```json
{
  "display_name": "陈安",
  "profile_text": "江夏粮商；本局建局时已确定的身份、关系或动机材料。"
}
```

The Program assigns local IDs during first intent creation and freezes those records into `initial_setup.stable_npcs` with `origin.kind = creation_authored`.

This task does not require a new UI surface. Future Creator/UI producers may populate this optional field without changing registry semantics.

## 7. Runtime Narrative materialization

Runtime-created NPCs are a required G5-03M2 capability, not a deferred optional idea.

### 7.1 Do not materialize every incidental mention

A Narrative mention may remain ephemeral. A stable materialization is appropriate when the accepted Narrative establishes a **distinct individual with plausible continuity relevance** — e.g. an ongoing relationship, obligation, role, possession, conflict, allegiance, recurring involvement, or other reason the Game may need to remember and independently act through that person.

No keyword gate, score threshold, or first-mention requirement is introduced. A person may remain incidental on first appearance and be promoted on a later turn when continuity becomes clear.

### 7.2 Reuse the existing semantic lane

Do **not** add a mandatory extra Provider call.

Extend the existing post-Narrative semantic-analysis response with an optional, independently fail-soft field conceptually:

```json
{
  "changes": [],
  "knowledge_events": [],
  "new_actor_candidates": [
    {
      "display_name": "陈安",
      "profile_text": "Only character material established by the accepted Narrative."
    }
  ]
}
```

Rules:

- malformed/absent `new_actor_candidates` must not invalidate Narrative, valid `changes`, or valid `knowledge_events`;
- the model describes candidates but never supplies the authoritative local ID;
- the request supplies the current stable actor roster so the model is instructed not to propose an already-stable actor as new;
- Program-side authoritative identity remains exact local ID after materialization;
- display-name matching is never used as the authoritative dedupe mechanism.

A bounded extraction safety ceiling is allowed (recommended v0.2 ceiling: 8 candidates per semantic turn). This is not a world-semantic actor limit; overflow is fail-soft and persistent characters may be materialized later.

### 7.3 Atomic semantic commit

Valid new actor materializations from a semantic result are committed through the existing single durable world-mutation seam **together with** that turn's valid `changes` / `knowledge_events` when applicable.

Do not create a second semantic mutation merely for actor registration.

A runtime actor ID must be derived/minted by Program logic from Game + accepted source-version + bounded candidate material, with deterministic replay behavior for the same accepted version.

After this semantic commit completes, the existing v0.3 semantic-terminal wake may immediately let the standalone Agency Selector see the newly materialized actor in that same player-turn opportunity.

## 8. Regenerate / correction currentness

Runtime-materialized actors are versioned by their accepted Narrative origin.

A runtime `stable_npcs[]` record carries:

```text
origin.kind = runtime_narrative
origin.source_turn_index
origin.source_gm_sha256
```

Program-owned stable-actor enumeration must receive/use the current accepted Conversation turn→GM-hash identities:

- creation-time actors are always current;
- a runtime actor is current only while its origin turn still has the matching accepted GM hash;
- regenerate/correction leaves stale physical history inert rather than letting an erased character enter Knowledge or Agency.

Do not delete history merely to obtain currentness.

## 9. Material normalization for actor execution

Actor execution must work for both material families without pretending they are the same provenance type.

Use/reuse one helper conceptually:

```text
stable_actor_material(record)
→ source_projection      for Source-backed actor
→ game_local_material   for no-Card Game-local actor
```

The actor request still receives only:

- that actor's own material;
- that actor's own committed Knowledge;
- that actor's own recent Agency history;
- minimal current cycle identity.

No actor receives another actor's private knowledge.

## 10. Knowledge boundary for a newly materialized actor

`game_local_material` may contain the actor's own identity/role/background facts that the accepted Narrative explicitly established when the actor came into existence.

It is not a substitute for G5-02 Knowledge Provenance. Other world facts are not automatically granted because an actor was materialized.

Because the model does not know the Program-minted ID before commit, same-turn `knowledge_events` are not required to target a just-created actor. On later turns the actor appears in the exact stable roster and may receive normal G5-02 events.

## 11. Execution slices

To keep implementation small and reviewable, G5-03M2 is mandatory but split:

```text
G5-03M2A Stable Actor Registry Foundation
→ Source-backed automatic snapshot
→ creation-authored no-Card NPC ingress
→ unified identity/material/currentness helpers
→ existing Knowledge/Agency consumers understand both material families

then Independent Review

G5-03M2B Runtime Narrative Actor Materialization
→ optional new_actor_candidates in existing semantic lane
→ Program-minted runtime identity
→ atomic semantic commit
→ current-hash filtering
→ same-turn visibility to later Agency

then Independent Review
```

M2 is not complete until **both M2A and M2B PASS**.

## 12. Protected boundaries

M2 MUST NOT:

- require a Character Card for a character to become a stable NPC;
- use display-name equality as authoritative identity;
- let the model mint authoritative actor IDs;
- create fake Source provenance for Game-local actors;
- read mutable Source current during ordinary gameplay, Continue, Restore, selector, or actor execution;
- make new-actor extraction a Narrative acceptance gate;
- add a separate mandatory runtime actor-discovery Provider call;
- automatically turn every incidental named person into a permanent actor;
- grant knowledge or action merely from registry membership;
- introduce a universal entity ontology/simulation engine;
- add SQLite schema/table/migration solely for registry expansion;
- implement Faction agency or G5-04;
- change Public d20 or mechanics.

## 13. Validation policy

Use focused-first, risk-based validation.

M2A requires zero real Provider calls.

M2B should use deterministic semantic stubs for acceptance. It adds no new Provider call beyond the already-existing semantic lane; parent real G5-03 Provider proof remains honestly pending if external Provider evidence is unavailable.

Do not restore a full-project regression matrix without a concrete cross-system reason.

## 14. Product completion criterion

After M2B, this must be possible without a pre-authored Character Card:

```text
Accepted Narrative establishes a new persistent person
→ background semantic lane recognizes the person as continuity-relevant
→ Program materializes a stable Game-local NPC identity
→ Save/Restore keeps that identity
→ later Knowledge can target the exact ID
→ later Agency can independently select and act through that NPC
```

This is a core G5 living-world capability.
