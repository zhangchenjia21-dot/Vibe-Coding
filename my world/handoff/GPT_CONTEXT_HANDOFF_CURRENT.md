---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-03
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G5-03 NPC / Faction Agency
current_execution_task: G5-03M1 Multi-Actor Agency Cycle
semantic_owner: GPT
current_execution_owner: KIMI
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Current state

```text
G4 Primary Source Assets & Local Game Creation   PASS / CLOSED
G5 World Semantics & GM Runtime                  ACTIVE
G5-01 World Turn / Semantic Materialization     PASS / CLOSED
G5-02 Knowledge Provenance                      PASS / CLOSED
G5-03 NPC / Faction Agency                      ACTIVE
G5-03M1 old single-NPC task                     SUPERSEDED / DO NOT EXECUTE
G5-03M1 Multi-Actor Agency Cycle                ACTIVE — KIMI
G5-03M2 Stable Actor Materialization            NEXT AFTER M1 REVIEW
G5-04 Event / Priority Evolution                NOT YET
```

## 2. G5-02 inherited truth

G5-02 established:

```text
Game / World Truth
!= actor knowledge
!= human-player disclosure
!= omniscient GM Context
```

Post-T0 Knowledge Provenance exists for current stable Player/Guaranteed-NPC local IDs. The ordinary semantic-analysis request already carries stable actor names + exact IDs. Knowledge parse failure remains isolated from valid G5-01 changes and Narrative acceptance.

The single historical G5-02 real-provider attempt timed out before feature execution. Keep that as a historical gap; G5-03 is the direct consumer.

## 3. Owner correction to agency scheduling

Owner explicitly rejected the previous canonical rule:

```text
at most one NPC evaluated per accepted player turn
+ deterministic round-robin
```

Owner product expectation: a single pressured world window may plausibly contain several independent NPC actions; Red-Cliffs example included Cao Cao, Sun Quan and Zhuge Liang.

Canonical rule now:

> **One player/world turn may lead to zero, one, or several independent actor actions.**

Current canonical decision:

`my world/architecture/world/G5_MULTI_ACTOR_AGENCY_CYCLE_V0_2_DECISION.md`

Historical v0.1 decision is SUPERSEDED.

## 4. Current task

`my-world/docs/tasks/G5-03M1_MULTI_ACTOR_AGENCY_CYCLE_TASK.md`

Old `G5-03M1_STABLE_NPC_INDEPENDENT_AGENCY_TASK.md` is SUPERSEDED / DO NOT EXECUTE.

M1 Agency Cycle:

```text
accepted ordinary turn
→ existing G5-01/G5-02 semantic request also performs Agency Selection
→ optional validated agency_candidates = 0..4 stable NPC IDs
→ one actor-scoped request per selected actor
→ selected requests may progress concurrently
→ 0..N valid acts may durably commit in the same Agency Cycle
```

No mandatory extra selector model request.

## 5. Selection semantics

For M1, eligible actor pool is current stable Guaranteed NPCs. This is a bootstrap pool, not the permanent product boundary.

Extend the existing semantic response with optional `agency_candidates`.

Selection parsing/validation is fail-soft:

```text
bad/absent agency_candidates
!= invalid changes
!= invalid knowledge_events
!= Narrative failure
```

Selector input should include bounded per-actor:

- exact ID/name;
- own Character Source/T0 agency-relevant material;
- own durable knowledge;
- own recent agency history.

Instruction: judge each actor only from that actor's own supplied material; do not use another actor's private knowledge to justify selection.

Cap 4 actors/cycle. No round-robin fallback or retry-until-nonempty.

## 6. Execution semantics

Every selected actor gets its own request containing only:

- exact actor identity;
- own Game-local Character Source/T0 material;
- own G5-02 knowledge;
- own recent agency history;
- minimal cycle/source identity.

Never combine several actors' private knowledge into one action-generation request.

Per actor output remains `hold|act` with bounded intent/action/effects. Wrong actor/malformed/provider failure is fail-soft. No hidden d20/check logic.

Protected:

> **GM omniscience must not become actor omniscience.**

## 7. Concurrency / foreground freedom

Selected actor Provider requests should progress concurrently, bounded by selected count (max 4). Do not implement sequential latency bias.

Agency never blocks player input.

If foreground Conversation starts, Restore/Recovery switches progress, source version changes, unrelated world head changes or session closes:

- invalidate remaining uncommitted requests;
- best-effort cancel transports;
- late callbacks cannot commit;
- actor actions already committed before the boundary remain durable.

## 8. Durable multi-actor semantics

Several actors may durably act from one source turn/cycle.

Use stable Agency Cycle + actor action IDs and existing `commit_world_mutation_durably(...)`; no SQLite schema/table.

Provider completions may be concurrent but world commits must serialize. A successful sibling action in the same cycle is allowed cycle-owned head progression and must not stale remaining siblings. Any unrelated head change invalidates them.

Later GM Context may project bounded Independent Actor Actions. Those actions are not automatic Player disclosure or automatic knowledge for other actors.

## 9. Required M1 review focus

When Kimi returns, verify actual code/evidence for:

1. one existing semantic-analysis request, not an added selector call;
2. optional `agency_candidates` parser/validation failure isolated from G5-01/G5-02;
3. 0..4 stable NPC selection with unknown/Player IDs rejected and no round-robin fallback;
4. selected actors get separate actor-scoped requests;
5. A's request excludes B/Player private knowledge and vice versa;
6. at least two selected requests can be active/progress concurrently;
7. two valid acts from one cycle can both durably commit regardless completion order;
8. sibling commit does not stale sibling; unrelated head change does;
9. mixed act/hold/failure works fail-soft;
10. foreground/Restore late callbacks cannot commit;
11. replay/reopen does not duplicate and GM Context preserves multiple actions;
12. no UI/SQLite/Source/Faction/G5-04+ scope creep.

## 10. Real Provider rule

After deterministic gates are green, standing authorization permits at most:

```text
1 real combined semantic-selection request
+
up to 2 real selected actor agency requests
```

Max 3 feature-owned calls. No retries/fallback/hidden switch.

If inconclusive/unavailable, review still proceeds with real proof pending.

## 11. Next slice already justified by Owner consumer

Guaranteed NPC is not the permanent agency pool.

After M1 IR, GPT should shape:

```text
G5-03M2 Minimal Stable Actor Materialization / Registry Expansion
```

Purpose: let important non-Guaranteed named NPCs become stable Game-local actors so cases such as Cao Cao / Sun Quan / Zhuge Liang can enter the same selection/execution contract when canonically available.

Do not solve M2 via free-form display-name matching or silently pull mutable external Source into an existing Game. Freeze identity/materialization semantics first.

## 12. Relationship to G5-04

Agency selection is local actor relevance only. G5-04 still owns broader Event/Priority scheduling, player-absence progression and world/faction pressures.

## 13. Temporary routing

Through 2026-09-06 23:59 (+08:00):

```text
GPT        → semantics / architecture / governance / final Independent Review
Kimi       → code-changing implementation owner
Grok Build → research/evidence support when useful
Owner      → Product UAT / verdict
```

Auto-expiry 2026-09-07 00:00 (+08:00); correct in-flight Kimi work may finish.
