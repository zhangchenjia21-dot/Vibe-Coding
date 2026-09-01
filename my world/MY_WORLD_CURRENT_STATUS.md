---
title: my world｜当前状态
status: current-project-status
version: 8.7
created: 2026-08-26
updated: 2026-09-01
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-09R1B1 Model Settings UI / Interaction
current_owner: Kimi
parent_task: G4-09R1 Runtime Model Settings v0.1
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
G4-08B Public d20 UI Integration      PASS / CLOSED
G4-08BC01 UI Projection / Fail-Loud   PASS / CLOSED
G4-09 First Playable B                ACTIVE
G4-09P1 Owner UAT B Production Prep   PASS / CLOSED
G4-09UATB Owner Product UAT           HOLD
G4-09R1 Runtime Model Settings v0.1   ACTIVE
G4-09R1S0 Semantic Freeze             PASS / CLOSED — GPT
G4-09R1M1 Backend Mechanism           PASS / CLOSED
G4-09R1M1C01 Projection/Kimi Proof    PASS / CLOSED
G4-09R1B1 Settings UI                 ACTIVE — KIMI
G4-GATE                               NOT YET
```

Owner-requested model settings remain the prerequisite before UAT B resumes.

Current formal packet:

`my-world/docs/tasks/G4-09R1B1_MODEL_SETTINGS_UI_TASK.md`

Canonical decision:

`my world/architecture/foundation/G4_RUNTIME_MODEL_SETTINGS_V0_1_DECISION.md`

Accepted backend review:

`my-world/docs/g4_09r1/G4-09R1M1C01_INDEPENDENT_REVIEW.md`

Current owner: **Kimi**. Reviewer/semantic owner: **GPT**.

---

## 2. Backend result｜PASS / CLOSED

Reviewed backend/evidence HEAD:

`6ea825ba0ea0d5a57728c55789f437ff9626b6cb`

G4-09R1M1 and correction-01 are closed.

Accepted backend truth:

- app-local durable settings/defaults and safe persistence;
- closed DeepSeek/Kimi four-profile catalog;
- exact model/context compatibility;
- DeepSeek/K3 Medium → effective High;
- K2.7 fixed Thinking ON and 256K-only compatibility;
- selected-provider-only credential routing and no fallback;
- shared runtime Provider seam across Opening/Narrative/Public d20 phases;
- UI-safe, non-mutating `inspect_candidate(settings)` projection;
- no secret or transport internals in the UI projection;
- real DeepSeek V4 Pro/Flash calls completed;
- real Kimi `k3-256k`, `k3`, and `kimi-for-coding` calls completed;
- Windows export freshness/regressions recorded green;
- Source/Final Create/Public d20/SQLite v4 boundaries remain unchanged.

---

## 3. Current task｜G4-09R1B1

Kimi owns only the Main Menu settings surface and interaction.

Required visible controls:

```text
模型
上下文上限
思考强度
DeepSeek / Kimi credential status
实际配置摘要
Save / Cancel
```

Exact display model list:

```text
DeepSeek V4 Pro
DeepSeek V4 Flash
Kimi K3
Kimi K2.7
```

UI must consume backend `inspect_candidate()` for compatibility/effective preview. It must not construct provider model ids/endpoints/request-body reasoning or duplicate compatibility rules.

Required product behavior:

- K2.7 → 1M unavailable, graded effort disabled, fixed-thinking explanation;
- Medium on DeepSeek/K3 → visible actual High disclosure;
- corrupt/invalid persisted setting → recoverable visible state, no silent provider substitution;
- credential status is boolean only, never key value;
- Save/Cancel/reopen/restart persistence;
- no Game/Source mutation;
- usable at 1280×720, 960×540 and maximized desktop;
- Continue/New Game/Public d20 remain green.

Real UI integration must demonstrate at least one DeepSeek and one Kimi selection through real generation.

---

## 4. G4-09UATB hold

Do not run the prior Owner UAT yet.

Resume condition:

```text
G4-09R1B1 Kimi
→ GPT Independent Review
→ final real DeepSeek + Kimi integration / Windows freshness
→ refresh Owner UAT instructions
→ G4-09UATB ACTIVE — OWNER
```

G4-09 and G4-08 remain open. G4-GATE is NOT YET. Do not start G4-10 or G5.