---
title: my world｜当前状态
status: current-project-status
version: 16.1
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: G6 Surface / Information Architecture Audit + Owner Discussion
current_owner: GPT product-architecture lane + Owner
parent_task: G6 RPG Experience & Internal Declarative UI Host
semantic_owner: GPT
owner_uat_required: false
context_handoff: handoff/GPT_CONTEXT_HANDOFF_CURRENT.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
---

# my world｜CURRENT STATUS

## 1. Stage state

```text
G1 Foundation                               PASS / CLOSED
G2 AI Conversation Spine                    PASS / CLOSED
G3 Persistence / Save / Timeline            PASS / CLOSED
G4 Primary Source Assets & Local Game       PASS / CLOSED
G5 World Semantics & GM Runtime             PRODUCT PASS / CLOSED
G5-GATE                                     PRODUCT PASS

G6 RPG Experience & Internal Declarative UI Host ACTIVE
MW-011 RPG Host / Player Profile outcome    PRODUCT PASS / CLOSED
MW-012 Zhang Chen Player Character Card     ENGINEERING PASS / INTEGRATED / PRODUCT INGRESS ACCEPTED
G6 Visual Runtime re-entry                  AUDITED — IMPLEMENTATION DEFERRED
G6 Surface / Information Architecture       ACTIVE — OWNER + GPT DISCUSSION
MW-013 Internal Declarative UI Host v0.1    HOLD / NOT AUTHORIZED YET
```

## 2. Core governance refresh — 2026-09-06

After MW-011 Product PASS and the Owner's route challenge, the project core files were refreshed to reflect actual G6 state rather than stale G4/G5 planning.

Updated current authorities:

```text
MY_WORLD_项目启动总纲_CURRENT.md        v2.1 / current_phase G6
MY_WORLD_核心设计原则_CURRENT.md        v2.0 / G6
MY_WORLD_架构_CURRENT.md                v3.0 / current_phase G6
MY_WORLD_总体规划路线图_CURRENT.md       v4.0 / current_phase G6
AGENT_EXECUTION_ROUTING_CURRENT.md      v2.0 / Codex + KimiCode routing
README.md                               refreshed G6 entry map
MY_WORLD_CURRENT_STATUS.md              v16.1
```

New discussion authority:

`architecture/ui/G6_SURFACE_INFORMATION_ARCHITECTURE_DRAFT_V0_1.md`

Status of that file:

```text
DRAFT / FOR OWNER DISCUSSION
NOT FROZEN
NOT IMPLEMENTATION AUTHORITY
```

## 3. MW-011 final closeout

```text
MW-011 R1 / IR#1       ENGINEERING PASS / INTEGRATED
R1 Owner UI UAT        NOT PASS — Player Host too thin
MW-011 R2 / IR#2       NOT PASS
MW-011 R3 / IR#3       ENGINEERING PASS / INTEGRATED
R3 Owner UI UAT        PRODUCT PASS
MW-011                  CLOSED
```

Formal records:

- `my-world/docs/mw011/MW-011_INDEPENDENT_REVIEW_IR3.md`
- `my-world/docs/mw011/MW-011_R3_INTEGRATION_VERIFICATION.md`
- `my-world/docs/mw011/MW-011_OWNER_UAT_R3_RESULT.md`

Owner UAT confirmed fresh Zhang Chen `0.1.1` now displays materially useful Character information.

Owner also explicitly observed that some current left-panel material may later belong in right-side secondary surfaces. Current placement remains accepted; future redistribution is G6 IA work, not an MW-011 reopening.

## 4. G6 corrected canonical route

```text
Runtime projection / ViewModel / first real consumer    DONE — MW-011
→ Visual Runtime re-entry audit                         DONE — implementation DEFERRED
→ real RPG Surfaces / player information architecture  CURRENT
→ Expansion mechanic-state consumer
→ Internal Declarative UI Host v0.1
→ bounded Action Intent
→ responsive / Theme / navigation
→ Owner UAT / visual polish
```

Route correction authority:

`architecture/ui/G6_ROUTE_CORRECTION_AFTER_MW011_UAT_2026-09-06.md`

## 5. Current Surface / IA discussion

Historical evidence from `zhangchenjia21-dot/the-world` is being reused as product evidence, not implementation authority.

The old The World panel's proven player-facing mother taxonomy was:

```text
概览
角色
人物
行囊
事务
系统
存档
```

Current `my world` discussion draft extends the long-term candidate set to:

```text
概览
角色
人物
行囊
事务
系统
地图
存档
```

Core IA principle:

> **Workspace / Domain truth is organized for maintenance; UI is organized for player decisions.**

Current draft three-Host split:

```text
Player Host
→ high-frequency Player HUD

Narrative Host
→ GM Narrative + Player natural-language action
→ primary visual/interaction surface

World Surface Host
→ Character / People / Journal / Inventory / Mechanics / Map / Save etc.
→ only when real Domain + player-safe projection exist
```

Current strongest first Surface candidate:

```text
角色 / Character Sheet
```

Second candidate:

```text
人物 / People
```

No code task is authorized until Owner + GPT freeze the first Surface outcome.

## 6. Surface appearance rule

A new player-facing Surface requires:

```text
real player question
+ real domain owner
+ player-safe projection
+ non-trivial product value
```

Do not create:

- fake HP / location;
- fake dynamic Inventory from authored starting possessions;
- keyword-guessed Quest/Thread state;
- omniscient NPC/relationship/faction views;
- empty RPG tabs;
- generic Surface infrastructure with no first consumer.

## 7. Visual Runtime disposition

Canonical audit:

`architecture/ui/G6_VISUAL_RUNTIME_REENTRY_AUDIT_2026-09-06.md`

Result:

```text
Runtime Asset Resolution implementation = DEFERRED
portrait / scene / authored-map implementation = DEFERRED
```

Re-enter only when there is a real authored first-party visual consumer or product outcome blocked by visual absence.

## 8. MW-013 disposition

`MW-013 Internal Declarative UI Host v0.1` was shaped before the Owner challenged the sequence.

Current formal state:

```text
MW-013 = HOLD / NOT AUTHORIZED TO IMPLEMENT YET
```

References:

- `my-world/docs/tasks/MW-013_INTERNAL_DECLARATIVE_UI_HOST_V0_1_TASK.md`
- `my-world/docs/tasks/MW-013_HOLD_NOTICE.md`

If Codex has already created isolated work, preserve branch/worktree and stop. Do not merge or continue until explicit re-authorization.

## 9. Zhang Chen current generation

```text
asset_id: character.han_end.zhang_chen
schema: character_card.v0.2
version: 0.1.1
generation fingerprint:
0b6cb72af535ef6147f71cb7592fe6ba048626dd997acf54c4e6893c848b59e4
```

Protected semantics remain unchanged.

## 10. Agent routing — current Owner rule

```text
GPT
→ product semantics / architecture / Task Shaping / assignment / Independent Review

Codex
→ high-complexity / architecture-critical / high-blast-radius implementation

KimiCode
→ bounded UI / interaction / ordinary surfaces / content tooling / tests

Owner
→ Product UAT / explicit product verdict
```

Do not default new work back to Zcode unless Owner explicitly changes routing.

## 11. Immediate route

```text
Owner + GPT discuss G6 Surface / IA Draft
→ freeze Player Host vs World Surface responsibilities
→ choose first real Surface
→ shape exact executable task
→ assign Codex or KimiCode according to actual seam/risk
→ GPT Independent Review
→ Owner UAT
→ repeat grounded Surface work
→ mechanic-state consumer
→ only then re-evaluate MW-013 Declarative UI Host
```
