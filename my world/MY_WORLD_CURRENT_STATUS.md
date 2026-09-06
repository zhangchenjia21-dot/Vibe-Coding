---
title: my world｜当前状态
status: current-project-status
version: 16.14
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-015 R2 Owner UAT build handoff — Model-driven Initial Character Curation
current_owner: Codex local UAT-build preparation lane
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
MW-015 R1 Character + Important Exp UI      ENGINEERING PASS / INTEGRATED / OWNER UAT NOT PASS
MW-015 R2 Initial Character Curation        ENGINEERING PASS / INTEGRATED / OWNER UAT PENDING
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
- `architecture/ui/G6_INITIAL_CHARACTER_CURATION_BASELINE_V1_0_DECISION.md`

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

MW-014 established lived-turn curation; MW-015 R2 adds the distinct Game/T0 initial Character baseline under the same information-c​​uration owner.

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

Owner UAT found a material product regression in `角色`:

```text
角色
→ only thin headline + summary visible
→ previously accepted rich Character information is missing
```

The left Host collapse itself is not rejected.

Formal Owner record:

`my-world/docs/mw015/MW-015_OWNER_UAT_R1_RESULT.md`

## 6. MW-015 R2 architecture — FROZEN

Architecture gap evidence:

`cbe0f12c411f046cc17318fd1be856dcd2c13e43`

Canonical decision:

`architecture/ui/G6_INITIAL_CHARACTER_CURATION_BASELINE_V1_0_DECISION.md`

Frozen result:

```text
Initial Character Curation
→ Game/T0-scoped model-curated baseline
→ same information_curation owner
→ optional backward-compatible initial record
→ no synthetic Conversation Turn
→ no opening-success dependency
→ no new SQLite table
→ no Source-current lookup
→ model owns semantic selection
```

Projection order remains:

```text
thin safe fallback
→ valid Initial Character baseline
→ current valid lived-turn Character curation
```

Initial baseline never creates Important Experiences from static T0 biography.

## 7. MW-015 R2 — ENGINEERING PASS / INTEGRATED / OWNER UAT PENDING

Task:

`my-world/docs/tasks/MW-015_R2_CHARACTER_INFORMATION_PRESERVATION_TASK.md`

Reviewed implementation candidate:

`c8618ad9c802d5e0d5c2de5db62e9e88aabdb698`

Independent Review:

`my-world/docs/mw015/r2/MW-015_R2_INDEPENDENT_REVIEW_IR1.md`

IR record commit / reviewed integration tip:

`d191f24ebfd4fdf7b7f13dd16b706c060be129a3`

Post-integration verification:

`my-world/docs/mw015/r2/MW-015_R2_INTEGRATION_VERIFICATION.md`

Integration-verification main commit:

`1e6545353dd4b583a3cc5a6b3881ef18d97d0f09`

Engineering result:

```text
open/activate Game
→ Initial Character curator runs independently of GM opening
→ input = frozen Game-local player-safe starting profile
→ model selects/summarizes Character information
→ successful result becomes durable Game/T0 baseline
→ right 角色 refreshes without requiring a Player-authored Turn
→ left Player Status Host remains hidden when empty

later lived Turn
→ existing MW-014 curator continues evolving current Character
```

Reviewed evidence includes:

- initial Runtime/SQLite lifecycle: 60 checks / 0 failures;
- real Zhang Chen shell/UI vertical with seven Character groups and zero accepted Turns;
- opening failure independence;
- Restore/reopen/Regenerate currentness and displaced-future isolation;
- old `{schema, turns}` compatibility and existing turn-chain preservation;
- MW-014 / MW-011 / MW-012 / MW-009 / G3 / G5 / Narrative regressions;
- Windows Desktop export;
- real Provider smoke.

Real Provider smoke recorded 3 successful rich baseline results and 1 `malformed_response`. The malformed response failed soft and performed zero baseline mutation. This remains an Owner-UAT reliability/latency risk, not an Engineering blocker; do not add Program semantic heuristics to repair model output.

No Product PASS is claimed until Owner accepts the real application.

## 8. Agent routing

Canonical authority:

`AGENT_EXECUTION_ROUTING_CURRENT.md v3.1`

```text
GPT
→ product semantics / architecture / Task Shaping / dispatch / Independent Review

Codex
→ sole default implementation agent for new production implementation and local UAT-build preparation

Owner
→ Product UAT / explicit product verdict
```

KimiCode / Zcode / other implementation agents require explicit future Owner re-authorization.

## 9. Owner-build handoff — CURRENT

Before Owner UAT, the canonical local playable checkout must be prepared:

```text
D:/AI/Projects/my-world
→ inspect branch/status/worktrees
→ safely fetch + fast-forward main to exact origin/main
→ verify exact local HEAD
→ run run-game.ps1 -ValidateExportOnly
→ Owner Launch Ready
```

Current intended integrated GitHub main includes integration verification commit:

`1e6545353dd4b583a3cc5a6b3881ef18d97d0f09`

Never overwrite unknown dirty work or use reset/clean/force to hide divergence.

## 10. Visual Runtime / MW-013 disposition

```text
Runtime Asset Resolution / portrait / scene / authored-map
→ DEFERRED until real authored visual demand exists

MW-013 Internal Declarative UI Host v0.1
→ HOLD / NOT AUTHORIZED YET
```

## 11. Immediate route

```text
Codex local Owner-build preparation
→ verify local HEAD = intended integrated main
→ fresh Windows export validation
→ Owner Launch Ready
→ Owner UAT MW-015 R2

if Owner UAT PASS:
→ MW-015 PRODUCT PASS / CLOSED
→ choose next grounded G6 outcome

if Owner UAT NOT PASS:
→ same MW-015 lineage if outcome remains unchanged
→ GPT root-cause / architecture classification
→ Codex correction
→ GPT Independent Review
→ integration + Owner-build handoff
→ Owner UAT again
```
