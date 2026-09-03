# G5 World Turn / Semantic Materialization v0.1 Decision

Status: **FROZEN / PRE-IMPLEMENTATION ALIGNMENT**  
Phase: **G5 World Semantics & GM Runtime**  
First consumer: **G5-01 Minimum Playable T0 + World Turn / Semantic Materialization**

## 1. Product purpose

G4 proved that Source-grounded Games can be created, played, saved, reopened and switched. G5 begins the transition from a durable AI conversation to a durable living world.

The first question is deliberately narrow:

> After the GM visibly narrates that something meaningful has actually changed, can the Runtime preserve that change as game-local semantic reality without turning free-form Narrative into a machine protocol?

Long-term principle remains:

> **Model authors the world; Runtime makes it durable; Player owns the timeline.**

G5-01 does **not** attempt full world simulation.

## 2. Baseline truth entering G5

Final Create already materializes an exact T0-scoped starting `world_state` containing Source provenance and selected World / Character projections.

Existing Runtime already owns:

- one Game = one SQLite;
- Timeline / current head;
- atomic `commit_world_mutation_durably(...)`;
- Save / Restore / Recovery;
- durable accepted Conversation;
- exact Source ancestry.

G5-01 must extend these seams rather than creating a parallel database, save system or second world owner.

## 3. Core ordering

```text
Player action
→ free-form GM Narrative streams visibly
→ Narrative Provider completes
→ Conversation durable acceptance
→ visible turn remains accepted regardless of auxiliary semantic analysis
→ isolated Semantic Materialization Lane
→ 0..N newly established durable changes
→ Program constructs World Turn record
→ one atomic world mutation
→ later Context may project materialized changes
```

Protected principle:

```text
Model Freedom First
+
Visible Narrative First
+
Semantic Materialization Behind the Narrative Foreground
+
Canonical Commit only at durable world mutation
```

Narrative must never carry JSON/sentinels/headers for G5 semantic extraction.

## 4. Semantic Materialization Lane

G5-01 uses a **separate, non-player-visible analysis request** after an ordinary Conversation turn is durably accepted.

The lane may ask the selected model for small structured machine data because this is auxiliary semantic analysis, not visible prose.

Initial shape should remain intentionally narrow, equivalent to:

```json
{
  "changes": [
    "A concise factual consequence newly established by the accepted GM narrative"
  ]
}
```

Meaning of `changes`:

- only facts/consequences that the accepted GM narrative has actually established;
- persistent enough to matter beyond the sentence itself;
- may include newly established local conditions, decisions, injuries, possessions, arrivals/departures, commitments, closures/openings, discoveries or other lived-world consequences;
- may be empty when the turn creates no meaningful durable change.

Do not include:

- prose style notes;
- hidden chain-of-thought/reasoning;
- hypotheticals or plans not realized;
- player intent that the GM did not establish as successful/current reality;
- post-T0 authored future canon merely because Source knows it;
- new backstage facts invented only by the extraction request.

The analysis response is never player-visible Narrative and is never persisted as raw Provider payload/reasoning.

## 5. Failure semantics — non-blocking by default

Semantic materialization is a background/best-effort lane in G5-01.

Exactly one analysis attempt per newly accepted ordinary turn in v0.1. No automatic style/mechanics-like repair loop and no cross-provider fallback.

If analysis is:

- transport failure;
- missing credential;
- malformed/unparseable;
- empty/invalid machine data;
- valid with `changes = []`;

then:

```text
Conversation remains accepted
Player-visible Narrative remains valid
No world mutation is fabricated
Play may continue
```

Do not mark a turn failed merely because semantic extraction failed.

A future task may tighten convergence only when a real consumer truly requires authoritative semantic completion before the next action. G5-01 has no such hard gate.

## 6. Durable World Turn record

A successful non-empty materialization creates a Program-owned record inside game-local `world_state` under an optional living-world namespace.

Conceptual v0.1 shape:

```text
living_world
  schema_version = living_world.v0.1
  semantic_turns_by_index
    <conversation_turn_index>
      world_turn_id
      source_turn_index
      source_gm_sha256
      materialized_at
      changes[]
```

This is a **turn-level durable consequence ledger**, not a universal ontology.

It does not yet own typed current state for:

- Knowledge;
- Relationship;
- Location topology/current location;
- Inventory system;
- Injury system;
- NPC/Faction agency;
- Event/Priority evolution.

Those consumers must later pull out their own Domain semantics. G5-01 must not pre-build a universal entity/fact graph.

World Turn records may later serve as provenance/input for those Domains.

## 7. Identity / replay / correction

