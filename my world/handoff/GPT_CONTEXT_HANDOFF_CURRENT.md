---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-02
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-09 First Playable B
current_execution_task: G4-09UATB Owner Product UAT
semantic_owner: GPT
current_execution_owner: OWNER
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Current state

```text
G4-07 First Playable A                PASS / CLOSED
G4-08 Expansion Pack v0.1             ACTIVE pending Owner verdict
G4-08M1 Public d20 Mechanism          PASS / CLOSED
G4-08B Public d20 UI Integration      PASS / CLOSED
G4-09 First Playable B                ACTIVE pending Owner verdict
G4-09P1 Owner UAT B Production Prep   PASS / CLOSED
G4-09R1 Runtime Model Settings v0.1   PASS / CLOSED
G4-09R1M1 Backend Mechanism           PASS / CLOSED
G4-09R1B1 Settings UI                 PASS / CLOSED AFTER CORRECTION-01
G4-09R1P1 Final Integration/Freshness PASS / CLOSED
G4-09UATB Owner Product UAT           ACTIVE — OWNER
G4-GATE                               NOT YET
```

No Codex or Kimi task is active. Do not start G4-10 while Owner UAT is active.

## 2. Read first

Governance:

1. `my world/MY_WORLD_CURRENT_STATUS.md`
2. `my world/architecture/foundation/G4_RUNTIME_MODEL_SETTINGS_V0_1_DECISION.md`
3. `my world/MY_WORLD_总体规划路线图_CURRENT.md` only after Owner verdict when shaping the next task

Implementation:

4. `AGENTS.md`
5. `docs/g4_09r1/G4-09R1P1_INDEPENDENT_REVIEW.md`
6. `docs/g4_09/G4-09UATB_Owner产品验收说明.md`

## 3. Runtime Model Settings v0.1 accepted

Final readiness evidence HEAD:

`f615cc49748320f346362430383e6ff074668278`

Accepted:

- Main Menu app-level Model Settings;
- exact four display models: DeepSeek V4 Pro / DeepSeek V4 Flash / Kimi K3 / Kimi K2.7;
- 256K/1M compatibility, Medium -> effective High, K2.7 256K-only fixed Thinking ON;
- application-local settings outside Game/Source;
- selected-provider credentials only and no fallback;
- one current validated runtime profile seam across Opening/Narrative/Public d20;
- real UI-selected DeepSeek V4 Pro Opening completed;
- real UI-selected Kimi K3 Opening completed;
- canonical Windows export rebuilt/validated after UI acceptance;
- production Source inventory World 2 / Character 6 / Expansion 1, exact Public d20 current;
- Owner Games unchanged;
- SQLite v4 unchanged.

R1P1 review note: Codex UAT wording used informal `208 赤壁前夜`; canonical Source display is `208｜赤壁前夕`. GPT corrected the Owner-facing UAT record during closeout. No implementation correction was required.

## 4. Current Owner UAT route

Owner instructions:

`my-world/docs/g4_09/G4-09UATB_Owner产品验收说明.md`

Route:

```text
run-game.cmd
-> Main Menu 模型设置
-> choose desired model / context / reasoning and Save
-> reopen settings; confirm effective summary
-> New Game
-> World: 汉末三国：天下未定
-> Entry: 208｜赤壁前夕
-> Character: 刘备
-> Expansion: 判定与检定：公开 d20
-> real GM Opening
-> one genuinely risky action -> visible d20 card
-> one ordinary/no-risk action -> no unnecessary card
-> Save -> Main Menu -> Continue
-> verify same Game/history/mechanic result
-> Owner verdict
```

Optional risky-action example:

`趁夜亲自潜近曹军水寨，越过警戒线侦察船阵，尽量不惊动哨兵。`

Owner UAT is not a DeepSeek-vs-Kimi benchmark. Owner may choose any accepted runtime configuration they want.

Product question:

> Does `判定与检定：公开 d20` add worthwhile gameplay rather than merely technical/database state?

Owner can return simply:

```text
PASS
```

or:

```text
FAIL
<哪里不好玩、哪里不自然，或者哪里坏了>
```

## 5. After Owner verdict

If PASS:

1. refresh both `main` branches;
2. record Owner PASS under implementation docs;
3. close `G4-09UATB`, `G4-09 First Playable B`, and `G4-08 Expansion Pack v0.1`;
4. Decision Propagation to implementation `AGENTS.md`, Current Status and this handoff;
5. inspect current roadmap authority before shaping G4-10 Runtime Asset Resolution;
6. do not start G5 before G4-GATE.

If FAIL:

1. refresh both mains;
2. record exact Owner failure;
3. classify seam: UI -> Kimi, mechanism/runtime/persistence -> Codex, semantic/product rule -> GPT first;
4. use correction-01 / correction-02 / redesign budget;
5. preserve accepted boundaries not implicated by the failure.

Engineering evidence does not replace the Owner product verdict.