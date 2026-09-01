---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-01
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-08M1 Public d20 Expansion Mechanism
current_execution_task: G4-08M1C01 NO_CHECK Action Idempotency Correction
semantic_owner: GPT
current_execution_owner: Codex
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Current state

```text
G4-07 First Playable A                PASS / CLOSED
G4-08 Expansion Pack v0.1             ACTIVE
G4-08S0 Expansion Semantic Freeze     PASS / CLOSED
G4-08M1 Public d20 Mechanism          CORRECTION REQUIRED
G4-08M1C01 NO_CHECK Idempotency       ACTIVE — CODEX
G4-08B UI Integration                 NOT YET
G4-GATE                               NOT YET
```

---

## 2. Read first

Governance:

1. `my world/MY_WORLD_CURRENT_STATUS.md`
2. `my world/architecture/source/G4_EXPANSION_V0_1_PUBLIC_D20_DECISION.md`

Implementation:

3. `AGENTS.md`
4. `docs/g4_08m1/G4-08M1_INDEPENDENT_REVIEW.md`
5. `docs/tasks/G4-08M1C01_NO_CHECK_ACTION_IDEMPOTENCY_CORRECTION_TASK.md`
6. `src/行动判定/L2_流程层/公开D20行动判定流程.gd`
7. `tests/g4_08m1/公开D20机制测试.gd`

---

## 3. Reviewed M1 implementation

Reviewed HEAD:

`31eca597d144c7c1214ddcc114d718a45fabf9dd`

Broadly correct/accepted in shape:

- third Source type `expansion_pack.v0.1` through Managed Library;
- exact generation selection/current/exact retention;
- Composition `0..N` and capability-slot conflict;
- Final Create exact Expansion provenance/rules/binding;
- SQLite remains v4;
- CHECK_REQUIRED strict proposal/freeze → Program RNG/result → durable check → second Provider narrative;
- retry/restart of a real check never rerolls;
- real Han + Afterglow DeepSeek evidence;
- no-Expansion G4-07 regression;
- no executable Source code;
- no UI changes.

Do not generically reopen those seams for correction-01.

---

## 4. IR blocker

The Expansion-enabled `NO_CHECK` branch is not durably replay-safe by stable `action_id`.

Current process checks prior completion only through durable Public d20 check records. A NO_CHECK result directly accepts Conversation and creates no check or other durable action-id marker.

Therefore this sequence can duplicate a turn:

```text
same action_id submitted
→ valid NO_CHECK + narrative
→ Conversation durable COMMIT
→ success ACK lost / process later reopened
→ same action_id retried
→ no durable action marker found
→ Provider called again
→ duplicate Player/GM turn possible
```

The focused suite only proves first NO_CHECK execution is one Provider call/zero RNG; it does not replay the same accepted NO_CHECK action_id.

This is blocking because the caller provides stable action identity before the Provider chooses NO_CHECK vs CHECK_REQUIRED. Both branches must be exactly-once.

---

## 5. Current correction

Packet:

`my-world/docs/tasks/G4-08M1C01_NO_CHECK_ACTION_IDEMPOTENCY_CORRECTION_TASK.md`

Formal Code Base:

`31eca597d144c7c1214ddcc114d718a45fabf9dd`

Owner: **Codex**. Reviewer: **GPT**. Correction budget: **correction-01**.

Required proof:

- first NO_CHECK stays one Provider call, zero RNG;
- same-process accepted replay → zero Provider/RNG/Conversation additions;
- fresh-process/reopen accepted replay → zero Provider/RNG/Conversation additions;
- same action_id + changed text → fail loud;
- pre-result failure remains retryable;
- lost-ACK window after durable NO_CHECK result but before Conversation acceptance recovers without Provider;
- lost-ACK window after Conversation acceptance but before final marker recovers without Provider/duplicate turn;
- CHECK_REQUIRED no-reroll path remains green;
- no-Expansion G4-07 remains green;
- no UI changes; schema v4 preferred.

Do not fake a NO_CHECK as a dice check.

---

## 6. After Codex returns

GPT must refresh current `main`, inspect actual correction code/evidence, and decide:

```text
PASS → G4-08M1 PASS / CLOSED → issue G4-08B to Kimi
```

or, only if the same seam still fails:

```text
correction-02 → audit neighboring action identity seam
```

Do not activate Kimi before M1C01 PASS.

---

## 7. Later product route

```text
G4-08M1C01 Codex
→ GPT IR
→ G4-08B Kimi UI integration
→ GPT IR
→ G4-09 First Playable B
→ Owner UAT B
```

G4-08 is not yet Product PASS and G4-GATE is not yet complete.
