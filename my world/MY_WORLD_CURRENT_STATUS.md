---
title: my world｜当前状态
status: current-project-status
version: 16.6
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

Focused semantic/domain audit:

`architecture/ui/G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_SEMANTIC_AUDIT_DRAFT_V0_1.md`

New frozen authority:

`architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md`

Current shell direction:

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

Only grounded surfaces proceed. A Surface requires a real player question, real data ownership, player-safe projection and non-trivial player value. No fake RPG state or empty tabs.

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

Neither UI Surface becomes a second truth source.

## 5. Model-driven information curation — FROZEN OWNER DECISION

Owner explicitly rejected Program-heavy semantic judging for information surfaces.

Canonical authority:

`architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md`

Frozen principle:

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

Therefore the model, not Program heuristics, decides:

- what happened semantically;
- what is important;
- whether Character current state should change;
- whether an event belongs in Important Experiences;
- which enabled information Surface should receive a structured update;
- whether accepted Player/Narrative context already expresses a meaningful protagonist decision;
- how to summarize the result for the player.

Program must not build a parallel semantic judge from keyword rules, score tables, regexes, event-type branches or protagonist-choice evidence heuristics.

Program responsibilities remain structural/infrastructure only:

```text
accepted current-turn/current-timeline binding
stable identity
payload syntax/type/size normalization
atomic persistence
idempotent replay
Save / Restore / Regenerate currentness
stale-future isolation
crash / retry correctness
canonical storage
presentation serialization / layout
```

Existing Domain wins remains a data-organization instruction to the model, not a reason to create a Program semantic router.

Player ownership of new meaningful protagonist choices remains a product principle, but the model is trusted to infer whether the accepted interaction actually expressed such a choice. Program does not implement a semantic evidence gate.

Owner explicitly accepts an additional bounded model call when it improves curation quality and reduces Runtime complexity.

Recommended direction:

```text
accepted Player input + accepted GM Narrative + current relevant state
→ one post-turn Information Curator model call
→ bounded structured curation result
→ Program normalization + durable commit
→ player-safe projection
```

Information Curator should be non-blocking by default: curation failure must not invalidate accepted Narrative or gate the next Player action.

## 6. Character evolution boundary

Current baseline:

```text
objective durable Character facts
→ model may recognize and curate them from accepted game context

non-voluntary long-term impacts
→ model may recognize them; dedicated Domain is used when one actually exists

major protagonist self-definition
→ model decides whether accepted Player/Narrative context truly establishes the choice
→ no Program semantic evidence gate

short-term state
→ model normally keeps it out of Character Sheet
```

Focused Character sections remain:

```text
basic identity
origin/background
current social identity / role
personality / values / principles
long-term non-numeric capabilities
long-term limitations / traits
long-term goals / self-direction
```

## 7. Important Experiences boundary

`重要经历` is not full World Timeline, every Turn/transcript log, current open tasks, or a second biography database.

The model decides contextual milestone significance. Examples such as identity changes, major capability gains/losses, long-term direction changes, trajectory-changing successes/failures and life-scale relationship events are guidance, not Runtime rules.

Ordinary-looking events are allowed to become milestones when their actual context/impact makes them important.

Restore to before a milestone must remove both the current milestone projection and any Character state that only exists in the restored-away future.

## 8. Current implementation audit

Current Runtime already has:

- durable accepted-turn world semantic consequences;
- Knowledge provenance;
- stable actor identity/materialization;
- Save / Restore / Timeline currentness;
- frozen Player Character `source_projection.player_profile`;
- fail-closed MW-011 profile projection.

But there is not yet a dedicated model-curated durable/player-safe shape for:

```text
current lived Player Character semantic state
protagonist milestone history
```

Current world semantic turns are bounded generic change strings, not a current Character information owner. Current MW-011 profile projector intentionally reads only frozen Game-local `player_profile`; it cannot satisfy an evolving Character Sheet by itself.

## 9. Likely implementation seam after Product Freeze

Current likely split:

```text
Codex
→ post-turn Information Curator model seam
→ bounded structured curation contract
→ normalized durable Character current-state material
→ normalized durable protagonist milestone material
→ Save/Restore/Regenerate/currentness
→ player-safe Character + Important Experiences projections

KimiCode
→ right-side Character Surface
→ right-side Important Experiences Surface
→ move transitional biography/profile out of left
→ left empty/collapsed behavior when no portrait/mechanic contribution
```

Do not implement a Character importance classifier, milestone score engine, keyword router or Program protagonist-choice semantic gate.

No implementation task is authorized yet. Product semantics must finish freezing first.

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
Owner + GPT finish remaining Character + Important Experiences product points
→ freeze semantic/domain audit under model-driven curation authority
→ shape executable backend/UI seam
→ Codex and/or KimiCode implementation
→ GPT Independent Review
→ Owner UAT
→ People Surface / mechanic-state consumer / next grounded surfaces
→ only then re-evaluate MW-013
```
