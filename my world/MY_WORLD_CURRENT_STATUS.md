---
title: my world｜当前状态
status: current-project-status
version: 16.16
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-016 People Surface Architecture Audit
current_owner: Codex architecture-audit lane
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
G6 Character + Important Experiences        PRODUCT BASELINE ACCEPTED
MW-014 Model-driven Information Curation    ENGINEERING PASS / INTEGRATED
MW-015 Character + Important Experiences    PRODUCT PASS / CLOSED
G6 People Surface product semantics         FROZEN
MW-016 People Surface Architecture Audit    READY FOR CODEX AUDIT
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

Current implemented right-side set:

```text
概览 / 角色 / 重要经历 / 存档
```

Only grounded Surfaces appear. Do not create fake RPG pages or expose omniscient Runtime state merely to fill UI.

## 3. Character + Important Experiences — PRODUCT BASELINE ACCEPTED

Canonical authority:

- `architecture/ui/G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_V1_0_DECISION.md`
- `architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md`
- `architecture/ui/G6_INITIAL_CHARACTER_CURATION_BASELINE_V1_0_DECISION.md`

```text
角色 / Character
→ evolving current Character Sheet
→ “现在的我是谁”

重要经历 / Important Experiences
→ protagonist-centered milestone history
→ “我是怎样走到现在的”
```

Frozen curation principle:

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

Program must not add keyword/regex semantic routing, importance scores, event-type rule trees or protagonist-choice heuristics.

## 4. MW-015 — PRODUCT PASS / CLOSED

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

Owner explicitly accepted the current semantic/product outcome. Visual density, typography, spacing and hierarchy remain deferred G6 UI polish and do not reopen MW-015.

## 5. People Surface — PRODUCT SEMANTICS FROZEN

Canonical authority:

`architecture/ui/G6_PEOPLE_SURFACE_V1_0_DECISION.md`

Owner decision:

```text
人物 / People
→ card-based presentation
→ cards collapsed by default
→ collapsed state shows only key identity / very brief latest-known summary
→ relationship and detailed information live in expanded state
→ each card represents only the player's current latest-known snapshot of that person
```

Core semantics:

- People is not an omniscient actor registry;
- a card may exist when accepted player-visible history has made the player meaningfully aware of a distinct person and the model judges that person worth maintaining;
- direct meeting is not mandatory; reliable in-game knowledge may also establish a card;
- Source existence, stable actor existence, GM omniscience or the protagonist's original-world historical knowledge do not automatically create a card;
- off-screen NPC changes do not update People until the player actually learns them;
- later player-visible information may replace/correct/remove prior player-known information;
- People keeps the latest player-known snapshot, not a biography/history log;
- expanded card may contain natural-language player-known relationship summary, but no numeric Relationship/affinity system is authorized;
- stable Game-local actor identity is required; display name is not authoritative identity;
- model decides semantic inclusion/update; Program owns identity, structure, currentness and presentation.

`World Truth != actor Knowledge != human-player disclosure` remains protected.

## 6. Existing implementation evidence relevant to People

G5 already provides:

- Program-owned stable NPC local identities;
- Guaranteed / Source-backed / creation-authored / runtime-narrative actor families;
- runtime actor materialization from accepted Narrative;
- actor Knowledge provenance;
- actor Agency/currentness boundaries.

But current player-safe UI projection only proves player-character known facts and does not yet expose a dedicated per-person latest-known read model.

Important implementation risk:

```text
stable actor registry
!= player-visible People list
```

Current `stable_actor_material(...)` may contain Source-backed/Game-local actor material that is not automatically player-safe. People must not dump it to the curator/UI.

## 7. CURRENT — MW-016 architecture audit

Task Packet:

`my-world/docs/tasks/MW-016_PEOPLE_SURFACE_ARCHITECTURE_AUDIT_TASK.md`

Identity:

```text
Work Item: MW-016
Type: planning / architecture audit
Executor: Codex
Architecture/Semantic Owner: GPT
Status: READY FOR CODEX AUDIT
Return ceiling: READY FOR GPT ARCHITECTURE DECISION
Production implementation: NOT AUTHORIZED YET
```

Audit must resolve with code evidence:

1. exact People-card → stable `local_character_id` binding without authoritative display-name matching;
2. same-turn runtime actor materialization vs Information Curator ordering/identity race;
3. minimum safe identity-resolution metadata the curator may receive;
4. durable owner shape for current People player-known snapshots;
5. bounded curation contract shape;
6. model-owned card eligibility/update/removal without Program heuristics;
7. Restore / Regenerate / reopen currentness;
8. whether one Information Curator can continue to maintain Character + Important Experiences + People without an unnecessary extra model call.

No People UI or production contract change is authorized during this audit.

## 8. Agent routing

Canonical authority:

`AGENT_EXECUTION_ROUTING_CURRENT.md v3.1`

```text
GPT
→ product semantics / architecture / Task Shaping / dispatch / Independent Review

Codex
→ sole default implementation agent and current repo-level architecture audit executor

Owner
→ Product UAT / explicit product verdict
```

KimiCode / Zcode / other implementation agents require explicit future Owner re-authorization.

## 9. Visual Runtime / MW-013 disposition

```text
Runtime Asset Resolution / portrait / scene / authored-map
→ DEFERRED until real authored visual demand exists

MW-013 Internal Declarative UI Host v0.1
→ HOLD / NOT AUTHORIZED YET
```

Do not re-enter MW-013 until multiple grounded real Surfaces / mechanic consumers expose repeated patterns.

## 10. Immediate route

```text
MW-016 Codex architecture audit
→ GPT architecture decision / freeze identity + disclosure + curation owner
→ if grounded: shape People implementation Task Packet / implementation revision
→ Codex implementation
→ GPT Independent Review
→ integrate after Engineering PASS
→ canonical local checkout + fresh export
→ Owner UAT
```

If the audit proves People cannot be safely grounded without a larger prerequisite, stop and classify that prerequisite rather than dumping stable actors or hidden material into the UI.
