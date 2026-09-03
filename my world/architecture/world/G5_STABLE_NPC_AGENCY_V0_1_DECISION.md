# G5 Stable NPC Agency v0.1 Decision

Status: **FROZEN / PRE-IMPLEMENTATION ALIGNMENT**  
Phase: **G5 World Semantics & GM Runtime**  
Parent: **G5-03 NPC / Faction Agency**  
First consumer: **G5-03M1 Stable Guaranteed-NPC Independent Agency Vertical**

## 1. Product purpose

G5-01 established durable world consequences. G5-02 established that GM/world truth is not automatically actor knowledge.

G5-03 now asks the next product question:

> Can a stable non-player actor choose and carry out a meaningful action from its own identity and knowledge, without waiting for the player to explicitly trigger that actor?

Protected principle:

> **Source provides inertia; actors create history.**

This first slice proves one stable NPC actor. It is not yet a general society simulator.

## 2. v0.1 actor scope

The first agency consumer is **Guaranteed NPCs already materialized with stable Game-local `local_character_id`**.

Player Character is not an autonomous actor in this lane.

Do not create agency identity for:

- incidental/emergent NPCs without stable local identity;
- Factions;
- settlements/groups;
- arbitrary entities.

Faction agency remains inside parent G5-03 but is not pulled into M1 merely for symmetry. A concrete stable faction ownership/identity consumer must justify it later.

## 3. Actor-scoped cognition

An agency request must not simply receive omniscient GM Context.

For the selected Guaranteed NPC, build a bounded actor-scoped context from:

1. that actor's exact Game-local Source projection / selected T0 profile;
2. that actor's committed G5-02 post-T0 Knowledge Provenance only;
3. that actor's own recent durable agency actions/effects, if any;
4. minimal stable identity metadata needed to bind the request.

Do **not** expose another actor's private knowledge merely because the GM can see it.

Do **not** dump the complete GM/world Source Context into the actor request as a shortcut.

The actor may of course know starting facts authored into its own Character Source; G5-02 did not normalize all T0 biography into a knowledge graph.

## 4. Background, non-blocking agency lane

NPC agency is a background/best-effort lane.

Protected ordering:

```text
Player action
→ free-form GM Narrative
→ durable Conversation acceptance
→ G5-01/G5-02 semantic lane reaches a terminal state
→ optional stable-NPC agency evaluation
→ optional atomic agency world mutation
```

The agency lane must **not** become a Turn Finalize Barrier for ordinary play.

The player may start the next action without waiting for NPC agency.

If the player starts another Conversation generation while agency work is queued/active, stale agency work must be cancelled/invalidated rather than delaying the foreground turn.

Provider/format/empty/no-action failure is fail-soft: no fake agency mutation and no Narrative failure.

No retry loop and no Provider fallback.

## 5. One actor evaluation per source turn

For v0.1, at most one Guaranteed NPC is evaluated for one accepted ordinary source turn.

If there are multiple Guaranteed NPCs, use a simple deterministic fair selection such as round-robin by stable roster order + source turn index. This is only an **evaluation scheduler**, not a claim that the selected NPC must act.

The selected actor may return `hold` / no meaningful action.

Do not build priority-driven scheduling here; G5-04 owns pressure/priority/event scheduling.

If there are no Guaranteed NPCs, no agency Provider request occurs.

## 6. Agency Provider request

Agency uses a separate background Provider request because it is **new world authorship**, not extraction of the already accepted Narrative.

It may occur only after the existing semantic lane for the source turn reaches a terminal state so the actor can see the latest committed knowledge/world result available to it.

Conceptual response:

```json
{
  "actor_id": "exact-local-character-id",
  "decision": "hold|act",
  "intent": "concise current aim",
  "action": "concise action the actor now undertakes",
  "effects": ["immediate world effect already established by undertaking the action"]
}
```

Rules:

- `actor_id` must equal the selected stable local actor ID;
- `hold` carries no durable action/effects;
- `act` requires bounded non-empty intent/action;
- effects are bounded immediate consequences that are already established by the action;
- do not turn uncertain future success into a guaranteed result merely to fill `effects`;
- no reasoning output;
- no narrative prose format requirement because this is a separate machine lane.

G5-05 owns mechanics integration; G5-03M1 must not invent hidden d20/check semantics.

## 7. Durable agency record

A valid `act` result becomes Game-local world truth under `living_world`, conceptually:

```text
living_world
  agency_turns_by_source_turn
    <conversation_turn_index>
      agency_turn_id
      source_turn_index
      source_gm_sha256
      source_head_id
      actor_id
      intent
      action
      effects[]
      materialized_at
```

