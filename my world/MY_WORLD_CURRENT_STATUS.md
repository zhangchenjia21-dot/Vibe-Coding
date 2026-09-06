---
title: my world｜当前状态
status: current-project-status
version: 16.20
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-019 Five Recommended Actions
current_owner: Codex implementation lane
parent_task: G6 RPG Experience & Internal Declarative UI Host
semantic_owner: GPT
owner_uat_required: true
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
MW-011 RPG Host / Player Profile            PRODUCT PASS / CLOSED
MW-012 Zhang Chen Player Character Card     ENGINEERING PASS / INTEGRATED
MW-014 Model-driven Information Curation    ENGINEERING PASS / INTEGRATED
MW-015 Character + Important Experiences    PRODUCT PASS / CLOSED
G6 People Surface product semantics         FROZEN
MW-016 People Architecture Audit            PASS / CLOSED
G6 People identity + curation architecture  FROZEN
MW-017 People Identity Bridge               ENGINEERING PASS / INTEGRATED
MW-018 People Curation + Card Surface        ENGINEERING PASS / INTEGRATED / OWNER UAT DEFERRED
G6 Five Recommended Actions semantics       FROZEN
MW-019 Five Recommended Actions             READY FOR CODEX
MW-013 Internal Declarative UI Host          HOLD / NOT AUTHORIZED
```

## 2. Current G6 information architecture

```text
Player Status Host
→ portrait + live mechanics/status HUD only
→ may collapse when empty

Narrative Host
→ GM Narrative + Player natural-language action
→ current action recommendations near composer
→ primary visual/interaction surface

World Information Host
→ grounded player information Surfaces
```

Current integrated right-side set:

```text
概览 / 角色 / 重要经历 / 人物 / 存档
```

Mother taxonomy remains:

```text
概览 / 角色 / 重要经历 / 人物 / 事务 / 行囊 / 系统 / 地图 / 存档
```

Only grounded Surfaces appear. Do not create fake RPG state or expose omniscient Runtime truth merely to fill UI.

## 3. Protected information-curation rule

Canonical authority:

- `architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md`
- `architecture/ui/G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_V1_0_DECISION.md`
- `architecture/ui/G6_INITIAL_CHARACTER_CURATION_BASELINE_V1_0_DECISION.md`
- `architecture/ui/G6_PEOPLE_SURFACE_V1_0_DECISION.md`
- `architecture/ui/G6_PEOPLE_IDENTITY_AND_CURATION_V1_0_DECISION.md`

Protected principle:

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

Program must not add keyword/name/encounter-count/importance/relationship heuristics to decide People meaning.

## 4. People product semantics — FROZEN

```text
人物 / People
→ card-based presentation
→ cards collapsed by default
→ collapsed = key player-known identity + brief latest-known positioning
→ expanded = relationship + latest-known summary/details
→ one card = player's current latest-known snapshot of one stable person
```

`latest-known != NPC omniscient current state`.

Off-screen Agency / World Evolution / NPC-private Knowledge / raw actor material must not update a People card until accepted player-visible information actually establishes the new knowledge.

People is not an actor registry, biography log or numeric Relationship system.

## 5. People identity + curation architecture — FROZEN

```text
accepted player-authored Turn
↓
World semantic lane
→ stable actor materialization when needed
→ exact request-scoped person identity binding receipt
↓
same-Turn current-version terminal barrier
↓
existing Information Curator one call
→ Character + Important Experiences + People
↓
information_curation currentness
↓
player-safe People projection
↓
card UI
```

Frozen boundaries:

- no authoritative display-name/fuzzy/first-match binding;
- same-name ambiguity remains unresolved rather than guessed;
- no People-specific default third Provider call;
- no raw stable actor/private material to People curator or UI;
- People latest-known snapshots live under `information_curation`, not NPC truth;
- no new SQLite table;
- old MW-014/MW-015 historical IDs remain valid through backward-compatible lived-record variants;
- People updates are full-snapshot replacement/tombstone by exact stable identity;
- leaf UI receives only safe content DTOs;
- Save/Restore/Regenerate/reopen follow accepted-history currentness;
- no GM-only opening People processing and no historical People backfill in v0.1.

## 6. MW-017 — ENGINEERING PASS / INTEGRATED

Task:

`my-world/docs/tasks/MW-017_PEOPLE_IDENTITY_BRIDGE_AND_BARRIER_TASK.md`

Independent Review:

`my-world/docs/mw017/MW-017_INDEPENDENT_REVIEW_IR1.md`

Integration verification:

`my-world/docs/mw017/MW-017_INTEGRATION_VERIFICATION.md`

MW-017 proved exact identity binding, same-turn runtime actor mint-before-bind, durable current receipts, full Player+GM prefix currentness, Restore epoch cancellation, fail-soft semantic terminal behavior and no disclosure of raw actor material.

## 7. MW-018 — ENGINEERING PASS / INTEGRATED / OWNER UAT DEFERRED

Task:

`my-world/docs/tasks/MW-018_PEOPLE_CURATION_AND_CARD_SURFACE_TASK.md`

Reviewed candidate:

`c8150a2ee9fa79d929cd0bb5f899132d3d2f6563`

Independent Review:

`my-world/docs/mw018/MW-018_INDEPENDENT_REVIEW_IR1.md`

Integration verification:

`my-world/docs/mw018/MW-018_INTEGRATION_VERIFICATION.md`

Current integrated People result:

```text
right 信息 navigation
→ 概览 | 角色 | 重要经历 | 人物 | 存档

