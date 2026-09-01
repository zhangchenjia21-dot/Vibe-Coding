---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-01
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-09R1 Runtime Model Settings v0.1
current_execution_task: G4-09R1M1 Runtime Model Settings / Multi-Provider Mechanism
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
G4-09R1M1 Backend Mechanism           ACTIVE — CODEX
G4-09R1B1 Settings UI                 HOLD — KIMI
G4-GATE                               NOT YET
```

Owner explicitly requested model/runtime settings before UAT B. Do not ask Owner to run the stale DeepSeek-only UAT yet.

---

## 2. Read first

Governance:

1. `my world/MY_WORLD_CURRENT_STATUS.md`
2. `my world/architecture/foundation/G4_RUNTIME_MODEL_SETTINGS_V0_1_DECISION.md`
3. `my world/architecture/source/G4_EXPANSION_V0_1_PUBLIC_D20_DECISION.md`

Implementation:

4. `AGENTS.md`
5. `docs/tasks/G4-09R1M1_RUNTIME_MODEL_SETTINGS_MECHANISM_TASK.md`
6. `docs/tasks/G4-09R1B1_MODEL_SETTINGS_UI_TASK.md` — HOLD
7. `src/provider/deepseek流式适配器.gd`
8. `src/context/上下文组装器.gd`
9. Opening / Narrative / ActionAdjudication Provider construction seams
10. `.env.example` / `run-game.ps1`

---

## 3. Frozen model catalog

```text
deepseek_v4_pro   → DeepSeek V4 Pro   → deepseek-v4-pro
deepseek_v4_flash → DeepSeek V4 Flash → deepseek-v4-flash
kimi_k3 / 256K    → Kimi K3            → k3-256k
kimi_k3 / 1M      → Kimi K3            → k3
kimi_k27 / 256K   → Kimi K2.7          → kimi-for-coding
```

Kimi K2.7 + 1M is invalid.

Requested reasoning:

```text
DeepSeek/K3:
Low    → low
Medium → high
High   → high
Max    → max

Kimi K2.7:
Thinking ON fixed; no graded effort control
```

Context setting means application runtime context ceiling/capability selection. Do not claim every Provider accepts a request-body context-window field. Do not pull G7 forward.

---

## 4. Credentials / scope

Secrets:

```text
DEEPSEEK_API_KEY
KIMI_API_KEY
```

Settings persist outside Games under an app-local settings seam. Default:

```text
DeepSeek V4 Pro / 256K / High
```

Settings are not Source, Composition, Game canonical reality or SQLite schema. Existing Games must not be rewritten.

No automatic cross-provider fallback.

---

## 5. Current Codex task

Packet:

`my-world/docs/tasks/G4-09R1M1_RUNTIME_MODEL_SETTINGS_MECHANISM_TASK.md`

Codex owns:

- durable application settings/defaults/validation;
- exact closed profile catalog;
- DeepSeek + Kimi fixed OpenAI-compatible endpoints;
- provider model-id derivation and reasoning mapping;
- selected-provider credential fail-loud;
- one runtime profile seam consumed by Opening, Narrative and both Public d20 Provider phases;
- `.env.example` / canonical launcher credential allowlist as required;
- deterministic regression evidence;
- small real DeepSeek and Kimi calls when local credentials/entitlement permit.

Real Kimi support cannot be declared from stubs alone. If Kimi credential or entitlement is unavailable, return that explicit blocker.

Protected: Source/Final Create/Game persistence/Public d20 semantics/SQLite v4/G7 architecture.

Return ceiling: **READY FOR INDEPENDENT REVIEW**. Codex must not start the UI task.

---

## 6. After Codex returns

GPT refreshes both main heads and reviews actual code/evidence.

If M1 PASS:

```text
G4-09R1M1 PASS / CLOSED
→ activate G4-09R1B1 — KIMI
```

Kimi then implements Main Menu `模型设置`, compatibility states, effective reasoning disclosure, credential status, save/cancel/reopen and responsive layout. UI never owns model ids/endpoints/secrets.

After Kimi IR PASS:

```text
real DeepSeek + Kimi integration
→ Windows export freshness
→ refreshed UAT instructions
→ G4-09UATB ACTIVE — OWNER
```

G4-09 and G4-08 remain open; do not start G4-10/G5.