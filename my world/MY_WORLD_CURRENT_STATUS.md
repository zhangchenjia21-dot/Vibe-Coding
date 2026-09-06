---
title: my world｜当前状态
status: current-project-status
version: 16.7
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-014 Model-driven Character + Important Experiences Curation v0.1
current_owner: Codex implementation lane
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
G6 Character + Important Experiences        SEMANTIC / IA FROZEN
MW-014 Model-driven Information Curation    READY FOR CODEX
MW-013 Internal Declarative UI Host v0.1    HOLD / NOT AUTHORIZED YET
```

## 2. Frozen G6 information architecture direction

Session shell:

```text
Player Status Host
→ portrait + live mechanics/status HUD only
→ no “who am I” biography/profile ownership

Narrative Host
→ GM Narrative + Player natural-language action
→ primary visual/interaction surface

World Information Host
→ active player information surfaces
```

Right-side mother taxonomy:

```text
概览
角色
重要经历
人物
事务
行囊
系统
地图
存档
```

Only grounded Surfaces proceed; no fake RPG state or empty tabs.

## 3. Character + Important Experiences — FROZEN

Canonical product/architecture authority:

`architecture/ui/G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_V1_0_DECISION.md`

Frozen split:

```text
角色 / Character
→ evolving current Character Sheet
→ “现在的我是谁”
→ current state only by default

重要经历 / Important Experiences
→ protagonist-centered milestone history
→ “我是怎样走到现在的”

行囊 / Inventory
→ owns starting/current possessions in player IA
→ starting possessions do not belong in Character Surface
```

Character may include current origin/background, current social identity/role, personality/values/principles, non-numeric capabilities, long-term limitations/traits and long-term goals/self-direction.

Long-term goals belong to Character; current unresolved commitments/tasks belong to future `事务`.

Character Surface completion will end the MW-011 transitional use of biography/profile in the left Player Status Host. If there is no legitimate portrait/mechanic contribution at that point, left may collapse/narrow instead of duplicating biography.

## 4. Model-driven information curation — FROZEN

Canonical authority:

`architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md`

Frozen principle:

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

The model decides:

- what happened semantically;
- what is important;
- whether current Character information changes;
- whether an event is an Important Experience;
- whether accepted Player/Narrative context already expresses a meaningful protagonist decision;
- which enabled information Surface should receive a structured update;
- how the player-facing summary should read.

Program must not implement a parallel semantic judge through keyword/regex rules, importance score tables, per-event semantic branches, protagonist-choice evidence heuristics or mechanical long-term thresholds.

Program responsibilities are machine-level:

```text
current Game/current Timeline binding
accepted Turn/version binding
stable identity
bounded payload syntax/type/size normalization
atomic persistence
idempotent replay
Save / Restore / Regenerate currentness
stale-future isolation
crash / retry correctness
player-safe serialization/projection
```

Owner explicitly accepts an additional bounded model call when it improves semantic quality and reduces Runtime complexity.

## 5. Current implementation gap

Current Runtime already has durable accepted-turn world semantics, Knowledge provenance, stable actor materialization, Save/Restore/Timeline currentness, frozen Player Character `source_projection.player_profile`, and the MW-011 fail-closed profile projection.

It does **not** yet have the required model-curated durable/current shape for:

```text
current lived Player Character information
protagonist Important Experiences history
```

The existing MW-011 projector reads only frozen Game-local `player_profile`; it cannot satisfy an evolving Character Sheet.

## 6. ACTIVE — MW-014

Executable task:

`my-world/docs/tasks/MW-014_MODEL_DRIVEN_CHARACTER_AND_MILESTONE_CURATION_V0_1_TASK.md`

Identity:

```text
Work Item: MW-014
Name: Model-driven Character + Important Experiences Curation v0.1
Primary Implementer: Codex
Reviewer: GPT
Revision: 1
Review-Round: 0
Status: READY FOR CODEX
Branch: mw-014-model-driven-information-curator-v01
Worktree: D:/AI/Projects/.worktrees/my-world/mw-014
Return ceiling: READY FOR INDEPENDENT REVIEW
```

Required backend vertical:

```text
accepted Player input + accepted GM Narrative
+ bounded current protagonist information
+ bounded recent Important Experiences
↓
Post-turn Information Curator model call
↓
bounded structured curation result
↓
Program normalization + durable currentness
↓
current Character material
+ protagonist milestone material
↓
player-safe Character + Important Experiences projections
```

Curator is non-blocking background semantic maintenance. Curation failure must not invalidate accepted Narrative or gate the next Player action by default.

MW-014 does not implement the final Godot Character/Important Experiences UI.

## 7. After MW-014

After Codex returns a clean pushed candidate:

```text
GPT Independent Review
→ Engineering PASS required
→ integrate exact reviewed candidate
→ shape/authorize KimiCode UI consumer
```

Expected next UI work will implement:

- right-side Character Surface;
- right-side Important Experiences Surface;
- migration of transitional biography/profile out of left Player Status Host;
- left empty/collapsed behavior when no portrait/mechanic contribution.

Do not pre-authorize this UI task until MW-014 produces a stable reviewed projection seam.

## 8. Visual Runtime disposition

Runtime Asset Resolution / portrait / scene / authored-map implementation remains deferred until real authored first-party visual demand exists.

Character portrait is a legitimate future Player Status Host consumer, but no media resolver is built merely to fill the slot.

## 9. MW-013 disposition

```text
MW-013 Internal Declarative UI Host v0.1
= HOLD / NOT AUTHORIZED TO IMPLEMENT YET
```

Re-evaluate only after multiple real Surfaces / mechanic consumers expose repeated component patterns.

## 10. Agent routing

```text
GPT
→ product semantics / architecture / Task Shaping / assignment / Independent Review

Codex
→ high-complexity / architecture-critical / Runtime/authority implementation

KimiCode
→ bounded UI / interaction / ordinary Surface consumers after stable seams exist

Owner
→ Product UAT / explicit product verdict
```

## 11. Immediate route

```text
MW-014 Codex implementation
→ GPT Independent Review
→ integrate only after Engineering PASS
→ shape/authorize KimiCode Character + Important Experiences UI consumer
→ GPT Independent Review
→ Owner UAT
→ People Surface / mechanic-state consumer / next grounded Surfaces
→ only later re-evaluate MW-013
```
