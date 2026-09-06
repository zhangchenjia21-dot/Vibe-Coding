---
title: my world｜当前状态
status: current-project-status
version: 16.4
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: G6 Character + Important Experiences Semantic / Domain Audit
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
G6 Surface / Information Architecture       ACTIVE — CHARACTER + IMPORTANT EXPERIENCES AUDIT
MW-013 Internal Declarative UI Host v0.1    HOLD / NOT AUTHORIZED YET
```

## 2. Current discussion authorities

Primary IA draft:

`architecture/ui/G6_SURFACE_INFORMATION_ARCHITECTURE_DRAFT_V0_1.md`

Current version: **v0.4 / DRAFT / FOR OWNER DISCUSSION**.

Focused semantic/domain audit:

`architecture/ui/G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_SEMANTIC_AUDIT_DRAFT_V0_1.md`

Current version: **v0.1 / DRAFT / FOR OWNER DISCUSSION**.

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
→ current state only by default, not mutation history

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
→ meaningful milestone/change history
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

Focused audit now proposes Character sections around:

```text
basic identity
origin/background
current social identity / role
personality / values / principles
long-term non-numeric capabilities
long-term limitations / traits
long-term goals / self-direction
```

## 6. Important Experiences boundary under discussion

`重要经历` is not:

- full world Timeline;
- every Turn / transcript log;
- current open tasks;
- a second biography database.

Draft milestone threshold includes only life-scale changes such as:

- identity/social-role changes;
- major capability gains/losses;
- Player-authorized long-term direction changes;
- trajectory-changing successes/failures;
- life-scale relationship events.

Ordinary battles, ordinary conversations, normal checks, small purchases, short-term injuries and routine task completion do not automatically become milestones.

Long-term target:

```text
authoritative lived history / Character semantic change / relevant formal Domain event
→ protagonist milestone selection/materialization
→ player-safe Important Experiences projection
```

Restore to before a milestone must remove both the milestone projection and any current Character change caused by that future.

## 7. Current implementation audit

Current Runtime already has:

- durable accepted-turn world semantic consequences;
- Knowledge provenance;
- stable actor identity/materialization;
- Save / Restore / Timeline currentness;
- frozen Player Character `source_projection.player_profile`;
- fail-closed MW-011 profile projection.

But there is not yet a dedicated authoritative/player-safe shape for:

```text
current lived Player Character semantic facets
current social identity / role
long-term capability / limitation evolution
Player-authorized long-term goal / principle evolution
protagonist milestone history
```

Current world semantic turns are bounded generic change strings, not a safe targeted Character owner. Current MW-011 profile projector intentionally reads only frozen Game-local `player_profile`; it cannot satisfy an evolving Character Sheet by itself.

## 8. Likely implementation seam after Product Freeze

Current likely split:

```text
Codex
→ minimal game-local Player Character semantic authority
→ protagonist milestone authority/materialization
→ Player-authorization boundary
→ Save/Restore/Regenerate/currentness
→ player-safe Character + Important Experiences projections

KimiCode
→ right-side Character Surface
→ right-side Important Experiences Surface
→ move transitional biography/profile out of left
→ left empty/collapsed behavior when no portrait/mechanic contribution
```

No implementation task is authorized yet. Product semantics must freeze first.

## 9. Visual Runtime disposition

Runtime Asset Resolution / portrait / scene / authored-map implementation remains deferred until a real authored first-party visual demand exists.

Character portrait is a valid future Player Status Host consumer, but no media resolver is built merely to fill the slot.

## 10. MW-013 disposition

```text
MW-013 Internal Declarative UI Host v0.1
= HOLD / NOT AUTHORIZED TO IMPLEMENT YET
```

Re-evaluate only after multiple real Surfaces / mechanic consumers expose repeated component patterns.

## 11. Agent routing — current Owner rule

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

## 12. Immediate route

```text
Owner + GPT finish Character + Important Experiences semantics
→ confirm long-term-goal vs 事务 boundary
→ confirm milestone threshold + growth behavior
→ decide left-column behavior after Character Surface exists
→ decide placement of current known facts
→ freeze semantic/domain audit
→ shape executable backend/UI seam
→ Codex and/or KimiCode implementation
→ GPT Independent Review
→ Owner UAT
→ People Surface / mechanic-state consumer / next grounded surfaces
→ only then re-evaluate MW-013
```