Stable identity must be tied at minimum to:

`game_id + source_turn_index + accepted GM hash + source_head_id + actor_id`.

A `hold` result creates no world mutation in v0.1.

Do not add a SQLite schema/table; reuse the existing world snapshot + atomic `commit_world_mutation_durably(...)` seam.

## 8. Agency effects as world truth

Agency `effects[]` are durable GM/world truth and may enter later ordinary GM Context through a bounded projection, for example:

```text
## Independent Actor Actions
孙权 [local-id]
- decided: ...
- action: ...
- world effects: ...
```

This projection is GM reference, not automatic actor knowledge and not automatic human-player disclosure.

Do not automatically copy agency effects into every actor's G5-02 knowledge ledger.

The acting NPC's own prior agency history may be included in its future actor-scoped agency context so it can maintain continuity with its own actions.

Other actors learn about the action only when later Narrative/knowledge provenance actually establishes awareness.

## 9. Stale-work / timeline safety

Agency work is more disposable than foreground Narrative.

At agency request start, capture at least:

- current `active_head_id`;
- accepted source turn index/hash;
- current accepted Conversation count/version;
- an agency epoch/generation token if useful.

Before committing an agency result, require that the request still belongs to the same current state.

At minimum, an old agency result must not commit if:

- the player has started a new Conversation generation;
- another accepted turn has advanced Conversation;
- current world `active_head_id` changed after the agency request snapshot;
- Save Restore / Recovery switched the current timeline;
- the source accepted GM hash is no longer current;
- the Game/session is closing.

Use existing Conversation attempt signals and Runtime `restore_completed` seam. Cancellation may be best-effort, but late callbacks must still be unable to commit after invalidation.

This is a real M1 invariant because agency is intentionally asynchronous and may race foreground play.

## 10. No foreground gate

Never implement:

```text
player input
→ wait for NPC agency Provider
→ only then allow next player turn
```

The product requirement is the opposite:

```text
NPC agency can enrich world progression
without turning Provider latency into a player-input lock
```

If a background agency attempt loses the race to the player's next action, dropping that agency attempt is acceptable in v0.1.

## 11. First deterministic proof

Use a Game-local stable Guaranteed NPC with authored Source identity and G5-02 knowledge.

Example:

```text
Player Character = 刘备
Guaranteed NPC = 孙权

Durable Knowledge Provenance says only 孙权 knows fact F.
→ agency request for 孙权 contains Sun Quan Source + Sun Quan knowledge F
→ request does not contain Player-only private fact P
→ controlled model returns a valid independent action based on F
→ one atomic agency mutation commits
→ later GM Context contains the actor action/effects
→ Save/reopen preserves it
```

Also prove:

- no Guaranteed NPC → no agency request;
- `hold` → no mutation;
- unknown/wrong actor ID → no mutation;
- malformed/provider failure → fail-soft;
- starting next player turn cancels/invalidates active agency and prevents late commit;
- Restore invalidates active agency and prevents late commit;
- same committed source version does not duplicate agency action;
- no cross-actor private knowledge leakage into the actor-scoped request.

## 12. Real Provider validation

Standing authorization applies:

`architecture/foundation/REAL_PROVIDER_VALIDATION_STANDING_AUTHORIZATION.md`

After all deterministic/integration gates are green, at most **one** task-owned real selected-Provider agency attempt may be made.

Use stubbed/task-owned setup for Opening/foreground prerequisites where useful so the bounded real call is spent on the feature-specific agency request rather than on unrelated Narrative availability.

A feature-specific real PASS requires a valid stable-actor agency response and durable agency record/effect. `hold`, malformed output, or Provider timeout is inconclusive/pending rather than fabricated PASS.

No fallback, hidden provider switch or second attempt.

## 13. Explicit non-scope

G5-03M1 does not implement:

- Faction identity/agency;
- priority/event scheduler (G5-04);
- universal actor graph;
- relationship engine;
- inventory/economy simulation;
- movement/pathfinding/GIS;
- false belief/rumor propagation;
- mechanics/d20 integration (G5-05);
- player-facing agency UI (G5-06/G6);
- long-session retrieval platform (G7).

## 14. Route

```text
G5-03M1 Stable Guaranteed-NPC Agency Vertical
→ GPT Independent Review
→ if mechanism/consumer is sound, decide whether G5-03 needs a second Faction slice before closeout
→ G5-04 Event / Priority-driven World Evolution
```