`world_turn_id` and the Persistence `mutation_id` must be Program-owned and stable for the accepted Conversation version, using at minimum:

```text
game identity
+ source turn index
+ accepted GM text hash/version identity
```

Same accepted content replay must converge to the same mutation identity/payload rather than duplicate a World Turn.

### Regenerate / latest-turn correction

A changed accepted GM text at the same turn index means the previous semantic record for that turn is stale.

Current Context projection must **never use a semantic record whose `source_gm_sha256` does not match the currently accepted Conversation entry at that turn index**.

A successful rematerialization replaces the current record for that turn index in the next world snapshot. Timeline retains older snapshots naturally, so Save/Restore can return to the old accepted Conversation + matching old semantic record.

This avoids requiring an output validator or irreversible side-effect compensation system in G5-01.

## 8. Persistence / Timeline semantics

Use the existing Game Runtime world-mutation seam and existing SQLite/Timeline storage.

Do not introduce a second persistence owner merely for G5-01.

Successful materialization:

```text
current world_state
→ clone + update living_world record
→ existing atomic world mutation CAS
→ COMMIT
→ Runtime publishes new current head/world_state
```

One successful semantic materialization = at most one canonical world mutation.

No per-token/chunk writes.

If world-mutation persistence fails:

- do not publish the candidate world state in memory;
- do not roll back the already accepted Conversation;
- do not fabricate a committed World Turn;
- expose a stable engineering/result status for tests/future UI, but G5-01 does not add UI.

## 9. Context projection

A later ordinary model request may receive a compact `Materialized World Changes` section derived from committed World Turn records.

Projection rules:

- only committed records;
- only records whose source GM hash still matches current accepted Conversation truth;
- no raw analysis payload/reasoning;
- no Source post-T0 leakage;
- keep the first implementation bounded (for example the latest relevant/recent materialized turns), rather than linearly dumping the entire world history.

G7 owns long-session retrieval/working-set optimization. G5-01 only proves the semantic state can survive beyond ephemeral UI/model output and re-enter Context safely.

## 10. Opening and autonomous world evolution

G5-01 v0.1 does **not** require semantic extraction of the initial GM-only Opening. T0 starting reality already comes from Final Create; Opening remains durable Conversation and can be revisited by later consumers if needed.

G5-01 also does not run autonomous NPC/Faction/world advancement while the Player is absent. That belongs to later G5 tasks.

## 11. Compatibility

Existing G4 Games have no `living_world` field. This means only:

```text
no G5 semantic turn material has been committed yet
```

The field is optional and can appear on the first successful G5-01 world mutation. No Source generation migration and no mandatory SQLite schema migration are implied by this decision.

Existing Source provenance/T0 materialization remains untouched.

## 12. Provider / Model Settings

Use the currently selected Runtime Model Settings provider/profile through the existing shared adapter seam.

G5-01 does not introduce:

- alternate hidden provider;
- cross-provider fallback;
- silent model switch;
- Source-specific model selection;
- narrative-format protocol.

Any future cheaper/faster semantic-analysis model lane is a separate architecture/product decision.

## 13. First implementation reality test

The first implementation must prove, with task-owned Game/Source state:

1. an ordinary free-form Narrative turn is visibly/durably accepted before semantic analysis matters;
2. a successful isolated semantic analysis produces at least one durable World Turn change;
3. the mutation survives close/reopen and Save/Restore according to Timeline truth;
4. a subsequent Context request can see committed matching materialized change(s);
5. malformed/transport analysis does not turn the accepted player action into a failed action and creates no fake mutation;
6. repeated same accepted content does not duplicate the World Turn;
7. regenerate/latest-turn replacement cannot project a stale record with an old GM hash;
8. no Source mutation, no Provider fallback, no UI requirement and no new narrative gate exists.

At least one real selected-Provider vertical is required for the successful semantic-analysis path. Failure/correction/replay cases can be controlled fixtures.

## 14. Explicit non-scope

Do not implement in G5-01:

- full entity graph / universal fact schema;
- Knowledge Provenance (G5-02);
- NPC/Faction autonomous Agency (G5-03);
- Event/Priority-driven World Evolution (G5-04);
- mechanics integration redesign (G5-05);
- UI projection surfaces (G5-06);
- portrait / scene / authored-map runtime (G6);
- long-session semantic retrieval architecture (G7).

## 15. Product checkpoint

G5-01 product value is not “the JSON exists.” The eventual Owner checkpoint should demonstrate a simple lived consequence that remains true after later play/reopen, while Narrative still feels free-form and responsive.

The deferred G4-11 narrative-voice soft-prompt effect may be observed opportunistically during that same future UAT; it does not receive a separate gate.
