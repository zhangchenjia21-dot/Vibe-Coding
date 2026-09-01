---
title: my world｜当前状态
status: current-project-status
version: 8.6
created: 2026-08-26
updated: 2026-09-01
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-09R1M1C01 Settings Projection + Kimi Proof
current_owner: Codex
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
G4-09R1M1 Backend Mechanism           CORRECTION REQUIRED
G4-09R1M1C01 Projection/Kimi Proof    ACTIVE — CODEX
G4-09R1B1 Settings UI                 HOLD — KIMI
G4-GATE                               NOT YET
```

Owner-requested model settings remain a prerequisite before UAT B resumes.

Canonical decision:

`my world/architecture/foundation/G4_RUNTIME_MODEL_SETTINGS_V0_1_DECISION.md`

Independent Review:

`my-world/docs/g4_09r1/G4-09R1M1_INDEPENDENT_REVIEW.md`

Current correction packet:

`my-world/docs/tasks/G4-09R1M1C01_SETTINGS_PROJECTION_KIMI_PROOF_CORRECTION_TASK.md`

Current owner: **Codex**. Reviewer/semantic owner: **GPT**.

---

## 2. M1 review result

Reviewed implementation/evidence HEAD:

`7b183d4b5aecd1b4d0e0f80fbfe235ded4c67344`

M1 established the intended runtime settings/multi-provider architecture and most of it is accepted:

- app-local durable settings/defaults;
- closed DeepSeek/Kimi profile catalog;
- exact endpoint/model derivation;
- DeepSeek/K3 effort mapping and K2.7 fixed-thinking capability metadata;
- selected-provider-only credentials and no fallback;
- one runtime Provider seam for Opening/Narrative/Public d20 phases;
- immutable per-request profile snapshot;
- reasoning-only stream chunks hidden from narrative;
- launcher no longer requires DeepSeek key merely to reach Main Menu;
- real DeepSeek V4 Pro and V4 Flash calls completed;
- protected Game/Source/Public d20/SQLite v4 boundaries remain intact.

M1 is not closed because two acceptance seams remain.

---

## 3. Correction A｜UI-safe effective projection

The future Main Menu UI must consume backend capability/effective truth without duplicating provider rules.

Current backend has `catalog()`, `validate()` and current persisted `request_snapshot()`, but no non-mutating projection for an unsaved candidate.

Correction adds a backend-owned candidate projection sufficient to show:

```text
model display/capability
selected + allowed context
requested/effective reasoning
K2.7 fixed thinking
selected-provider configured bool
validation errors
```

It must directly prove:

- K2.7 + 1M invalid;
- DeepSeek/K3 Medium → actual High;
- K2.7 has no fake effective graded effort;
- no API key value exposure;
- frontend does not derive endpoint/model id/request parameters.

---

## 4. Correction B｜Kimi Thinking wire + real proof

Frozen product truth remains:

```text
Kimi K3    → Thinking ON + low/high/max
Kimi K2.7  → Thinking ON fixed; no graded effort
```

Current M1 payload was only deterministically tested for Kimi because the Owner environment did not expose `KIMI_API_KEY`.

Current official Kimi behavior must be used to verify/correct the actual OpenAI-compatible wire so K3/K2.7 cannot accidentally run with Thinking off / older-model fallback.

Then run small real requests with local `KIMI_API_KEY` when available. If credential or model entitlement is unavailable, report that exact non-secret blocker; never substitute another model and never fake PASS from stubs.

---

## 5. G4-09UATB hold

Do not run the prior Owner UAT yet.

Resume condition:

```text
G4-09R1M1C01 Codex
→ GPT Independent Review
→ G4-09R1M1 PASS / CLOSED
→ G4-09R1B1 Kimi
→ GPT Independent Review
→ real Provider + Windows export freshness
→ refresh UAT instructions
→ G4-09UATB ACTIVE — OWNER
```

G4-09 and G4-08 remain open. G4-GATE is NOT YET. Do not start G4-10 or G5.