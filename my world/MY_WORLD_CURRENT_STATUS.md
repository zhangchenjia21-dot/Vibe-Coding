---
title: my world｜当前状态
status: current-project-status
version: 11.4
created: 2026-08-26
updated: 2026-09-03
phase: G5 World Semantics & GM Runtime
current_task: G5-01M1C02 Restore Timeline Isolation Correction
current_owner: KIMI
parent_task: G5-01M1 World Turn / Semantic Materialization Spine
semantic_owner: GPT
owner_uat_required: false
context_handoff: handoff/GPT_CONTEXT_HANDOFF_CURRENT.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                         PASS / CLOSED
G2 AI Conversation Spine              PASS / CLOSED
G2-GATE                               PASS
G3 Persistent Game / Save / Timeline PASS / CLOSED
G3-GATE                               PASS

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
G5-GATE                               NOT YET
```

Do not start G5-02 before G5-01M1 correction Independent Review + G5-01 Owner/product checkpoint.

## 2. Current Independent Review result

Reviewed implementation:

```text
IMPLEMENTATION_HEAD  eb171a19dd0b4eeb134392128fb8df7fd5b104cb
EVIDENCE_HEAD        f9b1be01bd102f3bb1ae6b0b762a6b97d3a5b6f1
```

Formal review:

`my-world/docs/g5_01/G5-01M1_INDEPENDENT_REVIEW.md`

Result:

```text
G5-01M1 Engineering PASS
NOT YET

focused correction required
```

The parent architecture is accepted: free-form Narrative is durably accepted before the independent semantic lane matters; semantic failure remains fail-soft; valid change uses the existing atomic world-mutation/Timeline owner; Context projection is bounded and hash-matching; no narrative protocol gate, SQLite schema migration, universal ontology, Source mutation, G5-02+ or G6 scope was introduced.

## 3. Blocking finding｜Restore timeline-local semantic state

Current semantic worker holds ephemeral state including:

```text
_attempted_versions
_queue
_active
```

Current Runtime already emits `restore_completed` after a committed Save/Recovery progress switch, but the semantic worker does not observe that boundary.

Concrete defect:

```text
future Turn N semantic version attempted
→ Restore to earlier timeline before Turn N
→ durable Conversation/world snapshot correctly rewinds
→ _attempted_versions still remembers abandoned future version
→ player later reaches exact same Turn N / GM hash again
→ worker reports already_attempted
→ no semantic request / no World Turn
```

This violates:

> **Player owns the timeline.**

The existing Timeline test proves restored-away semantic records do not re-enter Context, but does not prove abandoned-future attempt suppression cannot affect later restored play.

## 4. Current correction task

Packet:

`my-world/docs/tasks/G5-01M1C02_RESTORE_TIMELINE_ISOLATION_CORRECTION_TASK.md`

Owner: **KIMI** under the temporary execution routing.

Correction ceiling:

- use existing `restore_completed` or equivalent existing seam;
- abandoned-future attempt suppression must not survive Restore;
- queued pre-Restore work must be discarded/quarantined;
- active pre-Restore analysis must be unable to commit after Restore, including late callback;
- Restore itself launches no semantic Provider request;
- exact same accepted future version after Restore can materialize again;
- no real Provider call for this correction;
- preferably production change only in `src/世界回合/L2_流程层/语义物化流程.gd` plus tests/evidence;
- no SQLite schema/UI/Source/Runtime Model Settings/Public d20/G5-02+/G6 changes.

## 5. Temporary execution routing｜through 2026-09-06

Canonical decision:

`architecture/foundation/TEMPORARY_EXECUTION_ROUTING_2026-09-03_TO_2026-09-06.md`

Through 2026-09-06 23:59 (+08:00):

```text
GPT        → semantics / architecture / task shaping / final Independent Review
Kimi       → primary code-changing implementation owner, temporarily substituting for Codex
Grok Build → external research / evidence discovery / Provider-reality support / secondary cross-check
Owner      → Product UAT
```

The override auto-expires at 2026-09-07 00:00 (+08:00). Correct in-flight Kimi work may finish under its issued packet.

## 6. Provider authorization + outage state

Canonical decision:

`architecture/foundation/REAL_PROVIDER_VALIDATION_STANDING_AUTHORIZATION.md`

Required bounded real Provider validation remains pre-authorized in future packets and must not trigger repeated Owner permission questions.

Parent G5-01M1 real selected-Provider vertical remains:

```text
PENDING / EXTERNAL PROVIDER UNAVAILABLE
```

The two bounded Kimi K3 requests timed out during ordinary Narrative after 420 seconds, before the semantic lane executed. No fallback/hidden switch/third attempt occurred. This external outage is not the cause of the current correction.

G5-01M1C02 must make **no** real Provider call.

## 7. G5-01 semantic contract remains frozen

Canonical decision:

`architecture/world/G5_WORLD_TURN_SEMANTIC_MATERIALIZATION_V0_1_DECISION.md`

Protected ordering:

```text
Player action
→ free-form visible GM Narrative streaming
→ durable Conversation acceptance
→ separate best-effort Semantic Materialization Lane
→ optional durable World Turn mutation
```

Narrative acceptance does **not** depend on semantic-analysis success.

Conceptual namespace remains:

```text
living_world
  schema_version = living_world.v0.1
  semantic_turns_by_index
    <turn_index>
      world_turn_id
      source_turn_index
      source_gm_sha256
      materialized_at
      changes[]
```

This is a bounded turn-level consequence ledger, not a universal ontology.

## 8. Route after correction

```text
Kimi implements/pushes G5-01M1C02
→ GPT Independent Review
```

If Engineering PASS is then achieved while the real Provider vertical remains pending, G5-01 product/reality acceptance still remains open. A later successful real run or short Owner UAT must prove one lived consequence survives later play/reopen and re-enters Context while Narrative remains free-form.

Only after G5-01 product/reality PASS may GPT close G5-01 and shape G5-02 Knowledge Provenance.

Visual runtime remains deferred to G6.
