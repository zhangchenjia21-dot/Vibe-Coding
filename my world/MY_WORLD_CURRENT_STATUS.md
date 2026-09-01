---
title: my world｜当前状态
status: current-project-status
version: 8.2
created: 2026-08-26
updated: 2026-09-01
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-08BC01 Public d20 UI Projection / Fail-Loud Correction
current_owner: Kimi
parent_task: G4-08B Public d20 UI / Interaction Integration
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
G4-08M1 Public d20 Mechanism          PASS / CLOSED
G4-08M1C01 NO_CHECK Idempotency       PASS / CLOSED
G4-08B Public d20 UI Integration      CORRECTION REQUIRED
G4-08BC01 UI Projection / Fail-Loud   ACTIVE — KIMI
G4-GATE                               NOT YET
```

Current correction packet:

`my-world/docs/tasks/G4-08BC01_UI_PROJECTION_FAIL_LOUD_CORRECTION_TASK.md`

Formal Code Base:

`3a20234d06c10904c220cd1a49bf29f6ad6769e7`

Independent Review record:

`my-world/docs/g4_08b/G4-08B_INDEPENDENT_REVIEW.md`

Correction budget: **correction-01**.  
Primary execution owner: **Kimi**.  
Reviewer / semantic owner: **GPT**.

---

## 2. G4-08 backend status｜PASS / CLOSED through M1

Accepted mechanism remains frozen:

```text
exact Expansion Source
→ Managed Library
→ exact Composition selection
→ capability-slot compatibility
→ Final Create exact materialization
→ Program-owned Public d20 Host
→ strict adjudication before RNG
→ Program RNG / total / outcome
→ durable CHECK_REQUIRED or durable NO_CHECK action identity
→ retry / restart exactly once
→ real DeepSeek continuation
```

SQLite remains schema v4. No executable Source support was added.

Do not reopen M1/M1C01 absent concrete backend regression evidence.

---

## 3. G4-08B reviewed implementation

Reviewed implementation/evidence HEAD:

`3a20234d06c10904c220cd1a49bf29f6ad6769e7`

Broadly correct:

- Wizard projects installed Expansion inventory;
- exact 0..N selection uses existing Composition authority;
- Review shows selected Public d20;
- no-Expansion Game retains G4-07 path;
- Public d20 Player action routes through accepted L3 adjudication Host;
- UI supplies stable action identity and retries unresolved actions;
- durable check truth is read-only in UI;
- Continue / Load can reconstruct/remove accepted mechanic records;
- real Han DeepSeek Public d20 evidence exists;
- protected backend paths are unchanged.

G4-08B is nevertheless **not PASS** because GPT Independent Review found player-facing projection/routing defects.

---

## 4. Current correction｜G4-08BC01

### A. Mechanic-card lifecycle/order

Current live path appends the transient card before the Player/GM turn exists and does not replace/reposition it after acceptance. Current redraw path appends the historical card after GM.

Required stable history is:

```text
Player action
→ mechanic card
→ GM narrative
```

This same association must hold live, on retry, reopen retry, Continue and Load/Restore.

Each durable `check_id` may have at most one visible mechanic projection at a time. Existing-check retry must not duplicate cards through both stage signal and synchronous streaming return.

### B. Unsupported action-resolution capability

A Game-local Expansion with:

```text
capability_slot = action_resolution
capability_id != action_check.public_d20.v1
```

must fail visibly and gate Player input. It must not silently fall back to the legacy no-Expansion Narrative path.

### C. Evidence correction

The dedicated explicit-none Wizard/Review test is currently empty despite the evidence document claiming coverage. Add direct proof:

```text
explicit none
→ Review shows 拓展 / 无
→ frozen Expansion selection is empty with explicit-none semantics
```

Remove production debug probe output from the UI code.

---

## 5. Protected boundaries

G4-08BC01 must not redesign or modify semantic ownership under:

- `src/source/**`
- `src/最终建局/**`
- `src/persistence/**`
- `src/行动判定/L0_公理层/**`
- `src/行动判定/L1_器件层/**`
- `src/行动判定/L2_流程层/**`
- Provider protocol

No click-to-roll, dice animation, attributes, combat, or new Expansion semantics.

---

## 6. Next progression

```text
G4-08BC01 — Kimi focused correction
→ GPT Independent Review
→ if PASS: G4-08B PASS / CLOSED
→ G4-09 First Playable B
→ prepare Owner production Source Library with Public d20 exact package
→ Owner UAT B
→ remaining G4 gate work
```

G4-08 is not yet Product PASS. Do not start G5 before remaining G4 route / G4-GATE are complete.