人物
→ model-maintained latest-known cards
→ default collapsed
→ expand for relationship / summary / details
→ hidden NPC changes do not auto-update cards
→ accepted replacement / Restore / reopen re-project current history immediately
```

Engineering evidence is PASS. Product PASS is still pending.

### Owner route update

Owner explicitly chose on 2026-09-06:

```text
Do not run standalone MW-018 UAT now
→ implement Five Recommended Actions first
→ then test People + Recommendations together in one Owner build/session
```

Therefore MW-018 state is **OWNER UAT DEFERRED**, not Product PASS and not NOT PASS.

Retained People UAT risk remains new-person first-card reliability: a real Kimi K3 validation materialized `沈青` but omitted its transient candidate correlation field, so the safe identity bridge correctly declined to guess and no card appeared for that new actor.

## 8. Five Recommended Actions — PRODUCT / ARCHITECTURE FROZEN

Canonical decision:

`architecture/ui/G6_FIVE_RECOMMENDED_ACTIONS_V1_0_DECISION.md`

Product rule:

> **Five recommended actions != five allowed actions.**

Target experience:

```text
accepted GM Narrative
→ dedicated player-safe background Action Recommender
→ exactly 5 model-generated suggested actions
→ click one PREFILLS existing composer
→ player edits freely
→ existing Send / Ctrl+Enter remains authoritative
```

Key boundaries:

- free-form natural-language input always remains available;
- recommendation click never auto-sends;
- recommendation generation is independent/fail-soft and cannot block foreground play;
- recommendations use only bounded player-visible accepted Conversation material;
- no raw World / actor / private Knowledge / Agency / Evolution / Source material;
- recommendations are ephemeral derived UI, not Game truth and not persisted;
- successful accepted GM-only opening also receives recommendations;
- Restore/reopen may generate one fresh set for the current accepted prefix;
- new foreground action / Regenerate clears stale recommendations immediately;
- no hidden Provider fallback;
- no generic Action Intent / MW-013 framework is pulled forward.

## 9. CURRENT — MW-019 Five Recommended Actions

Task Packet:

`my-world/docs/tasks/MW-019_FIVE_RECOMMENDED_ACTIONS_TASK.md`

Identity:

```text
Work Item: MW-019
Type: product-facing Narrative Host vertical
Primary Implementer: Codex
Reviewer: GPT
Product UAT: Owner
Status: READY FOR CODEX
Branch: mw-019-five-recommended-actions
Worktree: D:/AI/Projects/.worktrees/my-world/mw-019
Return ceiling: READY FOR INDEPENDENT REVIEW
```

Required product result:

```text
GM Narrative accepted
→ five useful recommendation buttons appear near composer
→ buttons are optional guidance
→ click fills PlayerInput only
→ player can edit or ignore
→ normal send/d20 path is unchanged
```

Implementation should use a dedicated bounded player-safe recommendation call in parallel with existing post-Narrative background lanes. Do not alter the authoritative raw Narrative response format and do not reuse omniscient World semantic input for player-facing suggestions.

## 10. Roadmap correction / generic Action Intent

Canonical roadmap is now v4.1.

MW-019 is a fixed first-party Narrative interaction consumer. It may later provide evidence for generic G6-G `Bounded Action Intent`, but MW-019 does not authorize a generic intent schema/dispatcher or arbitrary callback system.

MW-013 remains HOLD.

## 11. Combined Owner UAT route

After MW-019:

```text
Codex implementation
→ GPT Independent Review
→ Engineering PASS
→ integrate reviewed main
→ safely sync D:/AI/Projects/my-world
→ ValidateExportOnly
→ one fresh Owner build
→ combined Owner UAT: MW-018 People + MW-019 Recommendations
```

Combined UAT still produces separate verdict lineage:

- People issue → MW-018 revision;
- Recommendation issue → MW-019 revision.

MW-019 Owner UAT must check:

```text
opening / completed GM turn produces five recommendations
→ suggestions are useful without feeling mandatory
→ no obvious hidden/omniscient information
→ click fills composer but does not send
→ text remains freely editable
→ manual free-form action remains effortless
→ stale options never survive next action / Regenerate / Restore
```

Owner product question:

> **“这些推荐让我更容易开始行动，同时我仍然觉得自己什么都能做吗？”**

## 12. Agent routing

```text
GPT
→ product semantics / architecture / Task Shaping / dispatch / Independent Review

Codex
→ sole default production implementer

Owner
→ combined Product UAT / explicit product verdict for MW-018 and MW-019
```
