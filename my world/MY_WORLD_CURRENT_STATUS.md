---
title: my world｜当前状态
status: current-project-status
version: 16.21
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: Combined Owner UAT build handoff — MW-018 People + MW-019 Recommendations
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
G6 Five Recommended Actions semantics       FROZEN
MW-019 Five Recommended Actions             ENGINEERING PASS / INTEGRATED / OWNER UAT PENDING
MW-013 Internal Declarative UI Host          HOLD / NOT AUTHORIZED
```

## 2. Current G6 information architecture

```text
Player Status Host
→ portrait + live mechanics/status HUD only
→ may collapse when empty

Narrative Host
→ GM Narrative + Player natural-language action
→ optional current action recommendations near composer
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

Retained People UAT risk: a real Kimi K3 validation materialized new actor `沈青` but omitted the transient candidate correlation field required for exact identity binding. The bridge correctly refused to guess, so no card was created for that actor. Owner UAT should intentionally test a newly introduced person as well as an already-known person.

## 8. Five Recommended Actions — PRODUCT / ARCHITECTURE FROZEN

Canonical decision:

`architecture/ui/G6_FIVE_RECOMMENDED_ACTIONS_V1_0_DECISION.md`

Product rule:

> **Five recommended actions != five allowed actions.**

Target experience:

```text
accepted GM Narrative
→ dedicated player-safe background Action Recommender
→ exactly 5 model-generated suggested actions on valid output
→ click one PREFILLS existing composer
→ player edits freely
→ existing Send / Ctrl+Enter / Public d20 remains authoritative
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

## 9. MW-019 — ENGINEERING PASS / INTEGRATED / OWNER UAT PENDING

Task Packet:

`my-world/docs/tasks/MW-019_FIVE_RECOMMENDED_ACTIONS_TASK.md`

Reviewed implementation commit:

`bf9996e67d267871e918f82bd9ad6d2fb539ff0b`

Reviewed candidate + evidence:

`5a06f636e332c600d9a5bb327a92792ec7c42c17`

Independent Review IR1:

`my-world/docs/mw019/MW-019_INDEPENDENT_REVIEW_IR1.md`

Independent Review commit / first integrated reviewed tip:

`a1841a63f549bab17517b2cae399345421cf1764`

Integration verification:

`my-world/docs/mw019/MW-019_INTEGRATION_VERIFICATION.md`

Integration verification commit:

`df6c6cc5beaf63bc82850e6b9d030d283b14970d`

Integrated product result:

```text
accepted GM Narrative
→ player-safe Action Recommender
→ compact recommendation area near composer
→ exactly five actions on successful structured output
→ click fills PlayerInput only
→ player may edit/ignore
→ normal send/d20 route unchanged
```

Reviewed engineering evidence:

- focused final: **122 checks / 0 failures**;
- aggregate 26 regression/export suites: all exit code 0;
- relevant G2/G4/G5/MW-014/MW-015/MW-017/MW-018 regressions pass;
- Windows Desktop export PASS;
- `git diff --check` clean;
- no recommendation persistence or new SQLite owner;
- input is bounded accepted Player/GM transcript only;
- one bounded real configured Kimi K3 call produced five useful recommendations with unchanged Owner production fingerprints.

### Retained Recommendation UAT risk

In the same bounded real validation, a second normal-turn recommendation response was valid-looking JSON wrapped in a Markdown code fence. The strict MW-019 parser correctly rejected it; no heuristic fence stripping, retry or hidden Provider fallback was used.

Therefore Engineering PASS stands, but recommendation **formatting reliability** is a real Product/UAT risk. If normal play frequently shows `暂时没有推荐行动`, keep any correction in MW-019 lineage and improve the structured-output/model seam deliberately rather than adding semantic repair heuristics.

A non-blocking review observation also remains for reopened Games with an unresolved durable d20 action: if the combined UAT ever shows recommendations while the existing d20 recovery path requires retry, classify that concrete UX behavior then; no corruption/bypass evidence currently exists.

## 10. CURRENT — Combined Owner UAT build handoff

Owner explicitly chose to test MW-018 + MW-019 together.

Before UAT:

```text
D:/AI/Projects/my-world
→ inspect branch/status/worktrees
→ preserve all unknown/dirty local work
→ safely fetch + fast-forward main to exact current origin/main
→ verify exact local HEAD
→ run run-game.ps1 -ValidateExportOnly
→ Owner Launch Ready
```

Never use reset/clean/force to hide local divergence. Never install either task branch as the Owner build.

## 11. Combined Owner UAT target

### MW-018 People

Owner should verify:

```text
人物 tab exists
→ useful card appears/updates after normal player-authored turns involving people
→ card starts collapsed
→ collapsed state is quick to scan
→ expand reveals relationship / latest-known details
→ no obvious private/omniscient/debug information
→ later learned information updates the same card
```

Prefer one known person and one newly introduced person.

### MW-019 Recommendations

Owner should verify:

```text
accepted opening / completed GM turn
→ five recommendations appear reasonably quickly when generation succeeds
→ suggestions are useful without feeling mandatory
→ no obvious hidden/omniscient information
→ click fills composer but does not send
→ text remains freely editable
→ manual free-form action remains effortless
→ next action / Regenerate / Restore never leaves stale suggestions
```

Owner product question:

> **“这些推荐让我更容易开始行动，同时我仍然觉得自己什么都能做吗？”**

Combined UAT still produces separate verdict lineage:

- People issue → MW-018 revision;
- Recommendation issue → MW-019 revision.

No Product PASS exists for either until Owner explicitly accepts it.

## 12. SillyTavern upstream functional reference — NON-CANONICAL

Owner-requested external functional reference has been preserved at:

`experience/SILLYTAVERN_UPSTREAM_FUNCTIONAL_REFERENCE_AUDIT_2026-09-06.md`

It records useful upstream feature lessons, optional future directions and mechanisms to adapt/reject. It is **non-canonical reference only**: it does not authorize new tasks or override current Product / Architecture / Roadmap / Status. Future improvements may cite it when a concrete player/author problem justifies reopening one of those directions.

## 13. Roadmap / deferred platform

Canonical roadmap remains v4.1.

MW-019 is a fixed first-party Narrative interaction consumer and evidence for possible later G6-G `Bounded Action Intent`; it does not authorize a generic intent schema/dispatcher.

MW-013 remains HOLD.

## 14. Agent routing

```text
GPT
→ product semantics / architecture / Task Shaping / dispatch / Independent Review / UAT interpretation

Codex
→ sole default production implementer and local UAT-build preparation agent

Owner
→ combined Product UAT / explicit separate product verdicts for MW-018 and MW-019
```
