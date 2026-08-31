---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-08-31
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-07 First Playable A
current_execution_task: G4-07B Playable UI Integration
semantic_owner: GPT
current_execution_owner: Kimi
owner_uat_required: true
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。不是 Product / Architecture / Status 的替代权威。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Read first

Governance:

1. `AGENTS.md`
2. `my world/MY_WORLD_项目启动总纲_CURRENT.md`
3. `my world/MY_WORLD_核心设计原则_CURRENT.md`
4. `my world/MY_WORLD_架构_CURRENT.md`
5. `my world/MY_WORLD_总体规划路线图_CURRENT.md`
6. `my world/MY_WORLD_CURRENT_STATUS.md`
7. `my world/AGENT_EXECUTION_ROUTING_CURRENT.md`

Implementation:

8. `AGENTS.md`
9. `docs/tasks/G4-07B_PLAYABLE_UI_INTEGRATION_TASK.md`
10. `docs/g4_07a/G4-07A_FIRST_PLAYABLE_OPENING_RUNTIME_IMPLEMENTATION_EVIDENCE.md`
11. `src/应用壳.gd`
12. `src/main.tscn`
13. current Narrative UI under `src/ui/**`
14. `src/最终建局/L3_外交层/原子最终建局公开接口.gd`
15. `src/首次开场/L3_外交层/首次开场公开接口.gd`
16. `src/runtime/当前游戏会话运行时.gd`
17. `src/游戏库/L3_外交层/游戏库公开接口.gd`

Do not default to rereading the whole historical repository.

---

## 2. Current route

```text
G4-05 New Game Wizard          PASS / CLOSED
G4-06 Atomic Final Create      PASS / CLOSED
G4-07A Opening Runtime         PASS / CLOSED
G4-07B Playable UI Integration ACTIVE — KIMI
G4-07 Owner UAT                WAITING FOR G4-07B IR PASS
```

Parent gate G4-07 is product-facing and cannot close without Owner UAT.

---

## 3. G4-07A accepted truth

Accepted implementation/evidence:

```text
opening runtime       dac0e8e4bf655a234ca5b8d0952f6a199373b4af
durable continuation 221710941950198c4fced9c30991bd295fea39ef
evidence / HEAD       fdb6a30ad138c332837f17af1d8c74b5643db44b
```

GPT Independent Review: **PASS / CLOSED**.

Key verified semantics:

- real Han + Afterglow use production G4-06 creation and `open_existing_game()`;
- Opening Context comes from durable Game-local setup/current World;
- Opening module does not accept Source Library or Wizard state;
- newer Source current does not enter an existing Game's Opening context;
- Han early-start future markers remain excluded;
- no-Entry remains no-Entry;
- Guaranteed NPC is canonical knowledge only, with no forced first-scene convergence;
- first request is GM-only, one system message, no fake Player prompt;
- Provider failure/cancel leaves zero durable accepted Opening;
- successful Opening durable exactly once;
- fresh process reopen restores the exact Opening and rejects a second first Opening;
- continuation context after reopen = durable Game-local World + durable Conversation + next real Player action;
- schema remains v4.

Do not reopen G4-07A without new P0 evidence or a concrete integration defect.

---

## 4. Current task｜G4-07B

Formal packet:

`docs/tasks/G4-07B_PLAYABLE_UI_INTEGRATION_TASK.md`

Packet commit:

`064ae8b27d2169f8399e81a36a7d7624efe45fdd`

Formal Code Base:

`fdb6a30ad138c332837f17af1d8c74b5643db44b`

Owner: **Kimi**  
Reviewer: **GPT**  
Return ceiling: **READY FOR INDEPENDENT REVIEW**

Primary target:

```text
Main Menu
→ New Game Wizard / Review
→ Atomic Final Create
→ exact existing-only Game open
→ GM Opening streams in Narrative UI
→ Player sends real action
→ GM continuation
→ Save / exit / reopen / Continue
```

---

## 5. Critical G4-07B semantics

