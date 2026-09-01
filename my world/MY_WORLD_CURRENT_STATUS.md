---
title: my world｜当前状态
status: current-project-status
version: 8.3
created: 2026-08-26
updated: 2026-09-01
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-09P1 Owner UAT B Production Preparation
current_owner: Codex
parent_task: G4-09 First Playable B: Add Real Expansion
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
G4-09P1 Owner UAT B Production Prep   ACTIVE — CODEX
G4-GATE                               NOT YET
```

Current formal packet:

`my-world/docs/tasks/G4-09P1_OWNER_UAT_B_PRODUCTION_PREP_TASK.md`

Formal Code Base:

`08287d28a9cacfc7795c7c7a35ef4739ff9faf2c`

Accepted G4-08B correction review:

`my-world/docs/g4_08b/G4-08BC01_INDEPENDENT_REVIEW.md`

Primary execution owner: **Codex**.  
Reviewer / semantic owner: **GPT**.

G4-08 parent remains active until the real Expansion product vertical receives Owner UAT B PASS.

---

## 2. G4-08B final result｜PASS / CLOSED

G4-08B initial implementation established the real Public d20 player path but GPT Independent Review found two player-facing blockers: unstable/duplicated mechanic-card projection and silent fallback for unknown action-resolution capability. Kimi correction-01 closed both.

Reviewed correction/evidence HEAD:

`08287d28a9cacfc7795c7c7a35ef4739ff9faf2c`

Accepted UI/product integration now proves:

```text
New Game Wizard
→ installed Expansion inventory
→ explicit exact 0..N selection / explicit none
→ Review exact selection
→ unchanged Final Create
→ Game-local capability-aware routing
→ stable action_id
→ ActionAdjudication L3 Host
→ Program-owned result
→ public mechanic card
→ GM continuation
→ durable redraw on Continue / Load
```

Accepted specifics:

- no Expansion preserves the existing G4-07 Narrative path;
- Public d20 Games never call `conversation.begin_turn()` before the Host;
- UI never computes die faces, selected roll, total or outcome;
- transient and historical mechanic projection use durable `check_id` identity;
- one durable check has at most one visible card;
- accepted history is stable as `Player action → mechanic card → GM narrative` live and on redraw;
- retry/reopen uses the same durable action identity and does not reroll;
- NO_CHECK shows no dice card;
- Public d20 accepted turns do not expose legacy generic Regenerate in v0.1;
- unknown materialized `action_resolution` capability fails visibly, gates Player input and never falls back to legacy Provider play;
- explicit none has direct Wizard → Review → frozen payload evidence;
- protected backend paths remain unchanged and SQLite remains schema v4.

Correction evidence remains green with prior real DeepSeek Han Public d20 vertical; Provider-facing message semantics did not change.

---

## 3. G4-09 product target

Canonical roadmap target:

```text
accepted World + Character combination
+ 1 real Expansion
→ New Game
→ exact binding
→ real DeepSeek play
→ observable Expansion effect
→ Save / reopen / Continue
→ Owner UAT B
```

Gate question:

> **Does the Expansion genuinely add worthwhile gameplay rather than merely adding database records?**

Engineering PASS does not answer this question. Owner Product UAT is required.

---

## 4. Current task｜G4-09P1 production preparation

Before Owner UAT, prepare the real local product environment without asking the Owner to manipulate managed internals.

Required preparation:

### Production Source Library

Use only the Managed Source Library public API against:

```text
user://my-world/source-library
```

Install or verify the exact real Public d20 Expansion:

```text
Display Name  判定与检定：公开 d20
asset_id      exp.check_core.public_d20
asset_type    expansion_pack
schema        expansion_pack.v0.1
capability    action_check.public_d20.v1
slot          action_resolution
```

Current repository package authority:

`res://tests/fixtures/g4_08m1/判定与检定_公开d20`

Do not manually copy files into managed Source storage. Do not delete or normalize Owner World/Character generations and do not modify existing Owner games.

### Launch freshness

Canonical Owner launch path remains:

`run-game.cmd`

Validate current Windows export using:

```powershell
.\run-game.ps1 -ValidateExportOnly
```

The current export must include accepted G4-08B/BC01 code; stale outputs are not launch eligible.

### UAT packet

Codex prepares a concise product-only UAT instruction record under `my-world/docs/g4_09/`.

---

## 5. Planned Owner UAT B route

After GPT reviews G4-09P1, Owner UAT B should use the established Han baseline, preferably:

```text
World      汉末三国：天下未定
Entry      208 / 赤壁前夕
Player     刘备
NPC        孙权 (optional guaranteed)
Expansion  判定与检定：公开 d20
```

Product actions:

1. launch through the canonical launcher;
2. New Game and explicitly select Public d20;
3. verify Review shows the Expansion;
4. create/open the Game and complete the real DeepSeek Opening;
5. perform one genuinely risky action and observe a public Program-owned d20 result;
6. verify the GM continuation respects the result;
7. perform one ordinary/no-risk action and verify no dice card appears unnecessarily;
8. Save → Main Menu → Continue and confirm the same Game/history/mechanic result persists;
9. judge whether Public d20 improves play.

Owner verdict is explicit `PASS` or `FAIL` with concise product notes.

---

## 6. Protected boundaries

G4-09P1 does not reopen:

- Source schema / Managed Library semantics;
- Composition;
- Final Create;
- Public d20 semantics/RNG/durable identity;
- persistence schema;
- Provider protocol;
- G4-08B accepted interaction design;
- G8 in-game Source import;
- G5/G6 systems.

---

## 7. Next progression

```text
G4-09P1 production prep — Codex
→ GPT Independent Review
→ G4-09UATB Owner Product UAT
→ explicit Owner PASS / FAIL
→ Decision Propagation
```

If Owner PASS, close the First Playable B / G4-08 product vertical and continue remaining G4 work (`G4-10`, `G4-11`, then `G4-GATE`).

Do not declare G4-09 PASS, G4-08 Product PASS, or G4-GATE PASS before explicit Owner UAT B verdict.
