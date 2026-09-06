---
title: my world｜当前状态
status: current-project-status
version: 16.9
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-015 Character + Important Experiences Surfaces v0.1
current_owner: KimiCode implementation lane
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
MW-014 Model-driven Information Curation    ENGINEERING PASS / INTEGRATED
MW-015 Character + Important Experiences UI READY FOR KIMICODE
MW-013 Internal Declarative UI Host v0.1    HOLD / NOT AUTHORIZED YET
```

## 2. Current architecture direction

Canonical architecture map has been aligned to G6 v3.1:

`MY_WORLD_架构_CURRENT.md`

Session shell:

```text
Player Status Host
→ portrait + live mechanics/status HUD only
→ no “who am I” biography/profile ownership
→ may collapse/hide when no legitimate content exists

Narrative Host
→ GM Narrative + Player natural-language action
→ primary visual/interaction surface

World Information Host
→ active player information surfaces
```

Current mother taxonomy:

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

Only grounded Surfaces appear. Current grounded right-side consumer set for MW-015 is:

```text
概览 / 角色 / 重要经历 / 存档
```

## 3. Character + Important Experiences — FROZEN

Canonical authority:

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

Character includes current origin/background, current social identity/role, personality/values/principles, non-numeric capabilities, long-term limitations/traits and long-term goals/self-direction.

Long-term goals belong to Character; current unresolved commitments/tasks belong to future `事务`.

Character Surface completion ends the MW-011 transitional use of biography/profile in the left Player Status Host. Current v0.1 has no grounded portrait/mechanic contribution, so the left Host may collapse/hide instead of duplicating biography or fake status.

## 4. Model-driven information curation — FROZEN

Canonical authority:

`architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md`

Frozen principle:

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

The model decides semantic meaning, importance, Character evolution, milestone selection and player-facing summarization. Program must not build a parallel semantic judge through keyword/regex rules, importance scores, event-type trees, protagonist-choice evidence heuristics or mechanical long-term thresholds.

MW-015 is a pure presentation consumer over the reviewed MW-014 seam; it must not re-interpret Narrative semantics locally.

## 5. MW-014 — ENGINEERING PASS / INTEGRATED

Task:

`my-world/docs/tasks/MW-014_MODEL_DRIVEN_CHARACTER_AND_MILESTONE_CURATION_V0_1_TASK.md`

Independent Review:

`my-world/docs/mw014/MW-014_INDEPENDENT_REVIEW_IR1.md`

Integration verification:

`my-world/docs/mw014/MW-014_INTEGRATION_VERIFICATION.md`

Reviewed candidate:

```text
8a3b64d0747ebf9c987c32af93e804672a8cbbe3
```

Integrated review lineage:

```text
048b2a76ff8238a7e9025da268a24ae924281856
```

Current implementation main after integration documentation before MW-015 shaping was:

```text
7a0eeb35f8f836fa6df98288bef98a8f535b8393
```

Reviewed player-safe L3 seam for the UI consumer:

`src/信息整理/L3_外交层/角色经历投影公开接口.gd`

It exposes current Character + Important Experiences without a render-time Provider call or raw/private Runtime data.

## 6. G6 root/supporting IA drift — ALIGNED

Before MW-015 shaping, stale supporting prose that still assigned “who am I” to the left Host or omitted `重要经历` was aligned.

Updated current supporting authorities include:

```text
MY_WORLD_架构_CURRENT.md v3.1
architecture/ui/声明式UIHost设计.md v1.4
```

Current meaning is now consistent:

```text
left  = portrait + live status only
right = current Character / Important Experiences / other grounded information
MW-013 remains HOLD
```

## 7. ACTIVE — MW-015

Executable task:

`my-world/docs/tasks/MW-015_CHARACTER_AND_IMPORTANT_EXPERIENCES_UI_V0_1_TASK.md`

Identity:

```text
Work Item: MW-015
Name: Character + Important Experiences Surfaces v0.1
Primary Implementer: KimiCode
Reviewer: GPT
Revision: 1
Review-Round: 0
Status: READY FOR KIMICODE
Branch: mw-015-character-important-experiences-ui-v01
Worktree: D:/AI/Projects/.worktrees/my-world/mw-015
Return ceiling: READY FOR INDEPENDENT REVIEW
```

Required product vertical:

```text
MW-014 player-safe projection
↓
right-side 概览 | 角色 | 重要经历 | 存档
↓
current Character Sheet + milestone history
↓
MW-011 biography/profile leaves the left Host
↓
empty current Player Status Host collapses/hides
```

Key scope constraints:

- no raw `world_state` / Narrative semantic inference in UI;
- no Program semantic classifier;
- no Inventory / People / Thread / Map / System fake tabs;
- no portrait resolver / fake HP or attributes;
- no MW-013 Declarative Host;
- Save ownership/callbacks remain G3-owned;
- Curator success should refresh the visible surfaces without reopening.

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
MW-015 KimiCode implementation
→ GPT Independent Review
→ integrate only after Engineering PASS
→ Owner UAT
→ People Surface / mechanic-state consumer / next grounded Surfaces
→ repeated patterns
→ only later re-evaluate MW-013
```
