---
title: my world｜当前状态
status: current-project-status
version: 9.0
created: 2026-08-26
updated: 2026-09-02
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-09R1P1 Runtime Model Settings Final Integration / Owner UAT Readiness
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
G4-08 Expansion Pack v0.1             ACTIVE
G4-08M1 Public d20 Mechanism          PASS / CLOSED
G4-08B Public d20 UI Integration      PASS / CLOSED
G4-09 First Playable B                ACTIVE
G4-09P1 Owner UAT B Production Prep   PASS / CLOSED
G4-09UATB Owner Product UAT           HOLD
G4-09R1 Runtime Model Settings v0.1   ACTIVE
G4-09R1S0 Semantic Freeze             PASS / CLOSED
G4-09R1M1 Backend Mechanism           PASS / CLOSED
G4-09R1B1 Settings UI                 PASS / CLOSED AFTER CORRECTION-01
G4-09R1B1C01A L3 UI Support           PASS / CLOSED
G4-09R1B1C01B UI State Consistency    PASS / CLOSED
G4-09R1P1 Final Integration/Freshness ACTIVE — CODEX
G4-GATE                               NOT YET
```

Owner UAT remains HOLD until the final integration/freshness gate passes Independent Review.

Current packet:

`my-world/docs/tasks/G4-09R1P1_FINAL_INTEGRATION_FRESHNESS_TASK.md`

Current owner: **Codex**. Reviewer/semantic owner: **GPT**.

## 2. G4-09R1B1 final result｜PASS / CLOSED

Final reviewed UI evidence HEAD:

`b6bd6bc8e077bbeccbb8639f6bc0670795e3e36c`

Accepted B1 vertical:

- Main Menu `模型设置` entry;
- exact display models DeepSeek V4 Pro / V4 Flash / Kimi K3 / Kimi K2.7;
- backend-owned context/reasoning/fixed-thinking/effective projection;
- Medium → actual High disclosure;
- K2.7 256K-only and fixed Thinking ON, including invalid 1M intermediate state;
- non-secret credential status;
- Save/Cancel/Escape/reopen/restart persistence;
- invalid persisted recovery through Runtime Settings L3 only;
- no Game/Source/SQLite mutation;
- required desktop layouts and regressions green;
- real DeepSeek and Kimi UI-selected generation evidence accepted.

Formal reviews:

- `my-world/docs/g4_09r1/G4-09R1B1_INDEPENDENT_REVIEW.md`
- `my-world/docs/g4_09r1/G4-09R1B1C01A_INDEPENDENT_REVIEW.md`
- `my-world/docs/g4_09r1/G4-09R1B1C01B_INDEPENDENT_REVIEW.md`

No correction-02 is required.

## 3. Current task｜G4-09R1P1

This is the final reality/freshness gate before Owner UAT B resumes. It must prove on current `main`:

```text
actual Main Menu settings -> DeepSeek V4 Pro -> real accepted Opening
actual Main Menu settings -> Kimi K3 -> real accepted Opening
canonical run-game Windows export freshness
production World/Character/Public d20 UAT prerequisites intact
Owner Games unchanged
Owner UAT instructions refreshed to start with Model Settings
```

Tests must use task-owned Game/Source/settings roots; do not overwrite Owner production model preference merely to validate. No feature redesign is allowed.

## 4. Resume condition for Owner UAT

```text
G4-09R1P1 Codex
→ GPT Independent Review
→ if PASS: G4-09R1 PASS / CLOSED
→ G4-09UATB ACTIVE — OWNER
```

Owner UAT will still decide G4-09/G4-08 product PASS. G4-GATE remains NOT YET; do not start G4-10 or G5.