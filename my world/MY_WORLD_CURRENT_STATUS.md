---
title: my world｜当前状态
status: current-project-status
version: 12.4
created: 2026-08-26
updated: 2026-09-03
phase: G5 World Semantics & GM Runtime
current_task: G5-03M1R01C02 Dirty Opportunity Consumption + Lifecycle Proof
current_owner: KIMI
parent_task: G5-03M1R01 Agency Scheduler v0.3
semantic_owner: GPT
owner_uat_required: false
context_handoff: handoff/GPT_CONTEXT_HANDOFF_CURRENT.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G4-10 Runtime Asset Resolution              DEFERRED / MOVED TO G6

G5 World Semantics & GM Runtime             ACTIVE
G5-01 World Turn / Semantic Materialization PASS / CLOSED
G5-02 Knowledge Provenance                  PASS / CLOSED
G5-03 NPC / Faction Agency                  ACTIVE
G5-03M1 Multi-Actor Agency                  REDESIGN ACTIVE
G5-03M1C01 old currentness correction       HISTORICAL PARTIAL PASS
G5-03M1C02 semantic-vs-agency correction    SUPERSEDED / DO NOT EXECUTE
G5-03M1R01 Agency Scheduler v0.3            CORRECTION REQUIRED
G5-03M1R01C01 Scheduler Lifecycle/Snapshot  CORRECTION REQUIRED / CLOSED INTO C02
G5-03M1R01C02 Dirty Opportunity Consumption ACTIVE — KIMI
G5-03M2 Stable Actor Materialization        NOT YET
G5-04 Event / Priority Evolution            NOT YET
G5-GATE                                     NOT YET
```

## 2. Review flow

Owner temporarily declined Gemini adversarial review. Current flow remains:

```text
Kimi implementation → GPT actual-code Independent Review
```

## 3. Accepted architecture

`architecture/world/G5_AGENCY_SCHEDULER_V0_3_DECISION.md` remains canonical. Do not re-couple Agency Selection into semantic analysis.

```text
ordinary durable accepted turn
→ semantic changes + knowledge only
→ one dirty Agency opportunity
→ standalone selector over latest current world
→ 0..4 stable actors
→ concurrent actor-scoped Agency Cycle
```

## 4. R01C01 review result

Reviewed implementation HEAD:

`9da292966e5a56bbbea7ca5aedb20919d5ebb092`

Review:

`my-world/docs/g5_03/G5-03M1R01C01_INDEPENDENT_REVIEW.md`

C01 successfully added production dirty wiring, cycle cleanup, current-hash selector consequence filtering and selector child cleanup. One blocking state-machine defect remains:

```text
Turn A accepted → dirty=true
→ selector/cycle A runs
→ dirty is never consumed
→ cycle A terminal sees dirty=true
→ automatically starts selector A2 for the same opportunity
```

This violates v0.3/C01 no-auto-retry semantics. The focused test also incorrectly treats `dirty` remaining true and manual re-consider starting another selector as a no-retry proof.

The C01 “production dirty wiring” test still directly calls `mark_dirty()`, so production callback wiring is not actually proven by that focused test.

## 5. Current correction

Packet:

`my-world/docs/tasks/G5-03M1R01C02_DIRTY_OPPORTUNITY_CONSUMPTION_CORRECTION_TASK.md`

C02 must:

- consume `dirty` exactly once when a selector starts;
- make every selector/cycle terminal outcome consume that opportunity;
- allow a fresh opportunity only after a later newly accepted ordinary player turn;
- prove no selector starts between Cycle A terminal and Turn B acceptance;
- replace direct-`mark_dirty()` pseudo-production proof with the real Application/production callback seam;
- preserve C01 current-hash filtering, cleanup, multi-actor concurrency, foreground/Restore, knowledge isolation, sibling-head and replay behavior.

## 6. Provider rule

**C02 makes zero real Provider calls.** Parent G5-03 feature proof remains honestly pending.

## 7. Next

Only after C02 Independent Review PASS and G5-03M1 Engineering closeout:

`G5-03M2 Minimal Stable Actor Materialization / Registry Expansion`

Do not start M2/G5-04 early.

## 8. Temporary routing

Through 2026-09-06 23:59 (+08:00): GPT owns architecture/review; Kimi owns code-changing implementation. Correct in-flight Kimi work may finish after expiry.
