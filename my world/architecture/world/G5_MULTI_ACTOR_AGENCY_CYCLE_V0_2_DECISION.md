# G5 Multi-Actor Agency Cycle v0.2 Decision

Status: **FROZEN / CURRENT CANONICAL**  
Phase: **G5 World Semantics & GM Runtime**  
Parent: **G5-03 NPC / Faction Agency**  
Supersedes: `G5_STABLE_NPC_AGENCY_V0_1_DECISION.md` section 5 single-actor scheduling and the old G5-03M1 packet.

## 1. Owner product correction

The Owner explicitly rejected the rule “evaluate at most one NPC per player turn”. In a pressured world state, several independent actors may plausibly act during the same world window. Example: in a Red-Cliffs-eve Liu Bei game, Sun Quan, Cao Cao and Zhuge Liang may all have independent reasons to act.

Canonical product rule:

> **One player turn may lead to zero, one, or several independent actor actions.**

The Runtime must not serialize the living world into an artificial one-NPC-per-turn queue merely to reduce Provider cost.

## 2. Agency Cycle

An eligible accepted ordinary turn creates at most one **Agency Cycle**:

```text
free-form GM Narrative
→ durable Conversation acceptance
→ existing G5-01/G5-02 semantic analysis
   + Agency Selection
→ 0..N selected stable actors
→ actor-scoped agency execution for each selected actor
→ 0..N durable independent actor actions
```

Selection and execution are distinct semantic phases even when selection reuses an existing Provider request.

## 3. Selection should reuse the existing semantic analysis request

Do **not** add a mandatory standalone selector Provider call when the existing post-Narrative semantic-analysis lane can carry the selection cheaply.

Extend its optional machine result conceptually to:

```json
{
  "changes": [],
  "knowledge_events": [],
  "agency_candidates": ["stable-actor-id-a", "stable-actor-id-b"]
}
```

`agency_candidates` is best-effort and failure-isolated:

```text
invalid / absent agency_candidates
!= invalid changes
!= invalid knowledge_events
!= Narrative failure
```

The selector decides only **which actors currently have a plausible reason to evaluate an independent action**. It does not author their actions.

The selector must be given bounded per-actor eligibility material and instructed to judge each actor from that actor's own Source/knowledge/history, not from another actor's private knowledge.

## 4. Bounded multiplicity, not single-actor throttling

For v0.2, the selector may choose **0..4 actors per Agency Cycle** from the current eligible stable actor roster.

`4` is a first implementation safety ceiling against runaway fan-out, not a game-world claim that only four actors can matter. It is intentionally high enough to support multiple simultaneous major actors and may be raised later from product evidence without changing the Agency Cycle contract.

Do not implement round-robin as the primary agency scheduler.

Fairness is not “everyone gets a turn”; relevance to current actor-local pressure is the selection criterion.

## 5. Actor execution is isolated per actor

Each selected actor receives its own non-player-visible agency request.

Required actor-scoped input:

1. exact stable actor ID + display name;
2. that actor's Game-local Character Source / T0 material;
3. only that actor's committed G5-02 Knowledge Provenance;
4. that actor's own recent committed agency history;
5. minimal cycle/source identity metadata.

Do not put several actors' private knowledge into one shared action-generation request. The selector may compare bounded actor blocks, but **execution remains one actor per request** so G5-02 knowledge boundaries remain meaningful.

## 6. Concurrent execution

Selected actor requests should be able to run concurrently in the background. Sequential Provider execution would recreate a latency bias where the first actor routinely acts and later actors lose the race to the player's next turn.

Concurrency is bounded by the selected actor count (max 4 in v0.2).

The player never waits for the Agency Cycle.

## 7. Durable commit semantics

Each valid `decision=act` may become its own atomic world mutation as soon as that actor result is valid and the Agency Cycle remains current.

Provider requests may run concurrently; durable commits must be serialized through the existing Runtime world-mutation seam so SQLite/world-head ownership remains single-writer.

Use stable identities containing at least:

`game_id + agency_cycle_id + source_turn_index + source_gm_sha256 + cycle_base_head_id + actor_id`.

A cycle may therefore durably contain multiple actor action records for the same source turn.

Sibling agency commits made by the same current Agency Cycle are **not** external staleness. Track the cycle's own allowed head progression. Any unrelated head change invalidates remaining uncommitted results.

## 8. Foreground wins

Agency remains background/fail-soft.

If the player starts the next Conversation attempt, Restore/Recovery switches progress, the source accepted version changes, or the Game closes:

- invalidate all queued/active uncommitted agency work;
- best-effort cancel transports;
- late callbacks must not commit;
- already committed actor actions remain durable history because they completed before the foreground boundary.

Never wait for the remaining selected actors before accepting the player's next action.

## 9. Machine output

Per-actor execution remains conceptually:

```json
{
  "actor_id": "exact-stable-id",
  "decision": "hold|act",
  "intent": "concise aim",
  "action": "concise action now undertaken",
  "effects": ["immediate established effect"]
}
```

`hold`, malformed output, wrong actor ID, Provider failure, or invalid effects produce no fake mutation and never fail foreground Narrative.

No hidden d20/check resolution in G5-03. G5-05 owns mechanics integration.

## 10. Context semantics

Committed agency actions may enter later bounded omniscient GM Context as world truth.

They do **not** automatically become:

- Player disclosure;
- another actor's knowledge;
- universal knowledge provenance.

The acting actor may receive its own prior actions in future agency input.

## 11. Current actor pool versus required future pool

The current Game already guarantees stable local identities and exact Character Source for **Guaranteed NPCs**. G5-03M1 uses at least this stable pool to prove multi-actor selection/execution.

However, Guaranteed NPC is **not** the permanent agency boundary.

The Owner's concrete example (Cao Cao / Sun Quan / Zhuge Liang in the same Red-Cliffs world) establishes a real consumer for a later G5-03 slice that can materialize additional important named actors into a Game-local stable actor registry when they have sufficient canonical identity/material.

Therefore route intent is:

```text
G5-03M1 Multi-Actor Agency Cycle over existing stable actors
→ Independent Review
→ G5-03M2 Minimal Stable Actor Materialization / Registry expansion
→ then decide Faction slice / G5-03 closeout
```

Do not solve M2 by free-form display-name matching or by silently pulling mutable external Source into an existing Game. Identity/materialization semantics must be frozen separately.

## 12. Relationship to G5-04

The Agency Selector is a local consumer question:

> Which currently stable actors plausibly have reason to act now?

This does not implement the general Event/Priority world-evolution engine. G5-04 still owns broader pressures, event scheduling, player-absence progression, faction/world priorities and selective world-time evolution.

## 13. Real Provider validation

Standing authorization applies.

For M1, after deterministic gates are green, a bounded feature proof may use at most:

- one real combined semantic-selection request; and
- up to two real selected actor execution requests in that same task-owned cycle.

No fallback, hidden Provider switch or retry-until-act loop.

If Provider availability prevents the proof, commit/push reviewable implementation and mark the real vertical pending under the standing outage rule.

## 14. Explicit non-scope for M1

Do not implement in M1:

- arbitrary emergent actor identity;
- free-form name→identity matching;
- Faction agency;
- G5-04 general event/priority scheduler;
- relationship/economy/inventory simulation;
- mechanics/d20 integration;
- player-facing agency UI;
- G7 long-session retrieval.

## 15. Protected principles

> **Source provides inertia; actors create history.**

> **GM omniscience must not become actor omniscience.**

> **Foreground player freedom outranks background agency completion.**

> **Multi-actor world behavior must not be collapsed into one-NPC-per-turn purely for implementation convenience.**
