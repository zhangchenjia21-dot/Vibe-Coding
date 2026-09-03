---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-03
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G5-01M1 World Turn / Semantic Materialization Spine
current_execution_task: G5-01M1C02 Restore Timeline Isolation Correction
semantic_owner: GPT
current_execution_owner: KIMI
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Current state

```text
G4 Primary Source Assets & Local Game Creation
                                      PASS / CLOSED
G4-10 Runtime Asset Resolution        DEFERRED / MOVED TO G6
G4-11 Two Primary Asset Families      PASS / CLOSED
G4-11C01 Narrative Voice Soft Prompt  PASS / CLOSED
G4-GATE                               PASS

G5 World Semantics & GM Runtime       ACTIVE
G5-01 Minimum Playable T0 + World Turn / Semantic Materialization
                                      ACTIVE
G5-01M1 Semantic Materialization Spine CORRECTION REQUIRED
G5-01M1C02 Restore Timeline Isolation ACTIVE — KIMI
G5-01 real Provider vertical          PENDING / EXTERNAL PROVIDER UNAVAILABLE
G5-02 Knowledge Provenance            NOT YET
G5-03 NPC / Faction Agency            NOT YET
G5-04 Event / Priority Evolution      NOT YET
```

Do not start G5-02 before G5-01M1 correction review + Owner/product checkpoint.

## 2. Temporary execution routing until 2026-09-07

Canonical decision:

`my world/architecture/foundation/TEMPORARY_EXECUTION_ROUTING_2026-09-03_TO_2026-09-06.md`

Through 2026-09-06 23:59 (+08:00):

```text
GPT        → meaning / architecture / governance / task shaping / final Independent Review
Kimi       → primary code-changing implementation owner; temporary Codex substitute
Grok Build → external research / evidence discovery / Provider-reality support / secondary technical cross-check
Owner      → Product UAT / explicit product verdict
```

The override auto-expires at 2026-09-07 00:00 (+08:00). Correct in-flight Kimi work may finish under its issued packet.

## 3. Parent G5-01M1 review result

Reviewed:

```text
IMPLEMENTATION_HEAD  eb171a19dd0b4eeb134392128fb8df7fd5b104cb
EVIDENCE_HEAD        f9b1be01bd102f3bb1ae6b0b762a6b97d3a5b6f1
```

Formal review:

`my-world/docs/g5_01/G5-01M1_INDEPENDENT_REVIEW.md`

Result:

```text
CORRECTION REQUIRED / NOT ENGINEERING PASS YET
```

The main architecture is acceptable: Narrative durable acceptance precedes the independent semantic lane; semantic failure is fail-soft; valid changes use existing atomic world mutation/Timeline; Context is bounded and matching; no narrative protocol gate, persistence schema migration, universal ontology, G5-02+ or G6 work was introduced.

## 4. Blocking Restore finding

`SemanticMaterializationProcess` keeps timeline-local ephemeral state:

```text
_attempted_versions
_queue
_active
```

Current Runtime emits `restore_completed` after committed Save/Recovery progress switch, but the semantic worker does not observe it.

Failure sequence:

```text
future Turn N version V attempted
→ Restore before Turn N
→ durable Conversation/world_state rewind correctly
→ _attempted_versions still remembers V
→ player later reaches exact same Turn N / GM hash
→ already_attempted
→ no new semantic request / no World Turn
```

This is abandoned-future execution state affecting the restored timeline and violates:

> **Player owns the timeline.**

A pre-Restore active/queued semantic request is also not explicitly invalidated at the progress-switch boundary. Cancellation alone is insufficient; a late completion must be incapable of committing into the restored timeline.

## 5. Current correction packet

`my-world/docs/tasks/G5-01M1C02_RESTORE_TIMELINE_ISOLATION_CORRECTION_TASK.md`

Owner: **Kimi**.

Required proof:

1. Save at timeline T;
2. future exact player/GM version is attempted/materialized;
3. Restore to T removes that future;
4. recreate the exact same future player/GM version;
5. a fresh semantic request is allowed and can commit again;
6. active pre-Restore analysis cannot commit after Restore even with a late callback;
7. queued pre-Restore work is discarded/quarantined;
8. Restore itself launches no semantic request.

Expected production scope is preferably only `src/世界回合/L2_流程层/语义物化流程.gd` plus focused tests/evidence. Use existing `restore_completed`; do not add a new persistence mechanism unless a blocker is returned first.

No real Provider call is required for this correction.

## 6. Provider status

Standing authorization/outage decision:

`my world/architecture/foundation/REAL_PROVIDER_VALIDATION_STANDING_AUTHORIZATION.md`

Parent M1 real vertical remains:

```text
PENDING / EXTERNAL PROVIDER UNAVAILABLE
```

Two bounded Kimi K3 real requests timed out in ordinary Narrative after 420 seconds before the semantic lane executed. No fallback/hidden switch/third attempt occurred.

Do not spend another Provider attempt for G5-01M1C02. The Restore invariant is deterministic and should be proven offline/SQLite.

## 7. Protected G5-01 semantics

Canonical decision:

`my world/architecture/world/G5_WORLD_TURN_SEMANTIC_MATERIALIZATION_V0_1_DECISION.md`

Core ordering:

```text
free-form visible Narrative
→ durable Conversation acceptance
→ separate best-effort semantic analysis
→ optional atomic World Turn mutation
```

Narrative acceptance remains independent of semantic-analysis success.

`living_world.v0.1` remains a bounded turn-level consequence ledger, not a universal ontology.

Do not add:

- narrative JSON/header/sentinel requirements;
- semantic-analysis acceptance gate;
- cross-provider fallback;
- Source mutation;
- SQLite schema migration;
- UI work;
- G5-02 Knowledge;
- G5-03 NPC/Faction Agency;
- G5-04 Event/Priority evolution;
- G6 visual runtime.

## 8. Review after Kimi returns

Refresh both mains and inspect actual correction diff/evidence.

Key questions:

1. Is `restore_completed` or an equivalent committed progress-switch seam actually observed?
2. Are attempted-version suppression and queued work reset/quarantined at Restore?
3. Is active pre-Restore work guarded by an epoch/version boundary so late completion cannot commit?
4. Does exact same future accepted version after Restore get a new semantic attempt?
5. Does Restore itself avoid automatic Provider analysis?
6. Are existing idempotency/correction/Save-Restore/Context tests still green?
7. Was no real Provider call made?
8. Is scope focused with no Runtime schema/UI/Source/G5-02+ creep?

If correction PASS:

```text
G5-01M1 Engineering PASS
+ real Provider vertical still PENDING
→ short G5-01 Owner/product reality checkpoint
```

Only after product/reality PASS may GPT close G5-01 and shape G5-02 Knowledge Provenance.
