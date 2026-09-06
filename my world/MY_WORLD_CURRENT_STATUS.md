---
title: my world｜当前状态
status: current-project-status
version: 16.15
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: G6-D Next Grounded Surface Audit — People
current_owner: GPT + Owner product/architecture discussion lane
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
MW-015 Character + Important Experiences    PRODUCT PASS / CLOSED
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

## 3. Character + Important Experiences — PRODUCT BASELINE ACCEPTED

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

MW-014 established lived-turn curation; MW-015 R2 added the distinct Game/T0 initial Character baseline under the same information-curation owner.

Formal records:

- `my-world/docs/mw014/MW-014_INDEPENDENT_REVIEW_IR1.md`
- `my-world/docs/mw014/MW-014_INTEGRATION_VERIFICATION.md`

## 5. MW-015 — PRODUCT PASS / CLOSED

R1 established the right-side information navigation and migrated biography/profile material away from the left Player Status Host, but Owner UAT found the initial Character Surface too thin.

R2 corrected that product regression through a model-driven Game/T0 Initial Character Baseline independent of opening success.

Final accepted product path:

```text
open/activate Game
→ right 信息 Host available
→ initial model curator uses frozen Game-local player-safe protagonist material
→ 角色 becomes materially useful without requiring a new Player Turn
→ later lived curation continues evolving current Character
→ left Player Status Host remains hidden while it has no real portrait/mechanic contribution
```

Final Owner record:

`my-world/docs/mw015/r2/MW-015_R2_OWNER_UAT_RESULT.md`

Final disposition:

```text
MW-015 R1 Owner UAT                         NOT PASS
MW-015 R2 Engineering / Independent Review  PASS / INTEGRATED
MW-015 R2 Owner UAT                         PRODUCT PASS
MW-015                                      CLOSED
```

Owner accepted the current information architecture / semantic outcome. Visual density, typography, spacing and hierarchy remain a **deferred G6 UI polish** concern and do not reopen MW-015.

## 6. Agent routing

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

## 7. CURRENT — next grounded G6 outcome audit

Roadmap G6-D requires real Surfaces to be pulled by actual player questions and real data owners rather than by traditional RPG completeness.

After Character + Important Experiences, the current strongest next candidate is:

```text
人物 / People
→ “我知道 / 遇见了哪些人？他们现在对我而言是什么样的人？”
```

Current evidence says People has potential product value because G5 already contains persistent actors, actor Knowledge/Agency boundaries and runtime-created actor materialization. However, the current general player-safe projection exposes only player-known facts and does **not yet prove a dedicated actor/relationship-safe read model**.

Therefore current work is **product/architecture audit, not implementation authorization yet**.

Before creating a Codex Task Packet, GPT + Owner must freeze at minimum:

- the exact player question People answers;
- which actors qualify to appear;
- what information is allowed to be shown about each person;
- how `World Truth != actor Knowledge != human-player disclosure` constrains the page;
- whether relationship/attitude is a real current owner or still insufficiently grounded;
- whether one bounded model-curation extension is needed, or existing deterministic safe projections are enough;
- Save / Restore / Regenerate currentness expectations;
- empty/unknown behavior without inventing social state.

Do not implement People merely by dumping all actors from omniscient `world_state`.

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
People Surface product/architecture audit
→ Owner discussion / semantic freeze
→ determine whether current Runtime already has enough safe actor/relationship material
→ if grounded: shape next flat MW-xxx Codex Task Packet
→ Codex implementation
→ GPT Independent Review
→ integrate after Engineering PASS
→ canonical local checkout + fresh export
→ Owner UAT
```

If People audit proves the data owner/player-safe projection is not mature enough, do not force the Surface; choose the next grounded G6 consumer from current evidence instead.
