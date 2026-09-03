# G5 Agency Scheduler v0.3 Decision

Status: **FROZEN / CURRENT CANONICAL**  
Phase: **G5 World Semantics & GM Runtime**  
Parent: **G5-03 NPC / Faction Agency**  
Supersedes: `G5_MULTI_ACTOR_AGENCY_CYCLE_V0_2_DECISION.md` scheduling/selection coupling and `G5-03M1C02_SEMANTIC_AGENCY_CURRENTNESS_SEPARATION_CORRECTION_TASK.md`.

## 1. Why redesign now

The same orchestration seam failed twice during review:

```text
semantic materialization currentness
+
Agency selection currentness
+
foreground/timeline currentness
```

The v0.2 optimization attempted to save one Provider call by making the existing G5-01/G5-02 semantic request also select Agency actors. That coupling created more correctness cost than the saved call justified.

Correction-budget rule therefore applies:

> **same seam still fails → redesign**

This is not a rollback of the whole G5-03 implementation. Preserve the already-working downstream Multi-Actor execution/runtime and replace only the upstream scheduler/selection coupling.

## 2. Canonical flow

```text
free-form Narrative
→ durable Conversation acceptance
→ G5-01/G5-02 semantic lane
   changes + knowledge only
→ mark world as agency-dirty
→ when foreground is idle and pending semantic work has settled:
   standalone Agency Selector evaluates the current latest world snapshot
→ selector returns 0..4 stable actor IDs
→ separate actor-scoped execution request per selected actor
→ selected executions may progress concurrently
→ 0..N valid sibling actions durably commit
```

Agency is a best-effort evaluation of the **current world state**, not a mandatory child transaction of every player turn.

## 3. Semantic lane is independent again

The G5-01/G5-02 semantic response returns only its own concerns:

```json
{
  "changes": [],
  "knowledge_events": []
}
```

Remove `agency_candidates` from the required semantic prompt/result/handoff.

Semantic materialization validity is governed only by its own accepted source-version semantics. A player starting a newer turn must not erase valid older accepted `changes` or `knowledge` merely because an Agency opportunity has become stale.

Narrative acceptance remains independent of semantic success.

## 4. Agency dirty/coalescing semantics

A newly durable accepted ordinary player turn marks Agency evaluation **dirty**.

Dirty means:

> the current world may deserve one fresh Agency evaluation when background conditions permit.

It does **not** mean every accepted turn must receive its own Agency Cycle.

If the player advances rapidly:

```text
Turn A
→ Turn B
→ Turn C
```

before Agency runs, obsolete opportunities may be coalesced. Once foreground and semantic background work settle, the scheduler evaluates the **latest current world snapshot**, not A/B/C individually.

No catch-up queue of historical Agency Cycles is required.

## 5. When the selector may run

The standalone selector may start only when:

- a current Game Session exists;
- Agency is dirty;
- foreground Conversation is idle;
- the G5-01/G5-02 semantic worker has no active or queued work;
- no selector/Agency Cycle is already active.

The semantic worker's terminal signal may be used only as a scheduling wake-up. Its parsed content does not carry Agency candidates.

If semantic analysis fails, malformed/empty/no-change, that still counts as a terminal background state; Agency may evaluate the current world snapshot afterward.

## 6. Selector authority and input

The selector answers only:

> **Which currently stable actors plausibly deserve an independent action evaluation now?**

For v0.3 it may use a bounded **GM-level current-world view**, including:

- current latest accepted player/GM turn identity/text, bounded;
- current materialized world consequences;
- current stable actor roster + bounded Source summaries;
- current Knowledge/Agency world state as useful scheduling evidence.

Selector omniscience is allowed because it does not author an actor's action or grant actor knowledge.

Protected distinction:

```text
Selector omniscience
!= actor omniscience
```

Output is machine-only and minimal:

```json
{"actors":["stable-id-a","stable-id-b"]}
```

Validate against the current eligible stable actor roster, deduplicate, cap at 4, reject Player/unknown IDs. No retry-until-nonempty and no round-robin fallback.

## 7. Per-actor execution remains isolated

Preserve the accepted downstream contract.

Each selected actor receives its own request containing only:

