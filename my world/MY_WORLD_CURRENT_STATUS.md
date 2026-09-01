---
title: my world｜当前状态
status: current-project-status
version: 8.0
created: 2026-08-26
updated: 2026-09-01
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-08M1C01 NO_CHECK Action Idempotency Correction
current_owner: Codex
parent_task: G4-08M1 Public d20 Expansion Mechanism
semantic_owner: GPT
owner_uat_required: false
context_handoff: handoff/GPT_CONTEXT_HANDOFF_CURRENT.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. Current

```text
G1 Foundation                         PASS / CLOSED
G2 AI Conversation Spine              PASS / CLOSED
G2-GATE                               PASS
G3 Persistent Game / Save / Timeline PASS / CLOSED
G3-GATE                               PASS
G4-01 Application Shell / Lifecycle  PASS / CLOSED
G4-02R1 Source semantic re-audit      PASS / CLOSED
G4-03 Managed Local Source Library    PASS / CLOSED
G4-04 Multi-Game / Game Library       PASS / CLOSED
G4-05 Asset-only New Game Wizard      PASS / CLOSED
G4-06 Atomic Final Create             PASS / CLOSED
G4-07 First Playable A                PASS / CLOSED
G4-07A Opening Runtime                PASS / CLOSED
G4-07B Playable UI Integration        PASS / CLOSED
G4-07UAT01 Owner Launch Freshness     PASS / CLOSED
G4-08 Expansion Pack v0.1             ACTIVE
G4-08S0 Expansion Semantic Freeze     PASS / CLOSED
G4-08M1 Public d20 Mechanism          CORRECTION REQUIRED
G4-08M1C01 NO_CHECK Idempotency       ACTIVE — CODEX
G4-08B UI Integration                 NOT YET
G4-GATE                               NOT YET
```

Current correction packet:

`my-world/docs/tasks/G4-08M1C01_NO_CHECK_ACTION_IDEMPOTENCY_CORRECTION_TASK.md`

Formal Code Base:

`31eca597d144c7c1214ddcc114d718a45fabf9dd`

Independent Review record:

`my-world/docs/g4_08m1/G4-08M1_INDEPENDENT_REVIEW.md`

Correction budget: **correction-01**.  
Primary execution owner: **Codex**.  
Reviewer / semantic owner: **GPT**.

---

## 2. G4-08M1 reviewed implementation

Reviewed implementation HEAD:

`31eca597d144c7c1214ddcc114d718a45fabf9dd`

M1 substantially established the intended mechanism:

- `expansion_pack.v0.1` is a third first-class Source type through the existing strict contract and Managed Library;
- Public d20 exact generation installs/current/exact revalidation and immutable retention are implemented;
- Composition supports explicit `0..N` exact Expansion selections;
- duplicate exact Expansion / same exclusive `capability_slot` collision fail closed;
- Final Create pins/materializes exact Expansion provenance, authored rules and capability binding;
- no Provider call occurs during Final Create and SQLite remains schema v4;
- `CHECK_REQUIRED` uses strict JSON Proposal → validation/freeze → Program RNG → Program total/outcome → durable check → second Provider narrative;
- same risky action retry/restart reuses exact durable roll/outcome and does not reroll;
- real DeepSeek evidence exists for Han and Afterglow;
- no-Expansion G4-07 regression remains intact;
- no executable Source mechanism and no UI ownership were added.

These seams are not generically reopened by the correction.

---

## 3. Independent Review blocker

M1 is not PASS because the Expansion-enabled `NO_CHECK` branch does not preserve stable `action_id` as durable replay identity.

Current behavior is effectively:

```text
stable action_id submitted
→ Provider returns NO_CHECK + narrative
→ Conversation durably accepts Player/GM turn
→ no durable action-id completion marker exists
```

If the success acknowledgement is lost, or the Game is reopened and the caller retries the same `action_id`, the runtime cannot distinguish the already accepted NO_CHECK action from a new submission. It may call Provider again and append a duplicate Player/GM turn.

This conflicts with the frozen contract because the caller supplies the stable action identity **before** the Provider decides whether the branch is `NO_CHECK` or `CHECK_REQUIRED`.

Therefore both branches must satisfy:

```text
same action_id + same Player payload
→ at most one accepted Player/GM turn
```

---

## 4. G4-08M1C01 target

The correction is intentionally narrow.

Required final semantics for an already accepted Expansion-enabled NO_CHECK action:

```text
same action_id + same Player text
→ already accepted / replay-safe result
→ Provider calls = 0
→ RNG calls = 0
→ Conversation additions = 0
```

And:

```text
same action_id + changed Player text
→ fail loud identity/payload conflict
```

The implementation must also cover restart/lost-ACK windows around durable NO_CHECK result and Conversation acceptance without turning NO_CHECK into a fake dice check.

Required focused evidence:

- first NO_CHECK remains one Provider call / zero RNG;
- same-process replay is exactly-once;
- fresh-process/reopen replay is exactly-once;
- changed payload conflicts;
- pre-result Provider failure remains retryable;
- durable result-before-Conversation and Conversation-before-final-marker windows recover without Provider replay;
- CHECK_REQUIRED no-reroll cases remain green;
- no-Expansion G4-07 route remains unchanged;
- schema stays v4 unless narrowly justified otherwise.

---

## 5. Frozen semantic authority

Canonical decision remains:

`my world/architecture/source/G4_EXPANSION_V0_1_PUBLIC_D20_DECISION.md`

First real Expansion:

```text
Display Name  判定与检定：公开 d20
asset_id      exp.check_core.public_d20
asset_type    expansion_pack
schema        expansion_pack.v0.1
capability    action_check.public_d20.v1
slot          action_resolution
```

No semantic redesign is requested by correction-01.

---

## 6. Next progression

```text
G4-08M1C01 — Codex focused correction
→ GPT Independent Review
→ if PASS: G4-08M1 PASS / CLOSED
→ G4-08B Wizard / Review / mechanic-card UI — Kimi
→ GPT Independent Review
→ G4-09 First Playable B
→ Owner UAT B
```

Do not activate Kimi before M1C01 passes. Do not claim G4-08 PASS from backend mechanism alone. Do not start G5 before remaining G4 route and G4-GATE are complete.
