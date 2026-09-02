---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-02
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-09R1 Runtime Model Settings v0.1
current_execution_task: G4-09R1B1C01A Runtime Settings L3 UI Support
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
G4-09R1B1 Settings UI                 CORRECTION REQUIRED
G4-09R1B1C01A L3 UI Support           ACTIVE — CODEX
G4-09R1B1C01B UI State Consistency    HOLD — KIMI
G4-GATE                               NOT YET
```

Owner UAT remains paused.

---

## 2. Read first

Governance:

1. `my world/MY_WORLD_CURRENT_STATUS.md`
2. `my world/architecture/foundation/G4_RUNTIME_MODEL_SETTINGS_V0_1_DECISION.md`

Implementation:

3. `AGENTS.md`
4. `docs/g4_09r1/G4-09R1B1_INDEPENDENT_REVIEW.md`
5. `docs/tasks/G4-09R1B1C01A_RUNTIME_SETTINGS_L3_UI_SUPPORT_TASK.md`
6. `docs/tasks/G4-09R1B1C01B_UI_STATE_CONSISTENCY_CORRECTION_TASK.md` — HOLD
7. Runtime Settings L2/L3 current implementation and focused tests

---

## 3. Accepted backend and B1 reality

Backend remains PASS / CLOSED for provider/model semantics:

- exact four-profile catalog;
- DeepSeek/K3 reasoning mapping;
- K2.7 fixed Thinking ON / 256K-only;
- selected-provider credentials, no fallback;
- shared Provider routing through Opening/Narrative/Public d20;
- real DeepSeek and real Kimi model calls completed;
- SQLite v4 / Source / Final Create / d20 semantics unchanged.

B1 reviewed HEAD:

`fcdcec66edad41afbb93f4a5e9cc70174402be5c`

Accepted B1 work:

- Main Menu settings surface and exact display model list;
- valid `inspect_candidate()` preview;
- Medium→actual High disclosure;
- valid K2.7/256K fixed-thinking UX;
- non-secret credential display;
- Save/Cancel/reopen/restart behavior;
- no Game/Source mutation;
- layout/regression evidence;
- real DeepSeek V4 Pro and Kimi K3 selected from the actual UI each reached real Opening generation.

---

## 4. Why B1 needs correction

### Finding A

With current context 1M, switching model to Kimi K2.7 yields backend `incompatible_context_limit`. UI correctly disables Save, but its failure path returns before applying fixed-thinking presentation. Reasoning can remain enabled and the fixed-Thinking explanation is hidden.

### Finding B

Application Shell directly imports Runtime Settings L0 rules to get `validated_default()` for corrupt persisted settings. UI must depend on Runtime Settings L3 only.

### Finding C

The custom overlay has no explicit `ui_cancel` / Escape path; packet required sensible Escape/Cancel behavior.

---

## 5. Current Codex task

Packet:

`my-world/docs/tasks/G4-09R1B1C01A_RUNTIME_SETTINGS_L3_UI_SUPPORT_TASK.md`

Codex owns only:

1. expose exact validated default through Runtime Settings L3, non-mutating and defensive-copy;
2. make `inspect_candidate()` return safe partial capability truth for a known profile even when context is incompatible, while preserving `success=false / status=incompatible_context_limit`;
3. direct tests for no-write/no-secret behavior.

Do not change UI or Provider wire/model semantics. Real Provider rerun is unnecessary unless Provider code is touched.

Return ceiling: READY FOR INDEPENDENT REVIEW.

---

## 6. After C01A

If GPT passes C01A, activate:

`my-world/docs/tasks/G4-09R1B1C01B_UI_STATE_CONSISTENCY_CORRECTION_TASK.md`

Kimi then removes the L0 dependency, consumes new L3 support, fixes K2.7 invalid-context fixed-thinking presentation, and adds Escape/ui_cancel = Cancel.

After C01B GPT Independent Review PASS:

```text
G4-09R1B1 PASS / CLOSED
→ final real Provider / Windows freshness integration
→ refreshed Owner UAT instructions
→ G4-09UATB ACTIVE — OWNER
```

Do not close G4-09/G4-08 before Owner Product UAT verdict.