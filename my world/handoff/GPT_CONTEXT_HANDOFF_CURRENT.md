---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-03
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G5-03M1R01 Agency Scheduler v0.3
current_execution_task: G5-03M1R01C02 Dirty Opportunity Consumption + Lifecycle Proof
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
G5-03M1R01 Agency Scheduler v0.3                CORRECTION REQUIRED
G5-03M1R01C01 Scheduler Lifecycle/Snapshot      CORRECTION REQUIRED / CLOSED INTO C02
G5-03M1R01C02 Dirty Opportunity Consumption     ACTIVE — KIMI
G5-03M2 Stable Actor Materialization            NOT YET
G5-04 Event / Priority Evolution                NOT YET
```

Canonical:

`my world/architecture/world/G5_AGENCY_SCHEDULER_V0_3_DECISION.md`

C01 review:

`my-world/docs/g5_03/G5-03M1R01C01_INDEPENDENT_REVIEW.md`

Current packet:

`my-world/docs/tasks/G5-03M1R01C02_DIRTY_OPPORTUNITY_CONSUMPTION_CORRECTION_TASK.md`

Owner temporarily declined Gemini review; current flow is Kimi implementation → GPT Independent Review.

## Accepted architecture

Do not redesign or re-couple Agency selection into semantic analysis:

```text
ordinary durable accepted turn
→ semantic changes + knowledge only
→ one dirty Agency opportunity
→ standalone selector over latest current world
→ 0..4 stable actors
→ existing concurrent actor-scoped Agency Cycle
```

## C01 review result

Reviewed implementation HEAD: `9da292966e5a56bbbea7ca5aedb20919d5ebb092`.

C01 correctly added:

- production `generation_completed` dirty wiring for ordinary player turns;
- cycle terminal child cleanup;
- current accepted-hash selector consequence filtering;
- selector adapter cleanup.

Blocking defect remains: `dirty` is never consumed when a selector starts. Therefore Cycle A terminal still sees `dirty=true` and immediately calls `consider_agency()`, creating another selector for the same accepted-turn opportunity.

Current focused no-retry test is inverted: it asserts dirty remains true and manual re-consider starts a new selector. The C01 production-wiring test also directly calls `mark_dirty()` instead of exercising Application production callback wiring.

## C02 review focus

When Kimi returns verify actual code/tests for:

1. selector start consumes current dirty opportunity (`dirty=false`);
2. completed/no-actors/malformed/provider-failed/hold opportunity cannot auto-retry or manual re-consider without a new accepted turn;
3. Cycle A terminal produces zero selector A2 before Turn B acceptance;
4. later Turn B production acceptance creates fresh dirty and can start selector/cycle B;
5. production dirty proof does not directly call `mark_dirty()`;
6. exact completed-cycle cleanup cannot let an old callback clear a newer cycle;
7. C01 current-hash filtering and all R01 concurrency/knowledge/foreground/Restore/replay behavior remain green;
8. zero real Provider calls and `git diff --check` PASS.

## Provider

C02 makes zero real Provider calls. Parent real G5-03 feature proof remains pending honestly.

## Next

Only after C02 Independent Review PASS and G5-03M1 Engineering closeout:

`G5-03M2 Minimal Stable Actor Materialization / Registry Expansion`

Temporary Kimi implementation routing remains active through 2026-09-06 23:59 (+08:00); correct in-flight work may finish afterward.
