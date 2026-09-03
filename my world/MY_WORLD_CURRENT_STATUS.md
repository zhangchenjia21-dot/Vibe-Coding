---
title: my world｜当前状态
status: current-project-status
version: 11.1
created: 2026-08-26
updated: 2026-09-03
phase: G5 World Semantics & GM Runtime
current_task: G5-01M1 World Turn / Semantic Materialization Spine
current_owner: CODEX
parent_task: G5-01 Minimum Playable T0 + World Turn / Semantic Materialization
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
G4-01 ... G4-09                       PASS / CLOSED
G4-10 Runtime Asset Resolution        DEFERRED / MOVED TO G6
G4-10M1 Mechanism                     SUPERSEDED / DO NOT EXECUTE
G4-11 Two Primary Asset Families      PASS / CLOSED
G4-11P1 Engineering Reality Prep      PASS / CLOSED
G4-11UAT Owner Reality Test           PASS / CLOSED
G4-11C01 Narrative Voice Soft Prompt  PASS / CLOSED
G4-GATE                               PASS

G5 World Semantics & GM Runtime       ACTIVE
G5-01 Minimum Playable T0 + World Turn / Semantic Materialization
                                      ACTIVE
G5-01M1 Semantic Materialization Spine ACTIVE — CODEX
G5-02 Knowledge Provenance            NOT YET
G5-03 NPC / Faction Agency            NOT YET
G5-04 Event / Priority Evolution      NOT YET
G5-GATE                               NOT YET
```

Do not start G5-02 before G5-01 implementation review + Owner product checkpoint.

## 2. Standing authorization｜Required real Provider validation

Owner explicitly authorized future **bounded real Provider validation that is required by an approved my world Task Packet**.

Canonical decision:

`architecture/foundation/REAL_PROVIDER_VALIDATION_STANDING_AUTHORIZATION.md`

Effect:

```text
approved Task Packet requires real Provider evidence
+ task defines bounded scenario/call/turn/attempt ceiling
+ current approved Provider/profile
→ PRE-AUTHORIZED
→ execution Agent proceeds without asking Owner again
```

Future Task Packets requiring real Provider evidence must cite this standing authorization and specify the smallest reasonable call/turn/attempt ceiling.

This authorization removes repeated permission interruptions. It does not authorize open-ended benchmarks, Provider/model switching, billing/account changes, secrets/credentials in prompts, unrelated private data, new external services or other scope expansion.

G5-01M1 §12.E is already covered. Codex should execute the bounded real-selected-Provider proof after offline gates are green without another Owner confirmation.

## 3. G4 final closeout

G4-11 Owner result:

`my-world/docs/g4_11/G4-11UAT_OWNER_RESULT.md`

Owner confirmed that Han/刘备 and Afterglow/莉维娅 materially feel like different RPG worlds. Cross-world narrative prose voice similarity remains a non-blocking quality finding.

G4-11C01 Independent Review:

`my-world/docs/g4_11/G4-11C01_INDEPENDENT_REVIEW.md`

Reviewed implementation:

- production change only adjusted shared `GM_INSTRUCTIONS` wording;
- Opening and continuation receive the same generic soft narrative-voice guidance;
- no Source migration, Provider change, style classifier, keyword gate, reject/retry behavior or G5 semantics were introduced;
- no standalone Owner UAT was required; the style effect is deferred to the next suitable product UAT.

Therefore:

```text
G4-11C01 PASS / CLOSED
→ G4-11 PASS / CLOSED
→ G4-GATE PASS
→ G4 PASS / CLOSED
```

## 4. Visual work remains deferred

Owner decision remains protected:

```text
G4-10 Runtime Asset Resolution
DEFERRED / MOVED TO G6
```

Canonical decision:

`architecture/source/G4_VISUAL_ASSET_DEFERRAL_TO_G6_DECISION.md`

G5 must remain independent of portrait / scene / authored-map presentation. Map image still does not equal topology / travel / current location / GIS.

## 5. G5-01 semantic freeze

Canonical decision:

`architecture/world/G5_WORLD_TURN_SEMANTIC_MATERIALIZATION_V0_1_DECISION.md`

Core product transition:

```text
G4:
Source-grounded durable AI RPG conversation

G5:
accepted Narrative can produce durable game-local world semantics
```

Protected ordering:

```text
Player action
→ free-form visible GM Narrative streaming
→ durable Conversation acceptance
→ separate best-effort Semantic Materialization Lane
→ optional durable World Turn mutation
```

Narrative acceptance does **not** depend on semantic-analysis success.

## 6. Current task｜G5-01M1

Implementation packet:

`my-world/docs/tasks/G5-01M1_WORLD_TURN_SEMANTIC_MATERIALIZATION_TASK.md`

Owner: **CODEX**.

Required first vertical:

```text
accepted ordinary player/GM turn
→ one isolated semantic analysis request
→ 0..N durable newly-established consequences
→ Program-owned World Turn record
→ existing atomic world mutation / Timeline
→ matching committed consequences can re-enter later Context
```

Use current selected Provider/profile. No cross-provider fallback.

The machine-analysis lane may use small structured data, but malformed/transport/empty analysis must fail-soft:

```text
Conversation remains accepted
no fake World Turn
no retry loop
play continues
```

## 7. World Turn v0.1 boundary

Conceptual durable namespace:

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

This is a turn-level consequence ledger, not a universal ontology.

Do not pre-build G5-02/03/04 Domains inside M1.

Regenerate/correction safety requirement:

> a semantic record whose source GM hash no longer matches the currently accepted Conversation entry must never be projected into model Context.

## 8. Existing runtime seams to reuse

Current implementation already has:

- exact T0 Source materialization from Final Create;
- durable accepted Conversation;
- `Current Game Session Runtime.commit_world_mutation_durably(...)`;
- Timeline/current-head snapshots;
- Save/Restore/Recovery;
- one Game = one SQLite.

G5-01 must extend these seams rather than introduce a second persistence owner.

## 9. Protected cross-stage principles

```text
Model Freedom First
+
Visible Narrative First
+
Canonical Commit Behind durable infrastructure boundaries
```

and:

> **Model authors the world; Runtime makes it durable; Player owns the timeline.**

G5-01 must not add narrative JSON framing, mandatory prose format, output keyword validation or a semantic-extraction gate before visible play can continue.

## 10. Route after G5-01M1

If GPT Independent Review passes M1:

```text
G5-01UAT short Owner product checkpoint
ACTIVE — OWNER
```

The UAT should verify one simple lived consequence remains part of the world after later play/reopen while Narrative remains free-form. The deferred G4-11 narrative-voice soft-prompt effect may be observed in the same UAT without creating a separate gate.

Only after G5-01 Owner PASS may GPT close G5-01 and shape G5-02 Knowledge Provenance.
