---
title: my world｜当前状态
status: current-project-status
version: 11.9
created: 2026-08-26
updated: 2026-09-03
phase: G5 World Semantics & GM Runtime
current_task: G5-03M1 Multi-Actor Agency Cycle
current_owner: KIMI
parent_task: G5-03 NPC / Faction Agency
semantic_owner: GPT
owner_uat_required: false
context_handoff: handoff/GPT_CONTEXT_HANDOFF_CURRENT.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                              PASS / CLOSED
G2 AI Conversation Spine                   PASS / CLOSED
G3 Persistence / Save / Timeline           PASS / CLOSED
G4 Primary Source Assets & Local Game      PASS / CLOSED
G4-10 Runtime Asset Resolution             DEFERRED / MOVED TO G6

G5 World Semantics & GM Runtime            ACTIVE
G5-01 World Turn / Semantic Materialization PASS / CLOSED
G5-02 Knowledge Provenance                 PASS / CLOSED
G5-02M1 Known-Actor Knowledge Spine        ENGINEERING PASS / CLOSED
G5-02 real Provider vertical               HISTORICAL GAP / Provider unavailable

G5-03 NPC / Faction Agency                 ACTIVE
G5-03M1 old Stable Guaranteed-NPC packet   SUPERSEDED / DO NOT EXECUTE
G5-03M1 Multi-Actor Agency Cycle           ACTIVE — KIMI
G5-03M2 Stable Actor Materialization       NEXT AFTER M1 REVIEW
G5-04 Event / Priority Evolution           NOT YET
G5-GATE                                    NOT YET
```

## 2. G5-02 closeout

G5-02 is PASS/CLOSED. Formal closeout:

`my-world/docs/g5_02/G5-02_CLOSEOUT.md`

Protected distinction remains:

```text
World / Game Truth
!= actor knowledge
!= human-player disclosure
!= omniscient GM Context
```

G5-02 real selected-Provider feature proof remains an honest historical gap because its single bounded attempt timed out before feature execution. Do not reopen G5-02 solely to manufacture retrospective proof.

## 3. Owner correction to G5-03

Owner explicitly rejected the previously frozen rule:

```text
one eligible player turn
→ evaluate at most one NPC
→ round-robin
```

Reason: a pressured world window may plausibly contain several independent actor actions. Red-Cliffs-eve example: Cao Cao, Sun Quan and Zhuge Liang may all have simultaneous reasons to act.

Canonical product rule:

> **One player/world turn may lead to zero, one, or several independent actor actions.**

Current canonical decision:

`architecture/world/G5_MULTI_ACTOR_AGENCY_CYCLE_V0_2_DECISION.md`

Historical single-NPC decision:

`architecture/world/G5_STABLE_NPC_AGENCY_V0_1_DECISION.md` → SUPERSEDED.

## 4. Current task

Implementation packet:

`my-world/docs/tasks/G5-03M1_MULTI_ACTOR_AGENCY_CYCLE_TASK.md`

Old packet:

`my-world/docs/tasks/G5-03M1_STABLE_NPC_INDEPENDENT_AGENCY_TASK.md` → SUPERSEDED / DO NOT EXECUTE.

Current Agency Cycle:

```text
accepted ordinary turn
→ existing post-Narrative semantic-analysis request
   also performs Agency Selection
→ validated agency_candidates = 0..4 stable NPC IDs
→ separate actor-scoped execution request for every selected actor
→ selected requests may run concurrently
→ several valid acts may durably commit during the same Agency Cycle
```

No mandatory standalone selector Provider request is added. Selection is a semantic phase, but should piggyback on the existing auxiliary analysis request.

## 5. Selection semantics

For M1, eligible stable actors are current Guaranteed NPCs with durable `local_character_id`.

Selection is relevance-based, not round-robin fairness.

For every candidate, selector input may contain bounded:

- exact actor ID/display name;
- actor's own Character Source/T0 agency-relevant material;
- actor's own committed Knowledge Provenance;
- actor's own recent agency history.

Instruction must require each candidate to be judged from **that actor's own** material rather than another actor's private knowledge.

Optional `agency_candidates` parsing is fail-soft and independent:

```text
bad selection
!= bad G5-01 changes
!= bad G5-02 knowledge
!= Narrative failure
```

Cap: 4 actors/cycle in v0.2. No retry-until-nonempty and no round-robin fallback.

## 6. Per-actor execution

Each selected actor gets an independent machine request containing only:

- exact actor identity;
- that actor's Game-local Character Source/T0 material;
- that actor's own durable knowledge;
- that actor's own recent agency history;
- minimal cycle/source identity.

Do not author several actors' actions from one combined private-knowledge execution prompt.

Protected principle:

> **GM omniscience must not become actor omniscience.**

## 7. Concurrent background behavior

Selected actor requests must be able to progress concurrently, bounded by selected count (max 4), so Provider latency does not recreate a first-actor-only bias.

Agency is background/best-effort and never a Turn Finalize Barrier.

Foreground player input wins. On new Conversation attempt / Restore / Recovery / source replacement / unrelated world-head change / session close:

- invalidate remaining uncommitted actor work;
- best-effort cancel transports;
- late callbacks cannot commit;
- actor actions already committed before the foreground boundary remain durable.

## 8. Durable multi-actor cycle

Several actors may create durable actions from the same source turn/cycle.

Use existing `commit_world_mutation_durably(...)` only; no SQLite schema/table.

Provider requests may finish concurrently, but durable commits must serialize. A successful sibling agency commit is allowed cycle-owned head progression; unrelated head changes invalidate remaining results.

Committed independent actor actions may enter later bounded omniscient GM Context. They are not automatic Player disclosure or automatic knowledge for other actors.

## 9. Actor pool after M1

Guaranteed NPC is an **M1 bootstrap pool**, not the permanent product boundary.

Owner's Cao Cao / Sun Quan / Zhuge Liang example provides a real consumer for:

```text
G5-03M2 Minimal Stable Actor Materialization / Registry Expansion
```

M2 will define how important non-Guaranteed named NPCs become Game-local stable actors without unsafe free-form name matching or silently importing mutable external Source into an existing Game.

Do not smuggle M2 into M1.

## 10. Relationship to G5-04

G5-03 selection answers only:

> Which currently stable actors plausibly have reason to act now?

G5-04 still owns broad Event/Priority world evolution, player-absence progression, faction/world pressures and selective world-time advancement.

## 11. Real Provider authorization

Standing authorization applies.

After deterministic gates are green, G5-03M1 may use at most:

```text
1 real combined semantic-selection request
+
up to 2 real selected actor execution requests
```

Maximum 3 feature-owned calls. No retry loops, fallback or hidden Provider switch.

If inconclusive/unavailable, commit/push reviewable work and mark real proof pending.

## 12. Temporary execution routing

Through 2026-09-06 23:59 (+08:00):

```text
GPT        → semantics / architecture / task shaping / final Independent Review
Kimi       → primary code-changing implementation owner
Grok Build → research/evidence support when useful
Owner      → Product UAT / explicit verdict
```

Auto-expiry 2026-09-07 00:00 (+08:00); correct in-flight Kimi work may finish.

## 13. Route

```text
Kimi G5-03M1 Multi-Actor Agency Cycle
→ GPT Independent Review
→ shape/execute G5-03M2 Stable Actor Materialization
→ decide remaining Faction slice from actual consumer
→ G5-04
```

Visual runtime remains deferred to G6.
