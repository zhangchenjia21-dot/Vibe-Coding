---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-02
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-09R1 Runtime Model Settings v0.1
current_execution_task: G4-09R1B1C01B Settings UI State Consistency
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
G4-09R1M1 Backend Mechanism           PASS / CLOSED
G4-09R1B1 Settings UI                 CORRECTION REQUIRED
G4-09R1B1C01A L3 UI Support           PASS / CLOSED
G4-09R1B1C01B UI State Consistency    ACTIVE — KIMI
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
5. `docs/g4_09r1/G4-09R1B1C01A_INDEPENDENT_REVIEW.md`
6. `docs/tasks/G4-09R1B1C01B_UI_STATE_CONSISTENCY_CORRECTION_TASK.md`
7. current Application Shell settings UI + B1 focused tests

---

## 3. Accepted backend and B1 reality

Backend/provider model semantics remain PASS / CLOSED:

- exact four-profile catalog;
- DeepSeek/K3 reasoning mapping;
- K2.7 fixed Thinking ON / 256K-only;
- selected-provider credentials, no fallback;
- shared Provider routing through Opening/Narrative/Public d20;
- real DeepSeek and real Kimi model calls completed;
- SQLite v4 / Source / Final Create / d20 semantics unchanged.

Original B1 reviewed HEAD:

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
- real DeepSeek V4 Pro and Kimi K3 selected from actual UI each reached real Opening generation.

---

## 4. C01A accepted support

Reviewed implementation/evidence HEAD:

`bb3c16b392887a4649f32e23348067c70a3e7a1c`

Formal review:

`my-world/docs/g4_09r1/G4-09R1B1C01A_INDEPENDENT_REVIEW.md`

Accepted L3 seams:

```text
validated_default_settings()
→ defensive exact deepseek_v4_pro / 256k / high

inspect_candidate(kimi_k27 / 1m / high)
→ success=false / incompatible_context_limit
→ candidate.allowed_context_limits=[256k]
→ candidate.fixed_thinking=true
→ candidate.graded_reasoning=false
→ candidate.reasoning_effective=null
→ selected-provider credential bool only
```

No transport/secret fields; unknown/malformed candidates expose no partial identity; no settings/Game/Source/SQLite write side effect.

---

## 5. Current Kimi task

Packet:

`my-world/docs/tasks/G4-09R1B1C01B_UI_STATE_CONSISTENCY_CORRECTION_TASK.md`

Kimi must:

1. remove `src/应用壳.gd` direct Runtime Settings L0 dependency and use L3 `validated_default_settings()` for corrupt persisted recovery;
2. consume partial L3 candidate truth even on `incompatible_context_limit` so `K3 / 1M → K2.7` shows Save disabled + 1M disabled + reasoning disabled + fixed Thinking ON explanation simultaneously;
3. preserve valid 256K recovery and re-enable reasoning when switching to a graded model according to L3 projection;
4. implement `ui_cancel` / Escape exactly as Cancel without Save/mutation;
5. keep accepted B1 layout, persistence, Continue/New Game/Public d20 behavior green.

Do not modify Runtime Settings/backend/Provider/Source/Final Create/Persistence/Public d20.

Existing real DeepSeek/Kimi UI generation evidence remains applicable if normal Save/provider routing is unchanged.

Return ceiling: READY FOR INDEPENDENT REVIEW.

---

## 6. After Kimi returns

GPT refreshes both mains and verifies actual code/evidence, especially:

- no Application Shell L0 import;
- invalid persisted recovery through L3 only and no silent save;
- exact K3/1M→K2.7 intermediate state consistency;
- 256K recovery and graded-model switch;
- Escape/ui_cancel no-save behavior;
- no backend/protected-path changes;
- focused B1, G4-07B, G4-08B and layouts green.

If PASS:

```text
G4-09R1B1 PASS / CLOSED
→ final real Provider / Windows freshness integration
→ refresh Owner UAT instructions
→ G4-09UATB ACTIVE — OWNER
```

Do not close G4-09/G4-08 before Owner Product UAT verdict.