- exact stable actor identity;
- that actor's Game-local Character Source/T0 material;
- that actor's **current-hash-matching** durable Knowledge Provenance;
- that actor's **current-hash-matching** recent agency history;
- minimal current Agency Cycle identity.

Several actors must not share private knowledge in one execution request.

Selected actor requests may run concurrently, bounded by selected count (max 4).

## 8. Foreground wins; missed opportunities are allowed

If foreground starts while selector or actor executions are active:

- invalidate/cancel the selector;
- invalidate/cancel all remaining uncommitted actor work;
- already committed actor actions remain durable;
- do **not** try to rescue or finish the obsolete Agency opportunity.

The new foreground attempt clears that obsolete opportunity. A later successfully accepted player turn marks Agency dirty again.

If foreground generation fails/cancels, the previous missed Agency opportunity need not be replayed automatically.

This is deliberate best-effort behavior, not data loss of authoritative player/Narrative truth.

## 9. Restore / replacement / world-head currentness

Restore/Recovery/session close invalidates active selector/cycle and clears obsolete dirty work. Do not auto-fire Agency merely because a save was loaded.

A selector freezes at least:

- latest accepted turn index + GM hash;
- current world head ID.

Before using selector output, these must still match the current snapshot and foreground must remain idle.

Actor execution keeps the existing cycle-owned expected-head rule:

- sibling agency commits may advance the expected head;
- unrelated head changes invalidate remaining uncommitted actor results;
- source accepted hash/replacement must remain current for that cycle anchor.

## 10. Durable cycle identity

Agency no longer promises one cycle per player turn, but each cycle that actually starts is still anchored to the latest accepted turn/world snapshot at selector time.

Existing durable cycle/action identity and `commit_world_mutation_durably(...)` may be reused where valid.

No new SQLite table/schema.

A cycle may still contain several sibling actor actions.

## 11. Replay

If the same current anchored cycle/action is already durably committed, do not re-execute that actor merely because scheduler consideration runs again.

No duplicate Provider execution/mutation for an already committed matching actor action.

## 12. Failure semantics

Selector malformed/empty/provider failure:

```text
→ no Agency this opportunity
→ foreground unaffected
→ no automatic retry loop
```

Actor `hold`, malformed/wrong actor/provider failure:

```text
→ no fake mutation for that actor
→ siblings remain independent
→ foreground unaffected
```

Agency is never a Turn Finalize Barrier.

## 13. What is preserved from v0.2/C01

Keep unless a concrete implementation dependency requires a narrow adaptation:

- multi-actor selection cap 4;
- separate actor execution requests;
- concurrent actor execution;
- actor-private Knowledge/Source/history;
- serialized atomic sibling commits;
- cycle-owned sibling head progression;
- Restore/foreground cancellation;
- stale actor memory filtering;
- same-version replay no-duplicate;
- Independent Actor Actions in bounded GM Context;
- no automatic Player/other-actor knowledge grant;
- no SQLite/UI/Faction/G5-04/mechanics expansion.

## 14. Actor pool

v0.3 M1 still uses stable Guaranteed NPCs as the bootstrap eligible pool.

Guaranteed NPC is not the permanent product boundary. After M1 Engineering PASS, G5-03M2 remains the intended slice for minimal stable actor materialization/registry expansion so important non-Guaranteed named actors can enter the same scheduler/execution contract.

## 15. Provider cost policy

The redesign explicitly accepts one additional lightweight selector model request when Agency evaluation actually runs:

```text
semantic call
+
selector call
+
0..N actor execution calls
```

The selector call is an accepted product/architecture cost. Do not re-couple it into semantic materialization merely to save a request.

A v0.3 implementation reality proof should stub unrelated Narrative/semantic prerequisites and may use at most:

```text
1 real selector request
+
up to 2 real actor execution requests
```

No retry loop, fallback, hidden Provider switch or real Narrative prerequisite call solely for this proof.

## 16. Protected principles

> **Source provides inertia; actors create history.**

> **GM omniscience must not become actor omniscience.**

> **Foreground player freedom outranks background agency completion.**

> **Agency evaluates the current world; it does not owe every historical turn a catch-up cycle.**

> **Spend one cheap selector call rather than buy correctness with orchestration coupling.**
