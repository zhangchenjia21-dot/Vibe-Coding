---
title: my world｜当前状态
status: current-project-status
version: 16.0
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: G6 Surface / Information Architecture Audit
current_owner: GPT product-architecture lane
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
G6 Surface / Information Architecture Audit ACTIVE — GPT
MW-013 Internal Declarative UI Host v0.1    HOLD / NOT AUTHORIZED YET
```

## 2. MW-011 final closeout

```text
MW-011 R1 / IR#1       ENGINEERING PASS / INTEGRATED
R1 Owner UI UAT        NOT PASS — Player Host too thin
MW-011 R2 / IR#2       NOT PASS
MW-011 R3 / IR#3       ENGINEERING PASS / INTEGRATED
R3 Owner UI UAT        PRODUCT PASS
MW-011                  CLOSED
```

Owner UAT confirmed the fresh Zhang Chen `0.1.1` Player Host now presents materially useful Character information.

Final UAT also produced a non-blocking IA observation: some information currently in Player Host may later belong in right-side secondary surfaces once right-side categories are grounded. Current placement remains accepted; future redistribution is deferred and must preserve player-safe disclosure + frozen Source ancestry.

Formal record:

`my-world/docs/mw011/MW-011_OWNER_UAT_R3_RESULT.md`

## 3. G6 canonical route — corrected

The Owner correctly challenged a premature jump to Declarative UI Host.

Canonical roadmap order remains:

```text
Runtime projection → ViewModel → real UI consumer
→ re-audit + Runtime Asset Resolution only for actual visual consumers
→ portrait / scene / authored-map presentation when grounded
→ Character / Relationship / Inventory / Faction / Map / Save real Surfaces
→ Expansion mechanic-state consumer
→ Internal Declarative UI Host v0.1
→ bounded Action Intent
→ responsive / Theme / navigation
→ Owner UAT / visual polish
```

Current disposition:

```text
first real consumer        DONE — MW-011
visual re-entry audit      DONE — visual implementation DEFERRED
next                        ground real RPG Surfaces / right-side IA
Declarative UI Host        HOLD until preceding real consumers justify it
```

Route authority:

`my world/architecture/ui/G6_ROUTE_CORRECTION_AFTER_MW011_UAT_2026-09-06.md`

## 4. Visual Runtime re-entry

Canonical audit:

`my world/architecture/ui/G6_VISUAL_RUNTIME_REENTRY_AUDIT_2026-09-06.md`

Result:

```text
Runtime Asset Resolution implementation = DEFERRED
portrait / scene / authored-map implementation = DEFERRED
```

Reason: real Host placement exists, but no materially authored first-party visual demand currently blocks the product. Do not build media infrastructure mechanically.

Retained invariant:

```text
authored visual presentation != gameplay/world/location/knowledge authority
map image != topology/current location/travel/pathfinding/GIS
```

## 5. Active G6 Surface / IA audit

Before the next implementation task, GPT must determine which right-side Surface is grounded by current real data and which candidate surfaces would be fake/empty.

Candidate families from supporting architecture:

```text
Character
Relationship / Faction
Inventory / Items
Map
Save / Timeline
```

Questions:

- what belongs in Player Host vs World Surface;
- which real domain owner already exists;
- which player-safe projection exists or is minimally missing;
- which first Surface creates real player value;
- whether implementation can remain frontend-only or requires core state/projection work.

Do not create empty tabs, fake HP/location/inventory/relationship state, or generic surface infrastructure without a first real consumer.

## 6. MW-013 disposition

`MW-013 Internal Declarative UI Host v0.1` was shaped too early relative to the canonical route.

```text
MW-013 = HOLD / NOT AUTHORIZED TO IMPLEMENT YET
```

Implementation packet remains historical/future reference:

`my-world/docs/tasks/MW-013_INTERNAL_DECLARATIVE_UI_HOST_V0_1_TASK.md`

Explicit hold notice:

`my-world/docs/tasks/MW-013_HOLD_NOTICE.md`

If Codex has already started, it must stop before further code-changing work, keep branch/worktree isolated, and report current state. Do not merge/push to main and do not discard unknown work.

## 7. Zhang Chen current generation

```text
asset_id: character.han_end.zhang_chen
schema: character_card.v0.2
version: 0.1.1
generation fingerprint:
0b6cb72af535ef6147f71cb7592fe6ba048626dd997acf54c4e6893c848b59e4
```

Protected semantics remain: physical transport; age 24 at chosen T0; no local prior identity/network/history; historical memory is protagonist belief, not guaranteed future truth; no automatic famous-person recognition; written-script-only literacy limitation; finite starting possessions; future meaningful choices remain Player-owned.

## 8. Agent routing — Owner current rule

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

Do not default new work back to Zcode unless Owner explicitly changes routing.

## 9. Immediate route

```text
pause MW-013
→ GPT completes G6 Surface / IA audit
→ choose the first grounded real Surface
→ shape executable task
→ assign Codex or KimiCode according to actual seam/risk
→ Independent Review
→ Owner UAT
→ continue real surfaces / Expansion consumer
→ only then re-evaluate MW-013 Declarative UI Host
```
