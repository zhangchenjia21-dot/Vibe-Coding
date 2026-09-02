---
title: my world｜当前状态
status: current-project-status
version: 8.8
created: 2026-08-26
updated: 2026-09-02
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-09R1B1C01A Runtime Settings L3 UI Support
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
G4-09R1M1 Backend Mechanism           PASS / CLOSED
G4-09R1M1C01 Projection/Kimi Proof    PASS / CLOSED
G4-09R1B1 Settings UI                 CORRECTION REQUIRED
G4-09R1B1C01A L3 UI Support           ACTIVE — CODEX
G4-09R1B1C01B UI State Consistency    HOLD — KIMI
G4-GATE                               NOT YET
```

Owner UAT remains HOLD. The B1 implementation has real DeepSeek/Kimi UI generation evidence, but GPT Independent Review found two state/ownership blockers plus the missing Escape cancel path.

Implementation review:

`my-world/docs/g4_09r1/G4-09R1B1_INDEPENDENT_REVIEW.md`

Current packet:

`my-world/docs/tasks/G4-09R1B1C01A_RUNTIME_SETTINGS_L3_UI_SUPPORT_TASK.md`

Current owner: **Codex**. Reviewer/semantic owner: **GPT**.

---

## 2. Accepted B1 implementation

Reviewed B1/evidence HEAD:

`fcdcec66edad41afbb93f4a5e9cc70174402be5c`

Accepted and not to be reopened absent regression:

- Main Menu `模型设置` entry and Main-Menu-only overlay;
- exact four display models;
- valid unsaved candidate preview through backend `inspect_candidate()`;
- Medium → actual High disclosure;
- valid K2.7 / 256K fixed-thinking presentation;
- non-secret DeepSeek/Kimi credential status;
- Save/Cancel/reopen/restart persistence;
- settings stay outside Game/Source/SQLite;
- Continue/New Game/Public d20 regressions recorded green;
- required desktop layout evidence recorded green;
- real DeepSeek V4 Pro UI selection → persisted setting → real Opening completed;
- real Kimi K3 UI selection → persisted setting → real Opening completed.

---

## 3. Why B1 is not PASS

### A. Invalid K2.7 intermediate state

Sequence:

```text
Kimi K3 / 1M
→ switch model to Kimi K2.7
→ backend correctly rejects K2.7 + 1M
```

Current UI disables Save and 1M, but returns before applying fixed-thinking presentation. Reasoning can remain enabled and the fixed Thinking explanation disappears. K2.7 capability truth must remain consistent even while the context choice is invalid.

### B. UI crosses Runtime Settings L0

Application Shell directly imports the Runtime Settings L0 rules only to obtain the validated default for corrupt persisted settings. UI must depend on L3 only. A small L3 default-settings seam is required.

### C. Escape cancel path

The custom settings overlay has a visible Cancel button but no explicit `ui_cancel` / Escape behavior, despite the B1 interaction requirement.

---

## 4. Current correction routing

First:

```text
G4-09R1B1C01A — Codex
```

Backend-only result:

- expose exact validated default through Runtime Settings L3;
- for known incompatible candidates, return safe partial capability truth through L3 while preserving failure status;
- no UI, Provider, Source, persistence-schema or d20 changes.

Then, only after GPT Independent Review PASS:

```text
G4-09R1B1C01B — Kimi
```

UI result:

- remove UI→L0 dependency;
- preserve K2.7 fixed-thinking presentation during invalid 1M state;
- valid 256K recovery and graded-model switching remain correct;
- Escape/ui_cancel behaves like Cancel without Save.

---

## 5. UAT hold

Resume condition:

```text
C01A Codex
→ GPT Independent Review
→ C01B Kimi
→ GPT Independent Review
→ G4-09R1B1 PASS / CLOSED
→ final Provider + Windows freshness integration
→ refresh Owner UAT instructions
→ G4-09UATB ACTIVE — OWNER
```

G4-09 and G4-08 remain open. G4-GATE is NOT YET. Do not start G4-10 or G5.