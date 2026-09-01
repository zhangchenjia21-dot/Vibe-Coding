---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-01
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-09R1 Runtime Model Settings v0.1
current_execution_task: G4-09R1B1 Model Settings UI / Interaction
semantic_owner: GPT
current_execution_owner: Kimi
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
G4-09R1M1 Backend Mechanism           PASS / CLOSED
G4-09R1M1C01 Projection/Kimi Proof    PASS / CLOSED
G4-09R1B1 Settings UI                 ACTIVE — KIMI
G4-GATE                               NOT YET
```

Owner UAT remains paused until Settings UI and final integration/freshness pass.

---

## 2. Read first

Governance:

1. `my world/MY_WORLD_CURRENT_STATUS.md`
2. `my world/architecture/foundation/G4_RUNTIME_MODEL_SETTINGS_V0_1_DECISION.md`

Implementation:

3. `AGENTS.md`
4. `docs/g4_09r1/G4-09R1M1C01_INDEPENDENT_REVIEW.md`
5. `docs/tasks/G4-09R1B1_MODEL_SETTINGS_UI_TASK.md`
6. Runtime Settings L3 public interface, especially `inspect_candidate(settings)`
7. current Application Shell / Main Menu scene and script

---

## 3. Accepted backend

Reviewed backend/evidence HEAD:

`6ea825ba0ea0d5a57728c55789f437ff9626b6cb`

Backend is **PASS / CLOSED**.

Accepted:

- app-local settings `user://my-world/settings/provider-runtime.json`;
- schema `my-world.provider-runtime.v1`;
- default `deepseek_v4_pro / 256k / high`;
- exact closed catalog:
  - DeepSeek V4 Pro → `deepseek-v4-pro`;
  - DeepSeek V4 Flash → `deepseek-v4-flash`;
  - Kimi K3 256K → `k3-256k`;
  - Kimi K3 1M → `k3`;
  - Kimi K2.7 256K → `kimi-for-coding`;
- K2.7 + 1M fail-closed;
- DeepSeek/K3 `low→low`, `medium→high`, `high→high`, `max→max`;
- K2.7 fixed Thinking ON / no graded effective effort;
- selected-provider credential only: `DEEPSEEK_API_KEY` or `KIMI_API_KEY`;
- missing selected key → zero network, no cross-provider fallback;
- shared runtime Provider seam across Opening/Narrative/Public d20 phases;
- active request profile snapshot immutable;
- reasoning-only stream material hidden from player narrative;
- real DeepSeek V4 Pro/Flash calls completed;
- real Kimi `k3-256k`, `k3`, `kimi-for-coding` calls completed;
- SQLite remains v4; Source/Final Create/Public d20 semantics unchanged.

## 4. Mandatory UI projection seam

Backend now exposes:

```text
ModelRuntimeSettingsPublicInterface.inspect_candidate(settings)
```

It is non-mutating and UI-safe. It returns backend-owned truth for:

```text
profile display/provider
selected + allowed context
requested/effective reasoning
graded/fixed thinking
selected-provider credential configured bool
validation error
```

It does not return endpoint/model id/request path/payload fields/key values and does not save the candidate.

Kimi UI must use this seam rather than duplicating provider rules.

---

## 5. Current Kimi task

Packet:

`my-world/docs/tasks/G4-09R1B1_MODEL_SETTINGS_UI_TASK.md`

Kimi owns:

- Main Menu `模型设置` entry;
- model/context/reasoning controls;
- backend-derived compatibility/disabled states;
- K2.7 256K-only + fixed-thinking explanation;
- Medium → actual High disclosure;
- non-secret DeepSeek/Kimi credential status;
- Save/Cancel/reopen/restart UX;
- responsive desktop layout.

Kimi must not own model ids, endpoints, request payload fields, credential values or provider fallback.

Required real UI integration before return:

```text
one DeepSeek selection → real generation
one Kimi selection     → real generation
```

Layouts: 1280×720, 960×540, maximized desktop. Continue/New Game/Public d20 must remain green.

Return ceiling: **READY FOR INDEPENDENT REVIEW**.

---

## 6. After Kimi returns

GPT refreshes both main branches and reviews actual UI code/evidence.

Verify:

1. UI consumes `inspect_candidate()` rather than duplicating compatibility/effective rules;
2. K2.7 disables 1M and graded reasoning visibly;
3. Medium visibly discloses actual High for DeepSeek/K3;
4. credential display is bool-only with no secret exposure;
5. Save/Cancel/reopen/restart behavior is correct;
6. invalid persisted state is visibly recoverable;
7. layout is usable at all required desktop sizes;
8. real DeepSeek and Kimi selected via UI each reach real generation;
9. Continue/New Game/Public d20 regressions remain green;
10. no Source/Game/SQLite mutation from settings.

If PASS:

```text
G4-09R1B1 PASS / CLOSED
→ final integration/freshness check
→ refresh Owner UAT instructions
→ G4-09UATB ACTIVE — OWNER
```

Do not close G4-09/G4-08 before Owner Product UAT verdict. G4-GATE remains NOT YET.