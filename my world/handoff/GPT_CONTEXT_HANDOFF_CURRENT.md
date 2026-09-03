---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-03
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G5-03M1 Multi-Actor Agency
current_execution_task: G5-03M1R01 Agency Scheduler v0.3 Simplification Redesign
semantic_owner: GPT
current_execution_owner: KIMI
---

# my world｜GPT CONTEXT HANDOFF CURRENT

Refresh both `main`s before authoritative work.

## Current state

```text
G5-01 World Turn / Semantic Materialization     PASS / CLOSED
G5-02 Knowledge Provenance                      PASS / CLOSED
G5-03 NPC / Faction Agency                      ACTIVE
G5-03M1 Multi-Actor Agency                      REDESIGN ACTIVE
G5-03M1C01                                      HISTORICAL PARTIAL PASS
G5-03M1C02                                      SUPERSEDED / DO NOT EXECUTE
G5-03M1R01 Agency Scheduler v0.3                ACTIVE — KIMI
G5-03M2 Stable Actor Materialization            NOT YET
G5-04 Event / Priority Evolution                NOT YET
```

Current canonical decision:

`my world/architecture/world/G5_AGENCY_SCHEDULER_V0_3_DECISION.md`

Current task:

`my-world/docs/tasks/G5-03M1R01_AGENCY_SCHEDULER_V0_3_SIMPLIFICATION_REDESIGN_TASK.md`

Historical `G5_MULTI_ACTOR_AGENCY_CYCLE_V0_2_DECISION.md` and C02 are superseded.

## Why redesign

The same semantic/Agency currentness seam failed twice. Owner approved relaxing the one-call optimization and correction-budget policy requires redesign rather than a third patch.

Do not rollback existing implementation. Preserve accepted downstream Multi-Actor execution/runtime; replace only semantic-piggyback selection.

## Canonical v0.3 flow

```text
Narrative durable accepted
→ semantic lane handles changes + knowledge only
→ scheduler marks Agency dirty
→ foreground idle + semantic worker fully settled
→ standalone selector evaluates latest current world snapshot
→ validated 0..4 stable actors
→ existing separate concurrent actor executions
→ 0..N durable sibling actions
```

Agency is best-effort current-world evaluation, not one required cycle per player turn. A/B/C may coalesce into one later C evaluation; no historical catch-up queue.

## Preserve

Review must preserve/verify:

- per-actor isolated execution;
- concurrent selected actor requests;
- current-hash actor Knowledge/History isolation;
- serialized sibling world commits;
- sibling expected-head progression;
- foreground/Restore cancellation;
- replay no duplicate;
- bounded GM Context projection;
- no automatic Player/other-actor knowledge grant.

## Remove

The semantic prompt/result/handoff must no longer carry production `agency_candidates`.

Semantic currentness returns to G5-01/G5-02 meaning: an older accepted turn whose GM hash still matches may materialize its valid changes/knowledge even if foreground advanced.

## Scheduler rules

A durable accepted ordinary turn marks dirty.

Selector starts only if dirty, Session ready, foreground idle, semantic active+queue counts are zero, and no selector/cycle is active.

Selector may use bounded GM-level current-world information. This does not grant actor knowledge; actual actor execution remains private.

Selector freezes latest accepted turn index/hash + world head. Late result is usable only if all still match and foreground remains idle.

New foreground cancels selector/remaining actors and abandons the obsolete opportunity. Next successfully accepted turn marks dirty again. Restore clears active/dirty background Agency and does not auto-fire Agency.

## R01 Provider proof

After deterministic gates green, standing authorization allows at most:

```text
1 real standalone selector request
+
up to 2 real actor execution requests
```

Narrative and semantic prerequisites must be stubbed. No retry/fallback/hidden switch. If Provider unavailable, code review proceeds with real proof pending.

## Review when Kimi returns

Inspect actual code/evidence for:

1. no semantic `agency_candidates` production dependency;
2. older unchanged accepted semantic truth still materializes;
3. dirty/coalescing A→B→C works without catch-up queue;
4. exactly one standalone selector for latest eligible snapshot;
5. selector latest-hash/head/foreground stale guards;
6. selector cancellation on foreground/Restore;
7. multi-actor concurrent execution + knowledge isolation preserved;
8. sibling commits/head progression preserved;
9. replay no duplicate preserved;
10. no M2/Faction/G5-04/UI/SQLite scope creep;
11. focused/regression tests + `git diff --check`;
12. real selector/actor proof or honest external-provider pending status.

## Next intended slice

After R01/M1 Engineering PASS:

`G5-03M2 Minimal Stable Actor Materialization / Registry Expansion`

Temporary Kimi implementation routing remains active through 2026-09-06 23:59 (+08:00); correct in-flight work may finish afterward.
