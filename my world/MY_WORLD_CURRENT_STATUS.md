---
title: my world｜当前状态
status: current-project-status
version: 15.9
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-013 Internal Declarative UI Host v0.1
current_owner: Codex implementation / GPT semantic-review lane
parent_task: G6 RPG Experience & Internal Declarative UI Host
semantic_owner: GPT
owner_uat_required: true
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
MW-011 G6 RPG Host / Player Profile outcome PRODUCT PASS / CLOSED
MW-012 Zhang Chen Player Character Card     ENGINEERING PASS / INTEGRATED / PRODUCT INGRESS ACCEPTED
G6 Visual Runtime re-entry                  AUDITED — IMPLEMENTATION DEFERRED
MW-013 Internal Declarative UI Host v0.1    READY FOR CODEX
```

G5 remains closed. G6 remains active.

## 2. MW-011 final closeout

Engineering lineage:

```text
MW-011 R1 / IR#1              ENGINEERING PASS / INTEGRATED
MW-011 R1 Owner UI UAT        NOT PASS — Player Host too information-thin
MW-011 R2 / IR#2              NOT PASS
MW-011 R3 / IR#3              ENGINEERING PASS / INTEGRATED
MW-011 R3 Owner UI UAT        PRODUCT PASS
MW-011                         CLOSED
```

Reviewed R3 branch head:

`my-world@78bd5ce26b5ec8a465a9f5d6fbcdb536925d5fc0`

Reviewed production/content test HEAD:

`my-world@16c42d576b28c6119c26ff310b426d0caec202ce`

Integration commit:

`my-world@12eedba6a6da47d351d33fb544efbdaa188c85b8`

Formal records:

- `my-world/docs/mw011/MW-011_INDEPENDENT_REVIEW_IR3.md`
- `my-world/docs/mw011/MW-011_R3_INTEGRATION_VERIFICATION.md`
- `my-world/docs/mw011/MW-011_OWNER_UAT_R3_RESULT.md`

Owner UAT used a fresh Zhang Chen `0.1.1` Game and confirmed the Player Host now presents materially useful Character information. The prior large-dead-space / almost-empty-profile product defect is resolved for MW-011.

Accepted Player profile chain remains:

```text
optional bounded Character Card v0.2 player_profile
→ selected Character projection
→ Final Create freezes exact profile into Game-local source_projection
→ separate fail-closed Player Character Profile Projection
→ MW-011 presentation-only ViewModel
→ rich bounded Player Host
```

Protected boundaries:

- old Games do not live-fetch/backfill newer Character Source generations;
- `player_profile` is presentation-only, not gameplay/world authority;
- raw `semantic_sections`, `gm_reference`, `gm_private`, `catalog_summary`, internal IDs/hashes/fingerprints and Source-current bytes do not reach the Player profile surface;
- MW-009 remains the owner of current Player-known facts;
- Narrative remains the dominant center surface;
- no stat ontology, Inventory mechanics, generic external UI DSL, Mod schema, Provider summarization or persistence table was introduced by MW-011.

## 3. Deferred left/right information-architecture requirement

Final MW-011 Owner UAT also established a non-blocking G6 IA observation:

> Some information currently displayed in the left Player Host may eventually belong in future right-side World/secondary surfaces after the right-side information set and category model are grounded.

Current disposition:

```text
current left placement = ACCEPTED
future redistribution = DEFERRED G6 IA work
```

Do not reopen MW-011 merely to move fields now.

Supporting UI design already defines long-term intent:

```text
Player Host
→ 我是谁 / 我现在怎么样
→ portrait / identity / high-frequency state / equipment-resource summary

