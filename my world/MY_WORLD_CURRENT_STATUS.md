---
title: my world｜当前状态
status: current-project-status
version: 16.5
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

Current version: **v0.2 / DRAFT / FOR OWNER DISCUSSION**.

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

## 5. Model-driven information curation — Owner direction

Owner explicitly rejected a Program-heavy significance classifier.

Current principle:

> **Model judges semantic importance; Program enforces authority, safety and durability.**

Meaning:

```text
Model
→ decides whether a change is semantically important
→ decides whether it belongs in Character / Important Experiences / future information surfaces
→ decides whether current Character material should add / replace / remove / remain unchanged
→ writes concise player-facing summary text

Program
→ does NOT decide importance through keyword rules / regex / score tables / fixed turn thresholds
→ validates accepted turn/hash linkage
→ validates allowed target + payload shape/size
→ enforces Existing Domain wins
→ enforces Player-owned self-definition evidence
→ enforces player-safe disclosure
→ commits idempotently
→ preserves Save / Restore / Regenerate currentness and persistence integrity
```

Owner explicitly accepts an additional bounded model call when that improves semantic accuracy, consistency and reduces Runtime complexity.

Preferred shape under discussion:

```text
accepted Player input + accepted GM Narrative
+ current player-safe Character state
+ bounded recent milestones
+ relevant formal Domain facts / authority hints
→ model-driven post-turn Information Curator
→ bounded semantic proposals
→ narrow Program validation + durable commit
```

This curator is semantic maintenance, not rendering. It must not turn Narrative delivery into a hard wait/gate; malformed/failed curation fails soft and can be retried without invalidating the accepted Narrative.

## 6. Character evolution boundary under discussion

Current baseline:

```text
objective durable Character facts
→ model may identify from world causality

non-voluntary long-term impacts
→ model may identify; Existing Domain wins if one exists

major protagonist self-definition
→ model may propose only with Player-originated / Player-authorized evidence

short-term state
→ model should leave outside Character Sheet
```

These are authority/product rules for the model and validator, not a request for a giant Program rule engine.

Existing Domain wins remains protected for Inventory / Relationship / Knowledge / Injury / Faction / Timeline / Thread / Mechanic State.

Focused audit proposes Character sections around:

```text
basic identity
origin/background
current social identity / role
personality / values / principles
long-term non-numeric capabilities
long-term limitations / traits
long-term goals / self-direction
```

## 7. Important Experiences boundary under discussion

`重要经历` is not:

- full world Timeline;
- every Turn / transcript log;
- current open tasks;
- a second biography database.

Milestone examples include life-scale identity, capability, direction, trajectory and relationship changes, but examples are guidance for model semantic judgment — not Runtime keyword lists or scoring rules.

Current target:

```text
authoritative lived history / Character semantic change / relevant formal Domain event
→ model-driven protagonist milestone curation
→ bounded durable/current milestone material
→ player-safe Important Experiences projection
```

Restore to before a milestone must remove both the milestone projection and any current Character change caused by that future.

## 8. Current implementation audit

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

## 9. Likely implementation seam after Product Freeze

Current likely split:

```text
Codex
→ model-driven Character / milestone curator seam
→ minimal game-local Player Character semantic authority
→ Player-authorization evidence validation
→ Save/Restore/Regenerate/currentness
→ player-safe Character + Important Experiences projections

KimiCode
→ right-side Character Surface
→ right-side Important Experiences Surface
→ move transitional biography/profile out of left
→ left empty/collapsed behavior when no portrait/mechanic contribution
```

No implementation task is authorized yet. Product semantics must freeze first.

## 10. Visual Runtime disposition

Runtime Asset Resolution / portrait / scene / authored-map implementation remains deferred until a real authored first-party visual demand exists.

Character portrait is a valid future Player Status Host consumer, but no media resolver is built merely to fill the slot.

## 11. MW-013 disposition

```text
MW-013 Internal Declarative UI Host v0.1
= HOLD / NOT AUTHORIZED TO IMPLEMENT YET
```

Re-evaluate only after multiple real Surfaces / mechanic consumers expose repeated component patterns.

## 12. Agent routing — current Owner rule

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

## 13. Immediate route

```text
Owner + GPT finish Character + Important Experiences semantics
→ confirm model-driven Information Curator as formal architecture rule
→ freeze semantic/domain audit
→ shape executable backend/UI seam
→ Codex and/or KimiCode implementation
→ GPT Independent Review
→ Owner UAT
→ People Surface / mechanic-state consumer / next grounded surfaces
→ only then re-evaluate MW-013
```