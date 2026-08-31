---
title: my world｜当前状态
status: current-project-status
version: 7.9
created: 2026-08-26
updated: 2026-08-31
phase: G4 Primary Source Assets & Local Game Creation
current_task: G4-08M1 Public d20 Expansion Mechanism
current_owner: Codex
parent_task: G4-08 Expansion Pack v0.1 + First Real Runtime Vertical
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
G4-08M1 Public d20 Mechanism          ACTIVE — CODEX
G4-GATE                               NOT YET
```

Current formal execution packet:

`my-world/docs/tasks/G4-08M1_PUBLIC_D20_EXPANSION_MECHANISM_TASK.md`

Formal Code Base:

`3b5cd80a26091a17d61c5a055637a422a9edb3aa`

Primary execution owner: **Codex**.  
Reviewer / semantic owner: **GPT**.  
Return ceiling: **READY FOR INDEPENDENT REVIEW**.

---

## 2. G4-07 final result｜PRODUCT PASS / CLOSED

Owner completed First Playable A product UAT and returned explicit **PASS**.

Formal record:

`my-world/docs/g4_07/G4-07_OWNER_UAT_RESULT.md`

The accepted no-Expansion vertical remains the regression baseline:

```text
real installed World/Character Source
→ New Game Wizard / Review
→ Atomic Final Create
→ real DeepSeek GM Opening
→ free-form Player actions
→ durable continuation
→ Save
→ Main Menu / Continue same Game
```

G4-08 must preserve this route when no Expansion is selected.

---

## 3. G4-08S0 result｜PASS / CLOSED

Canonical semantic decision:

`my world/architecture/source/G4_EXPANSION_V0_1_PUBLIC_D20_DECISION.md`

First real Expansion:

```text
Display Name  判定与检定：公开 d20
asset_id      exp.check_core.public_d20
asset_type    expansion_pack
schema        expansion_pack.v0.1
capability    action_check.public_d20.v1
slot          action_resolution
```

The historical `the-world` Public d20 material was used only as semantic reference. Current implementation remains Godot-native and governed by the present Source/Game/Runtime boundaries.

Frozen core:

- Expansion is optional `0..N` exact immutable generations;
- duplicate exact Expansion and same capability-slot collision fail closed;
- Public d20 has no hidden World-family restriction;
- roll only when outcome is uncertain and failure has meaningful stakes;
- certainly succeeds / certainly impossible / no-cost repeat → no roll;
- Program owns RNG / total / outcome;
- Risk Structure / Check Proposal freezes before RNG;
- no-check normal turn stays one Provider call;
- real check uses conditional second Provider continuation after durable result;
- same action retry/restart never rerolls;
- Expansion owns resolution, not downstream canonical World consequences;
- no natural-1/natural-20 auto override in v0.1.

---

## 4. Current task｜G4-08M1

M1 implements backend/mechanism only:

```text
Expansion Source contract / Managed Library
→ exact Composition backend
→ capability-slot compatibility
→ Atomic Final Create materialization/provenance
→ Program-owned capability registry
→ structured adjudication envelope
→ freeze-before-RNG
→ Program d20 RNG
→ durable check resolution
→ idempotent retry/restart
→ real DeepSeek resolution continuation
→ UI-neutral mechanic projection
```

M1 does **not** own Wizard/Narrative visual UI.

Required proof includes:

- real Expansion load/install/current/exact immutable generation;
- Public d20 exact selection and Final Create materialization;
- no-Expansion regression;
- normal/advantage/disadvantage Program computation;
- Provider failure after roll does not reroll;
- fresh-process reopen/Continue preserves exact result;
- real Han + real Afterglow Provider evidence;
- no executable Source plugin support.

If production persistence schema changes from v4, the implementation must use the smallest migration required and prove prior Game compatibility.

---

## 5. UI / product progression

After M1 reaches READY FOR INDEPENDENT REVIEW:

1. GPT refreshes current `main` and reviews actual implementation/evidence;
2. if M1 PASS, GPT issues a separate Kimi task (`G4-08B`) for:
   - Expansion selection in New Game Wizard;
   - Review projection;
   - lightweight public check mechanic card;
3. GPT reviews actual UI integration;
4. then G4-09 First Playable B / Owner UAT validates the real Expansion product experience.

First-generation UI does not require a player click-to-roll button or dice animation.

---

## 6. G4-08 acceptance direction

G4-08 eventually must prove:

```text
same playable World + Character route
+ exact selected Public d20 Expansion
→ Final Create pins/materializes it
→ real Runtime consumes it
→ risky Player action produces a real Program-owned check
→ public result constrains real DeepSeek continuation
→ Save / Continue preserves it
→ ordinary no-check action does not become a dice pipeline
→ no-Expansion route remains unchanged
```

Do not claim G4-08 PASS from M1 alone. Do not start G5 before the remaining G4 route and G4-GATE are complete.