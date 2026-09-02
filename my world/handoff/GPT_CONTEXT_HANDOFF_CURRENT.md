---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-02
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-09R1 Runtime Model Settings v0.1
current_execution_task: G4-09R1P1 Runtime Model Settings Final Integration / Owner UAT Readiness
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
G4-09R1M1 Backend Mechanism           PASS / CLOSED
G4-09R1B1 Settings UI                 PASS / CLOSED AFTER CORRECTION-01
G4-09R1B1C01A L3 UI Support           PASS / CLOSED
G4-09R1B1C01B UI State Consistency    PASS / CLOSED
G4-09R1P1 Final Integration/Freshness ACTIVE — CODEX
G4-GATE                               NOT YET
```

Owner UAT remains paused until G4-09R1P1 Independent Review passes.

## 2. Read first

Governance:

1. `my world/MY_WORLD_CURRENT_STATUS.md`
2. `my world/architecture/foundation/G4_RUNTIME_MODEL_SETTINGS_V0_1_DECISION.md`

Implementation:

3. `AGENTS.md`
4. `docs/g4_09r1/G4-09R1B1_INDEPENDENT_REVIEW.md`
5. `docs/g4_09r1/G4-09R1B1C01B_INDEPENDENT_REVIEW.md`
6. `docs/tasks/G4-09R1P1_FINAL_INTEGRATION_FRESHNESS_TASK.md`
7. current real settings UI runner / canonical `run-game.ps1`

## 3. Accepted Model Settings implementation

Final reviewed UI evidence HEAD:

`b6bd6bc8e077bbeccbb8639f6bc0670795e3e36c`

Accepted:

- app-local runtime model settings;
- exact four-profile catalog and compatibility semantics;
- DeepSeek/K3 Medium → effective High;
- K2.7 256K-only / fixed Thinking ON;
- selected-provider credentials only and no fallback;
- shared Provider route across Opening/Narrative/Public d20;
- Main Menu Model Settings UI using Runtime Settings L3 projection;
- no UI→Runtime Settings L0 dependency;
- K2.7 invalid 1M intermediate state preserves both failure and fixed-thinking truth;
- Save/Cancel/Escape/restart persistence;
- real DeepSeek and Kimi UI-selected generation evidence;
- SQLite v4 / Source / Final Create / Public d20 semantics unchanged.

## 4. Current Codex task

Packet:

`my-world/docs/tasks/G4-09R1P1_FINAL_INTEGRATION_FRESHNESS_TASK.md`

Codex owns final validation/UAT readiness only. It must:

1. rerun actual Main Menu selection + Save → real Opening for DeepSeek V4 Pro and Kimi K3 on current `main` using task-owned roots;
2. run `.\run-game.ps1 -ValidateExportOnly` and record whether current or rebuilt;
3. verify production World/Character/Public d20 UAT prerequisites are intact without modifying Owner Games or manually copying managed Source files;
4. rerun focused Settings UI / Runtime Settings / Public d20 regression floor;
5. update `docs/g4_09/` Owner UAT B instructions so the route begins with Main Menu Model Settings, Save, and reopen-confirmed effective summary before New Game.

Do not overwrite the Owner's production model preference merely for testing. Do not redesign model/provider/UI/d20/persistence semantics.

Return ceiling: READY FOR INDEPENDENT REVIEW.

## 5. After Codex returns

GPT refreshes both mains and reviews actual evidence. Verify:

- both real selected-provider UI verticals completed on final code line;
- no fallback or secret leak;
- canonical Windows export is fresh;
- production Public d20 and usable Source prerequisites remain intact;
- Owner Games modified = no;
- refreshed UAT instructions are product-only and include Model Settings first;
- regression floor / SQLite v4 remain green.

If PASS:

```text
G4-09R1P1 PASS / CLOSED
G4-09R1 Runtime Model Settings v0.1 PASS / CLOSED (engineering/product-entry prerequisite)
G4-09UATB ACTIVE — OWNER
owner_uat_required: true
```

Then tell Owner the exact launch/UAT route. Do not close G4-09/G4-08 until Owner Product UAT verdict.