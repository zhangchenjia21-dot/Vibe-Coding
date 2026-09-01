---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-01
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-09R1 Runtime Model Settings v0.1
current_execution_task: G4-09R1M1C01 Settings Projection + Kimi Proof
semantic_owner: GPT
current_execution_owner: Codex
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Current state

```text
G4-07 First Playable A                PASS / CLOSED
G4-08 Expansion Pack v0.1             ACTIVE
G4-08M1 Public d20 Mechanism          PASS / CLOSED
G4-08B Public d20 UI Integration      PASS / CLOSED
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

Owner UAT remains paused. Kimi UI must not start until M1C01 passes GPT Independent Review.

---

## 2. Read first

Governance:

1. `my world/MY_WORLD_CURRENT_STATUS.md`
2. `my world/architecture/foundation/G4_RUNTIME_MODEL_SETTINGS_V0_1_DECISION.md`

Implementation:

3. `AGENTS.md`
4. `docs/g4_09r1/G4-09R1M1_INDEPENDENT_REVIEW.md`
5. `docs/tasks/G4-09R1M1C01_SETTINGS_PROJECTION_KIMI_PROOF_CORRECTION_TASK.md`
6. `docs/g4_09r1/G4-09R1M1_机制实现证据.md`
7. Runtime Settings L0-L3
8. Provider L1-L3
9. `tests/g4_09r1/运行时模型设置机制测试.gd`
10. real multi-provider runner

---

## 3. M1 accepted implementation

Reviewed HEAD:

`7b183d4b5aecd1b4d0e0f80fbfe235ded4c67344`

Accepted and protected absent concrete regression:

- `user://my-world/settings/provider-runtime.json` with schema `my-world.provider-runtime.v1`;
- default `deepseek_v4_pro / 256k / high`;
- exact closed catalog:
  - DeepSeek V4 Pro → `deepseek-v4-pro`;
  - DeepSeek V4 Flash → `deepseek-v4-flash`;
  - Kimi K3 256K → `k3-256k`;
  - Kimi K3 1M → `k3`;
  - Kimi K2.7 256K → `kimi-for-coding`;
- K2.7 + 1M fail-closed;
- DeepSeek/K3 `low→low`, `medium→high`, `high→high`, `max→max`;
- K2.7 fixed-thinking capability metadata / no graded effective value;
- atomic/fail-safe settings persistence;
- selected Provider credential only: `DEEPSEEK_API_KEY` or `KIMI_API_KEY`;
- missing selected key → zero network; no cross-provider fallback;
- shared runtime Provider seam used by First Opening, ordinary Narrative, d20 adjudication and d20 resolution narrative;
- active request freezes runtime profile;
- reasoning-only stream material is not emitted as player narrative;
- canonical launcher allows either/both/no key at Main Menu startup;
- real DeepSeek V4 Pro/Flash evidence completed;
- Source/Final Create/Public d20/SQLite v4 unchanged.

---

## 4. Why M1 is not PASS

### A. Missing UI-safe unsaved candidate projection

Current Settings L3 only exposes current persisted `request_snapshot()` plus `catalog()/validate()`. G4-09R1B1 needs to preview an unsaved choice while backend retains ownership of compatibility/effective policy.

Correction must add a non-mutating projection sufficient for:

```text
K2.7 + 1M invalid
Medium → actual High
K2.7 fixed thinking
allowed context choices
selected-provider configured bool
```

No secret values, no save side effect, and no requirement for UI to derive endpoint/model/request fields.

### B. Kimi wire / real-provider proof incomplete

Current evidence has:

```text
DeepSeek V4 Pro    completed
DeepSeek V4 Flash  completed
Kimi K3            credential_unavailable
Kimi K2.7          credential_unavailable
```

Original M1 task explicitly says stubs are insufficient for final Kimi PASS.

Current official Kimi behavior must be used to verify/correct actual Thinking ON wire semantics for K3 and K2.7, then small real Kimi calls must run when local `KIMI_API_KEY` and entitlement are available.

Never substitute another model or expose the key.

---

## 5. Current correction

Packet:

`my-world/docs/tasks/G4-09R1M1C01_SETTINGS_PROJECTION_KIMI_PROOF_CORRECTION_TASK.md`

Correction budget: **correction-01**.

Codex should only touch the settings/provider contract/tests/evidence required for the two focused gaps. Do not implement final Settings UI and do not redesign Source/Game/Public d20/G7/G8.

Return ceiling: **READY FOR INDEPENDENT REVIEW**.

---

## 6. After Codex returns

GPT must refresh both `main` branches and inspect actual correction diff/evidence.

Verify:

1. candidate projection is backend-owned and non-mutating;
2. projection directly proves K2.7 context incompatibility, Medium→High, fixed-thinking and non-secret credential status;
3. Kimi K3/K2.7 wire keeps Thinking ON according to current official API behavior;
4. K2.7 sends no fake graded effort;
5. real Kimi K3 and K2.7 calls succeeded, or a precise current credential/entitlement blocker remains;
6. regressions stay green and SQLite remains v4.

If all required reality evidence is present and PASS:

```text
G4-09R1M1C01 PASS / CLOSED
G4-09R1M1 PASS / CLOSED
→ activate G4-09R1B1 — KIMI
```

If `KIMI_API_KEY` is still absent, M1 remains open and UI remains HOLD; do not call stub evidence a real Kimi PASS.

G4-GATE remains NOT YET; do not start G4-10/G5.