### Stable create-attempt identity

`creation_id` belongs to one frozen Review create attempt, not one click.

- double-click/retry must reuse it;
- successful create ends that Wizard create path;
- editing/re-reviewing before successful create produces a new attempt identity;
- UI must not produce duplicate Games through callback/retry behavior.

### Created-but-not-opened is valid

A durable Game with accepted Conversation = 0 is an `opening-pending` Game.

Provider failure/cancel or app exit after create must not delete/recreate it.

Continue must reopen that exact Game and allow/start the G4-07A first Opening.

### Existing-only

After create and on Continue, never use a first-run seam that silently creates a replacement Game.

### GM-only Opening UI

The first accepted durable entry has empty compatibility `player_text`. Do not render an empty/fake Player bubble.

### Durable continuation

First real Player action after Opening must use G4-07A durable continuation context, not Wizard or mutable Source current.

### Parent UAT remains required

Engineering evidence can make the vertical UAT-ready but cannot prove narrative richness, individuality, anti-convergence or product value.

---

## 6. Protected backend boundaries for Kimi

Expected Kimi production scope:

- `src/应用壳.gd`
- `src/main.tscn`
- `src/ui/**`
- narrow application/presentation glue

Treat these as read-only unless a concrete blocker is proven:

- `src/最终建局/**`
- `src/首次开场/**`
- `src/persistence/**`
- `src/runtime/**`
- `src/provider/**`
- `src/source/**`
- `src/domain/**`
- `src/context/**`
- `src/游戏库/**`
- `src/建局/**`

If Kimi reports a protected backend seam is insufficient, inspect evidence and route the smallest correction to Codex rather than letting frontend work absorb backend ownership.

---

## 7. Independent Review after Kimi returns

Refresh both repo `main` HEADs first. Inspect actual diff/evidence, not report text.

At minimum verify:

1. Kimi stayed within frontend/application scope or clearly reported any necessary exception;
2. Final Create button uses a stable create-attempt `creation_id` across retries/double-click prevention;
3. same UI attempt cannot create two Games;
4. successful create opens the exact returned DB existing-only;
5. create success + Opening failure does not roll back/delete/recreate the Game;
6. Continue on a created Game with accepted Conversation = 0 reaches Opening retry/start rather than blank/dead state;
7. accepted Opening renders as GM-only with no empty fake Player bubble;
8. accepted Conversation >= 1 does not trigger another first Opening;
9. first real Player action uses reviewed durable continuation context and real Provider path;
10. Save / Main Menu or app reopen / Continue restores same Game and history;
11. no-Entry UI path does not introduce hidden defaults;
12. Han early path retains temporal isolation;
13. Afterglow uses the same family-agnostic UI path;
14. Provider failure/cancel/retry has understandable UI and no duplicate durable truth;
15. Windows 1280×720, 960×540 and maximized interaction/layout evidence is real;
16. no debug IDs/fingerprints/schema/task jargon dominate player-facing surfaces;
17. G2/G3/G4-01/G4-04/G4-05/G4-06/G4-07A regressions are meaningful;
18. production schema remains v4 and frozen fixtures unchanged.

If PASS:

- close G4-07B;
- mark G4-07 **READY FOR OWNER UAT**;
- prepare a short Owner UAT route using real Han + Afterglow;
- do **not** close G4-07 until Owner explicitly judges the product experience.

---

## 8. Product UAT questions after G4-07B PASS

Owner should judge, not agents:

- Does Han actually feel grounded in its selected historical moment without future leakage?
- Does Afterglow feel like a genuinely different authored world rather than the same generic GM voice?
- Does the Player Character feel individually grounded?
- Do Guaranteed NPCs avoid collapsing unnaturally into scene one?
- Is Context rich enough for meaningful play without obvious starvation?
- Does free-form play feel like an AI RPG rather than a transcript demo?
- Does Save / exit / Continue preserve trust in the same ongoing Game?

Engineering PASS cannot answer these.
