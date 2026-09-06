---
title: my world｜当前状态
status: current-project-status
version: 16.11
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-015 Owner UAT — Character + Important Experiences Surfaces v0.1
current_owner: Owner product UAT lane
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
MW-011 RPG Host / Player Profile outcome    PRODUCT PASS / CLOSED
MW-012 Zhang Chen Player Character Card     ENGINEERING PASS / INTEGRATED / PRODUCT INGRESS ACCEPTED
G6 Visual Runtime re-entry                  AUDITED — IMPLEMENTATION DEFERRED
G6 Character + Important Experiences        SEMANTIC / IA FROZEN
MW-014 Model-driven Information Curation    ENGINEERING PASS / INTEGRATED
MW-015 Character + Important Experiences UI ENGINEERING PASS / INTEGRATED / OWNER UAT PENDING
MW-013 Internal Declarative UI Host v0.1    HOLD / NOT AUTHORIZED YET
```

## 2. Frozen G6 information architecture

```text
Player Status Host
→ portrait + live mechanics/status HUD only
→ no biography/profile ownership
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

Only grounded Surfaces appear. Current implemented right-side set is:

```text
概览 / 角色 / 重要经历 / 存档
```

## 3. Character + Important Experiences — FROZEN

Canonical authority:

- `architecture/ui/G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_V1_0_DECISION.md`
- `architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md`

```text
角色 / Character
→ evolving current Character Sheet
→ “现在的我是谁”
→ current state, not mutation log

重要经历 / Important Experiences
→ protagonist-centered milestone history
→ “我是怎样走到现在的”

行囊 / Inventory
→ owns starting/current possessions in player IA
→ starting possessions do not belong in Character Surface
```

Frozen curation principle:

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

Program must not add keyword/regex semantic routing, importance scores, event-type rule trees or protagonist-choice heuristics.

## 4. MW-014 — ENGINEERING PASS / INTEGRATED

Reviewed backend seam:

`src/信息整理/L3_外交层/角色经历投影公开接口.gd`

It exposes presentation-safe current Character + Important Experiences and requires no Provider call merely to render/reopen.

Formal records:

- `my-world/docs/mw014/MW-014_INDEPENDENT_REVIEW_IR1.md`
- `my-world/docs/mw014/MW-014_INTEGRATION_VERIFICATION.md`

## 5. MW-015 — ENGINEERING PASS / INTEGRATED / OWNER UAT PENDING

Task:

`my-world/docs/tasks/MW-015_CHARACTER_AND_IMPORTANT_EXPERIENCES_UI_V0_1_TASK.md`

Reviewed implementation candidate:

```text
3387cba213a2680b08f78b07a63ee0ecee5417ce
```

Independent Review record commit:

```text
0e7aed125f9f4d46d8ca86070d11b80f130ed213
```

Integration merge:

```text
967a856e02b761576cc4dcb773a693530dcf2fc9
```

Post-integration verification doc:

`my-world/docs/mw015/MW-015_INTEGRATION_VERIFICATION.md`

Engineering outcome:

```text
right information navigation
→ 概览 | 角色 | 重要经历 | 存档

角色
→ renders current MW-014 Character projection

重要经历
→ renders current ordered MW-014 milestone projection

left Player Status Host
→ transitional biography/profile/world/recent-actions/turn-count removed
→ currently collapses/hides because no real portrait/mechanic contribution exists
```

Engineering evidence reviewed by GPT includes the real-shell focused suite, MW-014/G3/G5/MW-009/MW-011/MW-012 regressions, responsive checks, Windows export and zero render-time Provider calls.

This is **not Product PASS** until Owner accepts the real app experience.

## 6. Owner UAT target

Owner should verify in the real application:

```text
open Game
→ right panel is 信息
→ tabs are 概览 / 角色 / 重要经历 / 存档
→ 角色 contains current protagonist information
→ left biography filler is gone and no fake status appears
→ play a meaningful turn
→ curator result becomes visible without reopening
→ 重要经历 reflects a meaningful milestone when the model judges one
→ Restore / Regenerate returns Character / Important Experiences to matching current history
```

Owner verdict options:

```text
PASS
NOT PASS — with concrete product/UAT findings
```

## 7. Agent routing — OWNER UPDATE

Canonical authority:

`AGENT_EXECUTION_ROUTING_CURRENT.md v3.0`

MW-015 was the final already-authorized KimiCode round.

After MW-015:

```text
GPT
→ product semantics / architecture / Task Shaping / dispatch / Independent Review

Codex
→ sole default implementation agent for all new production tasks

Owner
→ Product UAT / explicit product verdict
```

KimiCode / Zcode / other implementation agents require explicit future Owner re-authorization.

## 8. Visual Runtime / MW-013 disposition

```text
Runtime Asset Resolution / portrait / scene / authored-map
→ DEFERRED until real authored visual demand exists

MW-013 Internal Declarative UI Host v0.1
→ HOLD / NOT AUTHORIZED YET
```

Do not re-enter MW-013 until multiple grounded real Surfaces / mechanic consumers expose repeated patterns.

## 9. Immediate route

```text
Owner UAT — MW-015
→ if PASS: mark MW-015 PRODUCT PASS / CLOSED
→ choose next grounded G6 outcome
→ all new implementation tasks default to Codex

if NOT PASS:
→ GPT root-cause / scope classification
→ same MW-015 revision lineage for same-outcome defects
→ Codex implements the required correction
→ GPT Independent Review
→ Owner UAT again
```
