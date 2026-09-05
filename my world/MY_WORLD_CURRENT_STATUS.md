---
title: my world｜当前状态
status: current-project-status
version: 15.0
created: 2026-08-26
updated: 2026-09-05
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-011 G6 RPG Host ViewModel Baseline / Zcode
current_owner: ZCODE weekend implementation / GPT semantic-review lane
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
MW-011 G6 RPG Host ViewModel Baseline       ACTIVE — ZCODE
```

Owner completed the combined G5 checkpoint and explicitly issued **PASS / proceed to G6**.

Formal implementation-repo closeout:

`my-world/docs/g5_gate/G5_GATE_CLOSEOUT.md`

## 2. G5 final disposition

The combined Owner PASS resolves the remaining product checkpoints:

```text
MW-003 Visual Comfort Theme Pass                  PRODUCT PASS / CLOSED
MW-004 Minimal Player Agency Principle            PRODUCT PASS / CLOSED
MW-005 Three Kingdoms Literary Style Primer R4    PRODUCT PASS / CLOSED
G5-05 Meaningful Choice / Mechanics Integration   PRODUCT PASS / CLOSED
MW-008 Safe Markdown-Lite Narrative Rendering     PRODUCT PASS / CLOSED
G5-06 / MW-009 thin player-safe consumer          PRODUCT ACCEPTED FOR G5
G5-07 World Product Tests                         PRODUCT PASS / CLOSED
```

G5-GATE therefore passes with the intended semantics:

- free-form Narrative can produce durable consequences;
- Save/reopen/Restore preserve coherent current world semantics;
- World Truth / actor Knowledge / Player disclosure have real boundaries;
- stable NPCs may act independently;
- the world can selectively evolve outside direct Player causation and may correctly hold;
- Public d20 mechanics can ground living-world consequences without becoming a second truth system;
- no full-universe simulation or model-output gate forest was introduced.

## 3. Owner UAT observation promoted to G6

The final G5 session confirmed that MW-009 side panels now update correctly and preserve disclosure safety, but the Owner judged their **information density and information hierarchy too thin for the final RPG UI**.

This is accepted as a G6 requirement rather than a G5 defect:

```text
G5 proved a safe real consumer
G6 must make that consumer genuinely useful as an RPG product surface
```

Do not reopen MW-009 merely to add filler fields.

## 4. G6 canonical order

Roadmap and supporting UI architecture require consumer-first development:

```text
Runtime projection
→ presentation-only ViewModel
→ real UI consumer
→ Runtime Asset Resolution only for actual visual consumers
→ portrait / scene / authored-map presentation
→ Character / Relationship / Inventory / Faction / Map / Save real surfaces
→ Expansion mechanic-state consumer
→ Internal Declarative UI Host v0.1
→ bounded Action Intent
→ responsive / Theme / navigation
→ Owner UAT / visual polish
```

Supporting design:

`my world/architecture/ui/声明式UIHost设计.md`

First G6 decision:

`my world/architecture/ui/G6_RPG_HOST_VIEWMODEL_V0_1_DECISION.md`

External World Pack / Mod UI declaration remains G8 work.

## 5. MW-011 active

```text
Work Item: MW-011
Name: G6 RPG Host ViewModel Baseline
Capability-Anchor: G6 RPG Experience & Internal Declarative UI Host
Implementer: Zcode + GLM-5.3-flash
Reviewer: GPT
Revision: 1
Review-Round: 0
Status: ACTIVE — ZCODE
Branch: mw-011-g6-rpg-host-viewmodel-baseline
Worktree: D:/AI/Projects/.worktrees/my-world/mw-011
```

Task packet:

`my-world/docs/tasks/MW-011_G6_RPG_HOST_VIEWMODEL_BASELINE_TASK.md`

Outcome:

```text
existing authoritative/safe data
→ disposable ViewModel
→ useful Player Host + World Surface
```

Minimum product intent:

- Player Host no longer contains only identity and dead space after normal play; it may show safe World/Entry context, recent accepted Player actions and Player-turn count;
- World Surface defaults to Overview rather than always-visible Save controls;
- Save remains a real bounded surface using existing G3 semantics;
- Player-known facts remain sourced through the MW-009 disclosure boundary;
- no fabricated HP/location/inventory/relationship/faction/quest state;
- no generic declarative UI platform yet.

## 6. Routing

Owner weekend override remains active through **2026-09-06 23:59 (+08:00)**:

```text
Zcode + GLM-5.3-flash → primary implementation owner for new code-changing work
GPT                    → semantics / architecture / task shaping / Independent Review
```

At **2026-09-07 00:00 (+08:00)**, absent Owner extension, routing automatically returns to Codex-backend / Kimi-frontend according to the task seam.

All task worktrees:

`D:/AI/Projects/.worktrees/my-world/<task-or-revision>`

## 7. Immediate route

```text
MW-011 implementation
→ GPT Independent Review
→ Owner focused G6 UI UAT
→ next real G6 consumer / visual vertical
→ Internal Declarative UI Host only after real component needs are established
```

Do not manufacture G6 platform work merely to accelerate the appearance of a framework.
