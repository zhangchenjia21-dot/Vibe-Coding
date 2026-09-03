---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-03
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G5-02M1 Known-Actor Knowledge Provenance Spine
current_execution_task: G5-02M1C01 Actor Roster + Recent Knowledge Projection Correction
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
G5-02 Knowledge Provenance                      ACTIVE
G5-02M1 Known-Actor Knowledge Spine             CORRECTION REQUIRED
G5-02M1C01 Actor Roster + Recent Projection     ACTIVE — KIMI
G5-02 real Provider vertical                    PENDING / EXTERNAL PROVIDER UNAVAILABLE
G5-03 NPC / Faction Agency                      NOT YET
```

Do not start G5-03 before G5-02M1C01 review + G5-02 closeout.

## 2. Reviewed G5-02M1 implementation

Kimi implementation/evidence HEAD:

`492815aefd127c20bd17fbda892aad8d41279dcb`

Formal review:

`my-world/docs/g5_02/G5-02M1_INDEPENDENT_REVIEW.md`

The main architecture passed review in principle:

- reuses the existing single semantic-analysis request;
- Narrative acceptance remains independent/fail-soft;
- knowledge parse failure does not invalidate valid G5-01 changes;
- non-roster actor IDs are rejected before persistence;
- knowledge-only and combined results use at most one atomic world mutation;
- no SQLite schema, Source migration, generic graph, UI or G5-03+ scope.

## 3. Blocking findings

### A — missing actor roster in semantic request

The semantic response requires exact stable Game-local `knower_id` values, but production `_analysis_messages(...)` only sends accepted Player Action + GM Narrative. It does not send the Player Character / Guaranteed NPC local IDs or names.

A real model therefore cannot reliably return an exact ID it has never seen. Stub tests passed by hand-writing IDs.

### B — projection keeps oldest knowledge after the cap

Knowledge records are sorted oldest → newest and the event cap is consumed from the front. Once eight events exist, later new knowledge can be omitted from GM Context. v0.1 requires a bounded recent working set.

### C — real harness can false-pass with empty knowledge

The current real harness accepts `no_changes` and allows empty knowledge records to satisfy the Context boundary check. A future feature-specific PASS must require actual committed valid knowledge provenance and later Context projection.

## 4. Current correction packet

`my-world/docs/tasks/G5-02M1C01_ACTOR_ROSTER_RECENCY_CORRECTION_TASK.md`

Required:

1. derive current stable actor roster from Game-local world state;
2. include compact display-name + exact local ID roster in the **same existing** semantic-analysis request;
3. do not add a second Provider request;
4. project newest matching knowledge within the existing cap, optionally rendering selected events chronologically;
5. update the real-provider harness so no/empty knowledge cannot count as feature PASS;
6. preserve all existing G5-01/G5-02 deterministic semantics.

## 5. Provider rule for correction

Do **not** make another real Provider call.

The parent G5-02M1 packet allowed one bounded task-owned selected-Provider attempt. It was consumed and timed out during ordinary Narrative before the knowledge lane could be proven.

```text
real G5-02 vertical
PENDING / EXTERNAL PROVIDER UNAVAILABLE
```

No fallback, hidden Provider switch or second attempt.

## 6. Temporary routing

Through 2026-09-06 23:59 (+08:00):

```text
GPT        → semantics / architecture / governance / final Independent Review
Kimi       → code-changing implementation owner
Grok Build → external research/evidence support only when useful
Owner      → Product UAT / explicit verdict
```

Auto-expiry: 2026-09-07 00:00 (+08:00). Correct in-flight Kimi work may finish under its issued packet.

## 7. Protected semantics

```text
Game / World Truth
!= actor knowledge
!= human-player disclosure
!= omniscient GM model context
```

> **GM omniscience must not become actor omniscience.**

No Narrative JSON/sentinel gate, no knowledge-output classifier, no Faction knowledge, emergent NPC identity, false-belief/rumor engine, generic Knowledge Graph, G5-03 Agency, UI, G6 visuals or G7 retrieval platform.

## 8. When Kimi returns

Refresh both mains and inspect actual correction diff/evidence. Verify:

- exact roster appears in emitted semantic-analysis messages;
- unknown/incidental actors do not appear in supplied roster;
- only one analysis request occurs;
- >8 knowledge events retains newest and evicts bounded-old events;
- real harness now requires non-empty valid knowledge provenance + later Context;
- G5-02 A–F + G5-01 regressions remain green;
- no real Provider call was made;
- `git diff --check` PASS.

If correction PASS, close G5-02 and shape G5-03 NPC / Faction Agency.
