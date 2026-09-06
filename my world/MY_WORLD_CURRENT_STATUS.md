---
title: my world｜当前状态
status: current-project-status
version: 16.19
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-018 Owner UAT build handoff — People Curation + Card Surface
current_owner: Codex local UAT-build preparation lane
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
MW-018 People Curation + Card Surface        ENGINEERING PASS / INTEGRATED / OWNER UAT PENDING
MW-013 Internal Declarative UI Host          HOLD / NOT AUTHORIZED
```

## 2. Current G6 information architecture

```text
Player Status Host
→ portrait + live mechanics/status HUD only
→ may collapse when empty

Narrative Host
→ GM Narrative + Player natural-language action
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

## 7. MW-018 — ENGINEERING PASS / INTEGRATED / OWNER UAT PENDING

Task:

`my-world/docs/tasks/MW-018_PEOPLE_CURATION_AND_CARD_SURFACE_TASK.md`

Reviewed candidate:

`c8150a2ee9fa79d929cd0bb5f899132d3d2f6563`

Reviewed code commit:

`e9157f79860437207b73e894647be9f1278f5a74`

Independent Review:

`my-world/docs/mw018/MW-018_INDEPENDENT_REVIEW_IR1.md`

Review/integration tip:

`767001fb3e1962a28e77c778d72bb6e2ee0c7406`

Integration verification:

`my-world/docs/mw018/MW-018_INTEGRATION_VERIFICATION.md`

Integration verification commit:

`d0028be5004bc6563547e1f1d3f0aea06e817052`

Integrated product result:

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

Reviewed evidence:

- focused final: 135 checks / 0 failures;
- rendered visual checks: 127 / 0 across maximized, 1280×720 and 960×540;
- MW-017 / MW-014 / MW-015 and relevant G5 regressions pass;
- Windows Desktop export passes;
- `git diff --check` clean;
- one bounded real configured Kimi K3 vertical used exactly one World semantic call + one existing Information Curator call and produced a valid `李亭` card with unchanged Owner production fingerprints.

### Retained Owner-UAT reliability risk

The same real Kimi K3 run materialized new actor `沈青`, but the model referenced a transient candidate ref in `people_bindings` without placing `candidate_ref` on the candidate itself. The exact identity bridge therefore correctly refused to guess and no 沈青 card was produced.

This is not authorization for Program name matching. Owner UAT should intentionally include a newly introduced person and evaluate whether new-person card creation is sufficiently reliable in real play.

No Product PASS exists until Owner accepts the real application.

## 8. CURRENT — Owner UAT build handoff

Before Owner UAT:

```text
D:/AI/Projects/my-world
→ inspect branch/status/worktrees
→ preserve all unknown/dirty local work
→ safely fetch + fast-forward main to exact current origin/main
→ verify exact local HEAD
→ run run-game.ps1 -ValidateExportOnly
→ Owner Launch Ready
```

Never use reset/clean/force to hide local divergence. Never install the old task branch as the Owner build.

## 9. Owner UAT target

Owner should play at least one normal player-authored turn involving a person and verify:

```text
人物 tab exists
→ useful card appears or updates
→ card starts collapsed
→ collapsed state is quick to scan
→ expand reveals player-known relationship / latest-known details
→ no obvious private/omniscient/debug information leaks
→ later learned information updates the card
```

Owner should preferably test **a newly introduced person** as well as an already-known person because model candidate-ref reliability remains the main open product risk.

Verdict:

`PASS` or `NOT PASS — with concrete product/UAT findings`.

## 10. Next route

```text
Codex canonical local build preparation
→ Owner UAT MW-018

if PASS:
→ MW-018 PRODUCT PASS / CLOSED
→ choose/freeze next grounded G6 outcome
→ current proposed next product candidate: five model-generated suggested actions while preserving free-form input

if NOT PASS:
→ same MW-018 lineage if outcome is unchanged
→ GPT root-cause / scope classification
→ Codex correction
→ Independent Review + integration + Owner UAT again
```

MW-013 remains HOLD until multiple grounded Surfaces/mechanic consumers prove repeated patterns.

## 11. Agent routing

```text
GPT
→ product semantics / architecture / Task Shaping / dispatch / Independent Review

Codex
→ sole default production implementer and local UAT-build preparation agent

Owner
→ Product UAT / explicit product verdict for player-facing outcomes
```
