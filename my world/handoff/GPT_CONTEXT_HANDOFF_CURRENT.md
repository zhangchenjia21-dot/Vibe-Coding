---
title: my world｜GPT Context Handoff
status: current-handoff-navigation
created: 2026-08-29
updated: 2026-09-01
project: my world
implementation_repo: zhangchenjia21-dot/my-world
governance_repo: zhangchenjia21-dot/Vibe-Coding
current_parent_task: G4-09 First Playable B: Add Real Expansion
current_execution_task: G4-09P1 Owner UAT B Production Preparation
semantic_owner: GPT
current_execution_owner: Codex
---

# my world｜GPT CONTEXT HANDOFF CURRENT

> 接管导航 / 最小充分摘要。新 GPT 必须先刷新两个 GitHub `main` HEAD。

## 1. Current state

```text
G4-07 First Playable A                PASS / CLOSED
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

G4-08 parent remains active until Owner UAT B establishes Product PASS for the real Expansion vertical.

---

## 2. Read first

Governance:

1. `my world/MY_WORLD_CURRENT_STATUS.md`
2. `my world/MY_WORLD_总体规划路线图_CURRENT.md` — G4-09 section
3. `my world/architecture/source/G4_EXPANSION_V0_1_PUBLIC_D20_DECISION.md`

Implementation:

4. `AGENTS.md`
5. `docs/g4_08b/G4-08BC01_INDEPENDENT_REVIEW.md`
6. `docs/tasks/G4-09P1_OWNER_UAT_B_PRODUCTION_PREP_TASK.md`
7. `run-game.ps1`
8. Source Library public interface / Expansion install seam

---

## 3. G4-08B final accepted state

Reviewed correction/evidence HEAD:

`08287d28a9cacfc7795c7c7a35ef4739ff9faf2c`

G4-08BC01 and G4-08B are **PASS / CLOSED**.

Accepted player vertical:

```text
New Game Wizard
→ installed Expansion inventory
→ explicit exact 0..N selection / explicit none
→ Review exact selection
→ unchanged Final Create
→ Game-local capability routing
→ stable action_id
→ Public d20 ActionAdjudication Host
→ Program-owned result
→ public mechanic card
→ GM continuation
→ durable Continue / Load redraw
```

Important accepted invariants:

- no Expansion = unchanged G4-07 route;
- UI never owns RNG/total/outcome;
- one durable `check_id` → at most one visible mechanic-card projection;
- accepted history = `Player action → mechanic card → GM narrative` live and on redraw;
- retry/reopen reuses stable action identity and never rerolls;
- NO_CHECK shows no dice card;
- d20 accepted turns have no legacy generic Regenerate in v0.1;
- unknown materialized `action_resolution` capability fails visibly, gates input and never falls back to legacy play;
- explicit none has direct Wizard/Review/frozen-payload evidence;
- SQLite remains v4; protected backend paths unchanged.

Review note: correction evidence prose is stronger than some individual focused child-order/count assertions, but production code directly enforces the invariant and no contradictory path remains. Future evidence should assert exact UI order/count more explicitly.

---

## 4. Current task｜G4-09P1

Packet:

`my-world/docs/tasks/G4-09P1_OWNER_UAT_B_PRODUCTION_PREP_TASK.md`

Formal Code Base:

`08287d28a9cacfc7795c7c7a35ef4739ff9faf2c`

Owner: **Codex**. Reviewer: **GPT**.

Purpose: prepare the Owner's actual local product environment for Product UAT B, not add features.

### Production Source Library

Default managed root:

```text
user://my-world/source-library
```

Use only the SourceLibrary public install API. Do not manually copy into managed internals.

Install or verify the accepted exact Public d20 package:

`res://tests/fixtures/g4_08m1/判定与检定_公开d20`

Identity:

```text
asset_id      exp.check_core.public_d20
schema        expansion_pack.v0.1
capability    action_check.public_d20.v1
slot          action_resolution
```

If already installed exactly, reuse/verify. Do not delete generations, rewrite existing World/Character currents, or modify Owner games.

### Launch freshness

Canonical Owner launch remains:

```text
run-game.cmd
```

Validate current export through:

```powershell
.\run-game.ps1 -ValidateExportOnly
```

No competing launcher and no secret output.

### UAT instructions

Codex creates a concise Owner product UAT B record under `docs/g4_09/`.

---

## 5. G4-09 Owner UAT B target

Roadmap definition:

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

Preferred Owner route:

```text
World      汉末三国：天下未定
Entry      208 / 赤壁前夕
Player     刘备
NPC        孙权 (optional guaranteed)
Expansion  判定与检定：公开 d20
```

Owner should verify:

- Review visibly lists Public d20;
- real DeepSeek Opening works;
- a genuinely risky action naturally produces a public check;
- the mechanic card is understandable and GM continuation respects outcome;
- an ordinary/no-risk action does not unnecessarily roll;
- Save → Main Menu → Continue preserves the same Game/history/result;
- most importantly: the Expansion feels like worthwhile added gameplay.

Owner verdict must be explicit `PASS` or `FAIL`.

---

## 6. After Codex returns

GPT must refresh both `main` heads and inspect actual prep utility/evidence.

Verify:

- production Source install used the public API, not filesystem surgery;
- exact Public d20 identity/version/fingerprint is present;
- Owner games were not modified;
- existing World/Character library was not destructively normalized;
- current Windows export freshness passes;
- no secrets printed;
- UAT B instructions are product-only and minimal.

If PASS:

```text
G4-09P1 PASS / CLOSED
→ G4-09UATB ACTIVE — OWNER
```

Then tell the Owner exactly how to launch and what product path to test.

---

## 7. Later progression

```text
G4-09P1 Codex
→ GPT IR
→ G4-09UATB Owner Product UAT
→ explicit Owner PASS / FAIL
→ Decision Propagation
```

If Owner PASS, close First Playable B / G4-08 product vertical and continue G4-10 Runtime Asset Resolution, then G4-11 Two Primary Asset Families Reality Test, then G4-GATE.

Do not start G5 before G4-GATE.
