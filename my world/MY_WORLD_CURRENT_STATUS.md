---
title: my world｜当前状态
status: current-project-status
version: 16.8
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: G6 Character + Important Experiences UI Consumer Task Shaping
current_owner: GPT product-architecture / dispatcher lane
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

Character includes current identity/profile material such as origin/background, current social identity/role, personality/values/principles, non-numeric capabilities, long-term limitations/traits and long-term goals/self-direction.

Long-term goals belong to Character; current unresolved commitments/tasks belong to future `事务`.

Character Surface completion ends the MW-011 transitional use of biography/profile in the left Player Status Host. If no legitimate portrait/mechanic contribution exists then, left may collapse/narrow instead of duplicating biography.

## 4. Model-driven information curation — FROZEN

Canonical authority:

`architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md`

Frozen principle:

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

The model decides semantic meaning, importance, Character evolution, milestone selection and player-facing summarization. Program must not build a parallel semantic judge through keyword/regex rules, importance scores, event-type trees, protagonist-choice evidence heuristics or mechanical long-term thresholds.

Program responsibilities remain machine-level:

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

## 5. MW-014 — ENGINEERING PASS / INTEGRATED

Task:

`my-world/docs/tasks/MW-014_MODEL_DRIVEN_CHARACTER_AND_MILESTONE_CURATION_V0_1_TASK.md`

Independent Review:

`my-world/docs/mw014/MW-014_INDEPENDENT_REVIEW_IR1.md`

Integration verification:

`my-world/docs/mw014/MW-014_INTEGRATION_VERIFICATION.md`

Reviewed implementation candidate:

```text
8a3b64d0747ebf9c987c32af93e804672a8cbbe3
```

Review-record / integrated lineage:

```text
048b2a76ff8238a7e9025da268a24ae924281856
```

Post-integration documentation advanced `my-world/main` beyond that review record without changing production implementation.

Integrated backend vertical:

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

Engineering evidence includes focused 103/0, required G3/G5/MW-009/MW-011/MW-012 regressions, Windows export PASS, and real Kimi K3 production-path semantic smoke.

Non-blocking semantic observation from the real smoke: one Liu Bei-role scenario retained some strong starting wording about lacking local contacts/identity protection. Treat this as model/prompt curation quality for later UAT/iteration, not a reason to add Program semantic heuristics.

## 6. Current next work

MW-014 has established the stable backend/projection seam. The next grounded consumer is the actual player-facing UI vertical:

```text
right-side Character Surface
+ right-side Important Experiences Surface
+ migrate MW-011 transitional biography/profile out of left Player Status Host
+ left collapse/narrow behavior when no portrait/mechanic contribution exists
```

Before issuing the UI task, GPT should align stale supporting/root UI architecture prose that still says the left Player Host owns “who am I” or omits `重要经历` from the current taxonomy. Owner current decisions and frozen G6 decisions already govern, but supporting docs should no longer contradict them.

Expected implementer: **KimiCode** if the task remains a bounded UI consumer over the reviewed MW-014 projection seam. If shaping discovers a new Runtime/authority requirement, split or route that mechanism to Codex rather than expanding KimiCode scope.

## 7. Visual Runtime disposition

Runtime Asset Resolution / portrait / scene / authored-map implementation remains deferred until real authored first-party visual demand exists.

Character portrait is a legitimate future Player Status Host consumer, but no media resolver is built merely to fill the slot.

## 8. MW-013 disposition

```text
MW-013 Internal Declarative UI Host v0.1
= HOLD / NOT AUTHORIZED TO IMPLEMENT YET
```

Re-evaluate only after multiple real Surfaces / mechanic consumers expose repeated component patterns.

## 9. Agent routing

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

## 10. Immediate route

```text
align stale G6 supporting/root IA prose
→ shape/authorize Character + Important Experiences UI consumer
→ KimiCode implementation
→ GPT Independent Review
→ Owner UAT
→ People Surface / mechanic-state consumer / next grounded Surfaces
→ only later re-evaluate MW-013
```