World Surface Host
→ 这个世界有哪些值得查看的信息
→ candidate surfaces: Overview / Characters / Relationship-Faction / Quest-Clue / Items / Map / Save / Timeline
```

Specific right-side tabs/categories remain unfrozen. A future surface must have a real domain owner and player-safe projection before implementation; do not create empty tabs or fake RPG state.

## 4. Zhang Chen current generation

Accepted first-party Character generation:

```text
asset_id: character.han_end.zhang_chen
schema: character_card.v0.2
version: 0.1.1
generation fingerprint:
0b6cb72af535ef6147f71cb7592fe6ba048626dd997acf54c4e6893c848b59e4
```

Visible authored profile order:

```text
背景
性格
能力
局限
初始目标
行为原则
随身物品
```

Protected MW-012 semantics remain in force: physical transport, age 24 at selected T0, no local prior identity/network/history, historical memory as protagonist belief rather than guaranteed future truth, no automatic famous-person recognition, written-script-only literacy limitation, finite bounded starting possessions, and Player ownership of future meaningful choices.

## 5. G6 visual-runtime re-entry audit

Canonical audit:

`my world/architecture/ui/G6_VISUAL_RUNTIME_REENTRY_AUDIT_2026-09-06.md`

Historical G4 visual work reached its required G6 re-entry gate after MW-011 produced real UI consumers.

Result:

```text
Runtime Asset Resolution implementation = DEFERRED
portrait / scene / authored-map implementation = DEFERRED
```

Reason: Player/Narrative/World placement slots are real, but no materially authored first-party visual asset currently blocks or materially degrades the accepted product flow. Do not build media infrastructure merely to satisfy roadmap ordering.

Retained invariants:

- authored visual presentation != gameplay/world/location/knowledge authority;
- canonical absence is valid;
- missing/broken visuals should fail soft;
- fallback must not impersonate authored Source truth;
- authored map image != topology/current location/travel/pathfinding/GIS;
- old-Game presentation override remains deferred until a real use case exists.

## 6. MW-013 — Internal Declarative UI Host v0.1

Canonical architecture:

`my world/architecture/ui/G6_INTERNAL_DECLARATIVE_UI_HOST_V0_1_DECISION.md`

Executable task:

`my-world/docs/tasks/MW-013_INTERNAL_DECLARATIVE_UI_HOST_V0_1_TASK.md`

Identity:

```text
Work Item: MW-013
Name: Internal Declarative UI Host v0.1
Capability-Anchor: G6 RPG Experience & Internal Declarative UI Host
Primary Implementer: Codex
Reviewer: GPT
Revision: 1
Review-Round: 0
Status: READY FOR CODEX
Branch: mw-013-internal-declarative-ui-host-v01
Worktree: D:/AI/Projects/.worktrees/my-world/mw-013
Return ceiling: READY FOR INDEPENDENT REVIEW
```

Why Codex: this is architecture-critical shared UI infrastructure. Mistakes could weaken the player-safe projection boundary and prematurely constrain the later G8 external UI contract.

Required outcome:

```text
existing safe RPG Host ViewModel
→ bounded program-internal UI definition material
→ reusable Internal Declarative UI Host renderer
→ Godot Controls
```

v0.1 must prove reuse with at least:

1. Player Host structured profile/information;
2. World Overview structured information.

Only current grounded component kinds are authorized:

```text
section
status_list
fact_list
```

Definitions are program-internal and generated from already-safe ViewModel data. No Source/Mod UI declaration, arbitrary binding/expression/NodePath/callback/runtime query/filesystem/Provider capability, new domain owner, persistence schema or Action Intent is authorized.

Save controls and Narrative/Composer remain imperative and unchanged.

## 7. G6 canonical route

Canonical roadmap remains consumer-first:

```text
Runtime projection
→ presentation-only ViewModel
→ real UI consumer                         DONE / MW-011
→ visual-runtime re-entry audit            DONE / DEFER IMPLEMENTATION
→ grounded real surfaces                   CONTINUE ONLY AS DOMAIN OWNERS EXIST
→ Internal Declarative UI Host v0.1        ACTIVE / MW-013
→ bounded Action Intent
→ responsive / Theme / navigation
→ Owner UAT / visual polish
```

The visual sub-route may re-enter later when a real authored first-party portrait/scene/map demand exists.

External World Pack / Mod UI declaration remains G8 work.

## 8. Protected G5 semantics

- accepted free-form Narrative remains primary and is not gated by semantic/Knowledge/Agency/Evolution extraction;
- World Truth != actor Knowledge != human-player disclosure;
- stable NPCs may act independently;
- World Evolution may hold or selectively advance;
- Public d20 remains program-owned mechanics grounding rather than a second world truth;
- Save/reopen/Restore currentness remains authoritative;
- Literary Style Reference remains expression-only;
- raw accepted Narrative bytes remain authoritative; Markdown-lite remains disposable UI projection.

## 9. Agent routing — Owner current rule

For new implementation tasks GPT assigns between Codex and KimiCode by complexity, importance, blast radius and architectural authority:

```text
Codex
→ high-complexity / high-importance / high-blast-radius
→ Runtime / Source / Persistence / Save / world semantics / authority boundaries
→ cross-module refactors, hard debugging, critical integration
→ architecture-critical UI tightly coupled to core state

KimiCode
→ bounded, clear, lower-risk implementation
→ frontend/UI/interaction on established seams
→ ordinary surfaces/consumers, content tooling, tests, small refactors
→ batch content production once contracts are stable

GPT
→ product semantics / architecture / Task Shaping / assignment / Independent Review

Owner
→ Product UAT / explicit product verdict
```

Cleanly separable mixed work may be split `Codex mechanism/backend + KimiCode UI/consumer`. If a task cannot be safely split and touches core authority/persistence/runtime, prefer Codex.

Do not default new work back to Zcode unless Owner explicitly changes routing.

## 10. Immediate route

```text
Codex executes MW-013 from refreshed current main/governance
→ push exact clean candidate
→ GPT actual-code Independent Review
→ if Engineering PASS, integrate reviewed outcome
→ Owner focused UAT that declarative rendering preserves the accepted Player/World experience
→ then shape the next grounded G6 capability
```
