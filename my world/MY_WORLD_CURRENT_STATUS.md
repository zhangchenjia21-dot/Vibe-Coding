---
title: my world｜当前状态
status: current-project-status
version: 12.2
created: 2026-08-26
updated: 2026-09-03
phase: G5 World Semantics & GM Runtime
current_task: G5-03M1R01 Agency Scheduler v0.3 Simplification Redesign
current_owner: KIMI
parent_task: G5-03M1 Multi-Actor Agency
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
G5-03M1C01 Currentness correction           HISTORICAL PARTIAL PASS
G5-03M1C02 Semantic-vs-Agency correction    SUPERSEDED / DO NOT EXECUTE
G5-03M1R01 Agency Scheduler v0.3            ACTIVE — KIMI
G5-03M2 Stable Actor Materialization        NOT YET
G5-04 Event / Priority Evolution            NOT YET
G5-GATE                                     NOT YET
```

## 2. Why R01 redesign

The same semantic/Agency currentness seam required a second correction. Under the project correction budget:

> **same seam still fails → redesign**

Owner approved relaxing the optimization constraints rather than continuing to patch them.

Do **not** roll back G5-03 implementation commits. Preserve the passing downstream Multi-Actor execution/runtime and replace only the upstream scheduler/selection coupling.

Canonical decision:

`architecture/world/G5_AGENCY_SCHEDULER_V0_3_DECISION.md`

Historical v0.2 decision is superseded.

## 3. Current canonical flow

```text
free-form Narrative
→ durable Conversation acceptance
→ G5-01/G5-02 semantic lane: changes + knowledge only
→ mark Agency dirty
→ foreground idle + semantic queue settled
→ standalone lightweight Agency Selector over latest current world snapshot
→ 0..4 validated stable actors
→ separate concurrent actor-scoped executions
→ 0..N durable sibling actor actions
```

Agency is a best-effort evaluation of the latest current world, not a mandatory child cycle for every player turn.

Fast A→B→C player progress may coalesce into one later Agency evaluation of C; no historical Agency catch-up queue is required.

## 4. What is preserved

R01 must reuse/preserve where possible:

- multi-actor concurrent per-actor execution;
- actor-private Source/current Knowledge/own agency history;
- serialized `commit_world_mutation_durably(...)` sibling commits;
- same-cycle sibling expected-head progression;
- foreground/Restore cancellation;
- stale Knowledge/Agency History filtering;
- same-version replay no duplicate actor execution/mutation;
- bounded later GM Context `Independent Actor Actions`;
- no automatic Player/other-actor knowledge grant.

## 5. What is removed

The semantic-analysis result no longer owns Agency Selection.

Remove the current production dependency:

```text
semantic response agency_candidates
→ Application starts Agency
```

Semantic materialization returns to its own validity semantics. An older accepted source turn may still materialize valid changes/knowledge if its accepted GM hash remains current at that index; foreground advancement only makes the old Agency opportunity obsolete.

## 6. Standalone selector

Selector may run only when:

- Session ready;
- Agency dirty;
- foreground idle;
- semantic worker has zero active/queued work;
- no selector/cycle already active.

It may use bounded GM-level current-world context because it only decides who receives an action evaluation. Actor execution remains private/actor-scoped, so selector omniscience does not become actor omniscience.

Output:

```json
{"actors":["stable-id-a","stable-id-b"]}
```

Validate Guaranteed-NPC stable IDs, dedupe, reject Player/unknown, cap 4. No round-robin fallback/retry loop.

## 7. Foreground / Restore behavior

New foreground attempt cancels active selector + remaining uncommitted actor work and abandons that obsolete Agency opportunity. Already committed actions remain durable. A later successfully accepted turn marks dirty again.

Restore/Recovery/session close cancels active background Agency and clears obsolete dirty state. Loading a Save does not automatically fire Agency.

Before using selector output, frozen latest accepted turn/hash + world head must still match current state and foreground must remain idle.

## 8. Current packet

`my-world/docs/tasks/G5-03M1R01_AGENCY_SCHEDULER_V0_3_SIMPLIFICATION_REDESIGN_TASK.md`

Do not execute the superseded C02 packet.

## 9. Provider status / authorization

Parent v0.2 real proof remains historically pending due Provider unavailability.

R01 is a new approved redesign task. After deterministic gates are green it may use at most:

```text
1 real standalone selector request
+
up to 2 real actor execution requests
```

Stub Narrative/semantic prerequisites. No real Narrative call solely to reach Agency, no retry/fallback/hidden Provider switch. External outage leaves reality proof pending without blocking code review.

## 10. Next slice

After R01 Independent Review PASS and G5-03M1 Engineering closeout:

```text
G5-03M2 Minimal Stable Actor Materialization / Registry Expansion
```

This remains justified by the Owner requirement that important non-Guaranteed named actors such as Cao Cao / Zhuge Liang can enter Agency once stable Game-local identity/material exists.

## 11. Temporary routing

Through 2026-09-06 23:59 (+08:00): GPT owns architecture/review; Kimi owns code-changing implementation. Correct in-flight Kimi work may finish after expiry.

## 12. Route

```text
Kimi G5-03M1R01 Agency Scheduler v0.3
→ GPT Independent Review
→ close G5-03M1 Engineering if PASS
→ G5-03M2 Stable Actor Materialization
```

Visual runtime remains deferred to G6.
