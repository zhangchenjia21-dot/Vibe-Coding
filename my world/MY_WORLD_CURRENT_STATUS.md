---
title: my world｜当前状态
status: current-project-status
version: 9.1
created: 2026-08-26
updated: 2026-09-02
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-09UATB Owner Product UAT
current_owner: OWNER
parent_task: G4-09 First Playable B
semantic_owner: GPT
owner_uat_required: true
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
G4-08 Expansion Pack v0.1             ACTIVE pending Owner verdict
G4-08M1 Public d20 Mechanism          PASS / CLOSED
G4-08B Public d20 UI Integration      PASS / CLOSED
G4-09 First Playable B                ACTIVE pending Owner verdict
G4-09P1 Owner UAT B Production Prep   PASS / CLOSED
G4-09R1 Runtime Model Settings v0.1   PASS / CLOSED
G4-09R1S0 Semantic Freeze             PASS / CLOSED
G4-09R1M1 Backend Mechanism           PASS / CLOSED
G4-09R1B1 Settings UI                 PASS / CLOSED AFTER CORRECTION-01
G4-09R1P1 Final Integration/Freshness PASS / CLOSED
G4-09UATB Owner Product UAT           ACTIVE — OWNER
G4-GATE                               NOT YET
```

No engineering task is active while Owner UAT B is active. Do not start G4-10 or G5.

## 2. Runtime Model Settings v0.1｜PASS / CLOSED

Final implementation/readiness evidence HEAD:

`f615cc49748320f346362430383e6ff074668278`

Formal final review:

`my-world/docs/g4_09r1/G4-09R1P1_INDEPENDENT_REVIEW.md`

Accepted product/runtime truth:

- Main Menu application-level Model Settings exists;
- exact four player-facing profiles: DeepSeek V4 Pro / V4 Flash / Kimi K3 / Kimi K2.7;
- 256K/1M compatibility and reasoning mappings are backend-owned;
- Medium visibly maps to effective High where applicable;
- K2.7 is 256K-only with fixed Thinking ON;
- settings persist outside Games/Source and never store API keys;
- selected-provider-only credentials, no automatic fallback;
- Opening/Narrative/Public d20 Provider phases all use the same current validated runtime profile seam;
- actual UI selection + Save -> real DeepSeek V4 Pro Opening completed;
- actual UI selection + Save -> real Kimi K3 Opening completed;
- canonical Windows export was rebuilt/validated on the accepted launch line;
- production Source remains World 2 / Character 6 / Expansion 1 with exact Public d20 current;
- Owner Games were not modified;
- SQLite remains v4.

The owner-requested prerequisite before First Playable B UAT is therefore closed.

## 3. Current task｜G4-09UATB Owner Product UAT

Owner instructions:

`my-world/docs/g4_09/G4-09UATB_Owner产品验收说明.md`

Preferred product route:

```text
run-game.cmd
-> 模型设置
-> choose desired accepted model/context/reasoning and Save
-> reopen once; confirm effective summary
-> New Game
-> 汉末三国：天下未定
-> 208｜赤壁前夕
-> 刘备
-> 判定与检定：公开 d20
-> real Opening
-> genuinely risky action -> visible d20 card
-> ordinary/no-risk action -> no unnecessary card
-> Save -> Main Menu -> Continue
-> same Game/history/mechanic result
-> Owner product verdict
```

The Owner is not asked to benchmark DeepSeek vs Kimi. They may use the accepted configuration they want for the session.

The product question remains:

> Does `判定与检定：公开 d20` add worthwhile gameplay rather than merely technical state?

Owner returns `PASS` or `FAIL <reason>`.

## 4. After Owner verdict

If PASS:

```text
G4-09UATB Owner Product UAT   PASS / CLOSED
G4-09 First Playable B        PASS / CLOSED
G4-08 Expansion Pack v0.1     PASS / CLOSED
```

Then GPT performs Decision Propagation and inspects the current roadmap authority before shaping G4-10 Runtime Asset Resolution. G4-11 and G4-GATE still remain before G5.

If FAIL, GPT records the exact product seam and routes a bounded correction to the appropriate owner under the correction-budget rules.

Engineering evidence cannot substitute for this Owner verdict.