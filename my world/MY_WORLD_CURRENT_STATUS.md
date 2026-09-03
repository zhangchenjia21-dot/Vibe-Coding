---
title: my world｜当前状态
status: current-project-status
version: 11.7
created: 2026-08-26
updated: 2026-09-03
phase: G5 World Semantics & GM Runtime
current_task: G5-02M1C01 Actor Roster + Recent Knowledge Projection Correction
current_owner: KIMI
parent_task: G5-02M1 Known-Actor Knowledge Provenance Spine
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
G3 Persistence / Save / Timeline      PASS / CLOSED
G4 Primary Source Assets & Local Game Creation
                                      PASS / CLOSED
G4-10 Runtime Asset Resolution        DEFERRED / MOVED TO G6
G4-11 Two Primary Asset Families      PASS / CLOSED
G4-11C01 Narrative Voice Soft Prompt  PASS / CLOSED
G4-GATE                               PASS

G5 World Semantics & GM Runtime       ACTIVE
G5-01 World Turn / Semantic Materialization
                                      PASS / CLOSED
G5-01M1 Semantic Materialization Spine ENGINEERING PASS / CLOSED
G5-01M1C02 Restore Timeline Isolation CANCELLED / DO NOT EXECUTE
G5-02 Knowledge Provenance            ACTIVE
G5-02M1 Known-Actor Knowledge Spine   CORRECTION REQUIRED
G5-02M1C01 Actor Roster + Recent Knowledge Projection
                                      ACTIVE — KIMI
G5-02 real Provider vertical          PENDING / EXTERNAL PROVIDER UNAVAILABLE
G5-03 NPC / Faction Agency            NOT YET
G5-04 Event / Priority Evolution      NOT YET
G5-GATE                               NOT YET
```

Do not start G5-03 before G5-02M1C01 Independent Review and G5-02 closeout.

## 2. G5-02M1 reviewed implementation

Kimi pushed:

```text
IMPLEMENTATION/EVIDENCE_HEAD 492815aefd127c20bd17fbda892aad8d41279dcb
```

Evidence:

`my-world/docs/g5_02/G5-02M1_KNOWN_ACTOR_KNOWLEDGE_PROVENANCE_EVIDENCE.md`

Formal Independent Review:

`my-world/docs/g5_02/G5-02M1_INDEPENDENT_REVIEW.md`

Result:

```text
CORRECTION REQUIRED / NOT ENGINEERING PASS YET
```

The architecture is accepted: one existing auxiliary semantic-analysis request, isolated knowledge parsing, stable roster validation before persistence, at most one atomic world mutation, bounded soft Actor Knowledge Context, no Narrative gate/schema/platform creep.

## 3. Blocking findings

### A. Model is not given the exact actor IDs it must return

The production semantic-analysis request requires `knowledge_events[*].knower_id` to equal a stable Game-local `local_character_id`, but the request currently only includes accepted Player Action + GM Narrative. It does not include the Player/Guaranteed-NPC local-ID roster.

Deterministic stub tests can hand-write IDs, but a real model cannot reliably emit an exact ID it has never been shown.

### B. Knowledge Context is bounded in the wrong direction

Current projection sorts oldest → newest and consumes the event cap from the front. After eight events, later new knowledge can be omitted while early knowledge remains forever. The frozen decision requires a bounded recent working set.

### C. Real-provider harness can false-pass without knowledge

The current harness accepts `no_changes` and permits an empty knowledge record set to satisfy the boundary check. A future healthy Provider run must require at least one committed valid roster knowledge event and its later Context projection before feature-specific PASS.

## 4. Current correction

Packet:

`my-world/docs/tasks/G5-02M1C01_ACTOR_ROSTER_RECENCY_CORRECTION_TASK.md`

Required outcome:

- add a compact Player + Guaranteed-NPC display-name/local-ID roster to the **same existing** semantic-analysis request;
- keep one auxiliary Provider call per accepted ordinary turn;
- select newest matching knowledge within the existing bounded Context cap;
- update the real-provider harness so empty/no-knowledge output cannot count as G5-02 proof;
- preserve knowledge parse isolation and single atomic mutation semantics.

No extra real Provider call is allowed in this correction. The parent packet's one-attempt ceiling has already been consumed by an ordinary-Narrative timeout.

## 5. Provider status

```text
G5-02M1 real selected-Provider vertical
PENDING / EXTERNAL PROVIDER UNAVAILABLE
```

The single bounded attempt timed out during ordinary Narrative before feature-specific knowledge materialization. No fallback/hidden Provider switch/second attempt is allowed.

## 6. Temporary execution routing｜through 2026-09-06

```text
GPT        → semantics / architecture / task shaping / final Independent Review
Kimi       → primary code-changing implementation owner, temporarily substituting for Codex
Grok Build → external research / evidence support when useful
Owner      → Product UAT / explicit product verdict
```

The override auto-expires at 2026-09-07 00:00 (+08:00). Correct in-flight Kimi work may finish under its issued packet.

## 7. Protected semantics

```text
Game / World Truth
!= actor knowledge
!= human-player disclosure
!= omniscient GM model context
```

> **GM omniscience must not become actor omniscience.**

G5-02M1 remains limited to post-T0 acquisition for the Player Character + Guaranteed NPC stable local IDs. No generic Knowledge Graph, Faction knowledge, emergent NPC identity, false-belief engine, UI, G5-03 Agency or G6/G7 work.

## 8. Route

```text
Kimi G5-02M1C01
→ GPT Independent Review
→ if PASS, close G5-02
→ shape G5-03 NPC / Faction Agency
```
