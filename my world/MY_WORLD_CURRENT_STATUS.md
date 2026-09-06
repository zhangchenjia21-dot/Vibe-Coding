---
title: my world｜当前状态
status: current-project-status
version: 16.12
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-015 R2 — Character Information Preservation Correction
current_owner: Codex implementation lane
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
MW-015 R1 Character + Important Exp UI       ENGINEERING PASS / INTEGRATED / OWNER UAT NOT PASS
MW-015 R2 Character Information Preservation READY FOR CODEX
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

MW-014 R1 explicitly requires the frozen starting `player_profile` to remain visible/useful before lived curator updates.

Formal records:

- `my-world/docs/mw014/MW-014_INDEPENDENT_REVIEW_IR1.md`
- `my-world/docs/mw014/MW-014_INTEGRATION_VERIFICATION.md`

## 5. MW-015 R1 — OWNER UAT NOT PASS

R1 implementation and integration remain valid engineering evidence for the shell/navigation migration:

```text
right information navigation
→ 概览 | 角色 | 重要经历 | 存档

left Player Status Host
→ biography/profile/world/recent-actions/turn-count removed
→ collapses/hides when no real portrait/mechanic contribution exists
```

Owner UAT on 2026-09-06 found a material product regression in `角色`:

```text
角色
→ only thin headline + summary visible
→ previously accepted rich Character information is missing
```

This is **NOT PASS**.

The left Host collapse itself is not rejected.

Formal Owner record:

`my-world/docs/mw015/MW-015_OWNER_UAT_R1_RESULT.md`

Root-cause classification:

```text
MW-011 Owner PASS
→ rich starting Character information had proven product value

MW-014 contract
→ frozen starting player_profile must remain visible/useful before lived curation

MW-015 R1 shaping
→ intentionally reduced initial Character to headline/summary to avoid Inventory leakage
→ over-pruned legitimate Character material
→ product-value regression
```

## 6. ACTIVE — MW-015 R2

Task:

`my-world/docs/tasks/MW-015_R2_CHARACTER_INFORMATION_PRESERVATION_TASK.md`

Identity:

```text
Work Item: MW-015
Revision: 2
Primary Implementer: Codex
Reviewer: GPT
Status: READY FOR CODEX
Branch: mw-015-r2-character-information-preservation
Worktree: D:/AI/Projects/.worktrees/my-world/mw-015-r2
Return ceiling: READY FOR INDEPENDENT REVIEW
```

Required product correction:

```text
left Player Status Host
→ remains collapsed/hidden while no legitimate status content exists

right 角色 Surface
→ materially rich Character Sheet immediately
→ preserves legitimate frozen Game-local starting Character material
→ excludes starting possessions / other-domain material
→ evolves after successful model curation
```

Character-owned baseline includes, when present:

```text
identity/basic profile
origin/background
current social identity/role
personality/values/principles
non-numeric capabilities
limitations/long-term traits
long-term goals/self-direction
```

Do not repair with title keywords, regexes, Zhang-Chen special cases or other Program semantic classifiers.

If the frozen Game-local representation cannot distinguish Character-owned material from Inventory/other domains deterministically, Codex must STOP with an architecture finding rather than invent heuristics.

Existing Games must regain the rich Character view when the required frozen material already exists in that Game; no Source-current backfill is allowed.

## 7. Agent routing

Canonical authority:

`AGENT_EXECUTION_ROUTING_CURRENT.md v3.1`

```text
GPT
→ product semantics / architecture / Task Shaping / dispatch / Independent Review

Codex
→ sole default implementation agent for new production implementation

Owner
→ Product UAT / explicit product verdict
```

KimiCode / Zcode / other implementation agents require explicit future Owner re-authorization.

## 8. Owner-build handoff

For every product-facing outcome after Engineering PASS + integration:

```text
sync D:/AI/Projects/my-world safely to exact integrated main
→ validate exact local HEAD
→ run run-game.ps1 -ValidateExportOnly
→ Owner Launch Ready
→ Owner UAT
```

Never present an unreviewed task branch as the Owner build.

## 9. Visual Runtime / MW-013 disposition

```text
Runtime Asset Resolution / portrait / scene / authored-map
→ DEFERRED until real authored visual demand exists

MW-013 Internal Declarative UI Host v0.1
→ HOLD / NOT AUTHORIZED YET
```

## 10. Immediate route

```text
MW-015 R2 Codex implementation
→ GPT Independent Review
→ integrate only after Engineering PASS
→ canonical local checkout sync + fresh export
→ Owner UAT again

if Owner UAT PASS:
→ MW-015 PRODUCT PASS / CLOSED
→ choose next grounded G6 outcome

if NOT PASS:
→ same MW-015 lineage if outcome remains unchanged
→ root-cause / architecture classification
```
