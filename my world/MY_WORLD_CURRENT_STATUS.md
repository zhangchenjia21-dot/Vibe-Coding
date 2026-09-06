---
title: my world｜当前状态
status: current-project-status
version: 16.3
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: G6 Character + Important Experiences Surface / Information Architecture Discussion
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

## 2. Current discussion authority

`architecture/ui/G6_SURFACE_INFORMATION_ARCHITECTURE_DRAFT_V0_1.md`

Current version: **v0.4 / DRAFT / FOR OWNER DISCUSSION**.

Current frozen shell direction:

```text
Player Status Host
→ portrait + live mechanics/status HUD only
→ no “who am I” biography/profile ownership

Narrative Host
→ GM Narrative + Player natural-language action

World Information Host
→ player-active information surfaces
```

## 3. Current right-side mother taxonomy under discussion

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

Only grounded surfaces proceed. A Surface requires:

```text
real player question
+ real domain owner
+ player-safe projection
+ non-trivial product value
```

No fake RPG state or empty tabs.

## 4. Owner decisions recorded in G6 IA

Owner has explicitly established:

```text
角色 / Character
→ evolving current Character Sheet
→ answers “现在的我是谁”
→ not a static opening Character Card viewer

重要经历 / Important Experiences
→ separate top-level Surface
→ chronological protagonist-centered milestone history
→ answers “我是怎样走到现在的”

行囊 / Inventory
→ owns starting/current possessions in player IA
→ starting possessions do not belong in Character Surface
```

Character and Important Experiences may project the same lived change differently:

```text
Character
→ current result

Important Experiences
→ the meaningful change/milestone that produced it
```

Neither UI Surface becomes canonical truth owner.

## 5. Character evolution boundary under discussion

Current baseline:

```text
objective durable Character facts
→ may be established by world causality

non-voluntary long-term impacts
→ may be durable; Existing Domain wins if one exists

major protagonist self-definition
→ requires Player-originated / Player-authorized evidence

short-term state
→ does not enter Character Sheet
```

Existing Domain wins remains protected for Inventory / Relationship / Knowledge / Injury / Faction / Timeline / Thread / Mechanic State.

## 6. Important Experiences boundary under discussion

`重要经历` is not:

- full world Timeline;
- every Turn / transcript log;
- current open tasks;
- a second biography database.

Long-term target:

```text
authoritative lived history / Character semantic change / relevant formal Domain event
→ bounded protagonist milestone projection/materialization
→ player-safe Important Experiences Surface
```

Restore to before a milestone must remove both the milestone projection and any current Character change caused by that future.

## 7. Visual Runtime disposition

Runtime Asset Resolution / portrait / scene / authored-map implementation remains deferred until a real authored first-party visual demand exists.

Character portrait is a valid future Player Status Host consumer, but no media resolver is built merely to fill the slot.

## 8. MW-013 disposition

```text
MW-013 Internal Declarative UI Host v0.1
= HOLD / NOT AUTHORIZED TO IMPLEMENT YET
```

Re-evaluate only after multiple real Surfaces / mechanic consumers expose repeated component patterns.

## 9. Agent routing — current Owner rule

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

## 10. Immediate route

```text
Owner + GPT finish Character + Important Experiences semantics
→ freeze milestone threshold + Character/current-vs-history boundary
→ audit minimal game-local authority/projection needed
→ shape executable backend/UI seam
→ assign Codex and/or KimiCode
→ GPT Independent Review
→ Owner UAT
→ People Surface / mechanic-state consumer / next grounded surfaces
→ only then re-evaluate MW-013
```
