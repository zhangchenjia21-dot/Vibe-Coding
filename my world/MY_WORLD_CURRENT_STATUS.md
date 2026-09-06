---
title: my world｜当前状态
status: current-project-status
version: 16.18
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-018 People Curation + Card Surface
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
MW-018 People Curation + Card Surface        READY FOR CODEX
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

Current implemented right-side set before MW-018:

```text
概览 / 角色 / 重要经历 / 存档
```

Current mother taxonomy:

```text
概览 / 角色 / 重要经历 / 人物 / 事务 / 行囊 / 系统 / 地图 / 存档
```

Only grounded Surfaces appear. Do not create fake RPG state or expose omniscient Runtime truth merely to fill UI.

## 3. Character + Important Experiences — accepted baseline

Canonical authority:

- `architecture/ui/G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_V1_0_DECISION.md`
- `architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md`
- `architecture/ui/G6_INITIAL_CHARACTER_CURATION_BASELINE_V1_0_DECISION.md`

Protected principle:

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

MW-015 is PRODUCT PASS / CLOSED. Visual density/typography/spacing remain deferred G6 polish and do not reopen it.

## 4. People Surface — product semantics FROZEN

Canonical product decision:

`architecture/ui/G6_PEOPLE_SURFACE_V1_0_DECISION.md`

Owner-approved product form:

```text
人物 / People
→ card-based presentation
→ cards collapsed by default
→ collapsed = key player-known identity + very brief latest-known positioning
→ expanded = relationship + identity + traits + latest-known details
→ each card stores only the player's current latest-known snapshot of that person
```

Important meaning:

```text
latest-known
!= NPC omniscient current state
```

Off-screen Agency/World Evolution/private Knowledge does not update People until the player actually learns the new information.

People is not an actor registry, biography log or numeric Relationship system.

## 5. People identity + curation architecture — FROZEN

Canonical decision:

`architecture/ui/G6_PEOPLE_IDENTITY_AND_CURATION_V1_0_DECISION.md`

Approved route:

```text
accepted player-authored Turn
↓
existing World semantic lane
→ stable actor materialization when needed
→ exact identity binding receipt
↓
current-version terminal barrier
↓
existing Information Curator
→ Character / Important Experiences / People in one bounded curation call
↓
information_curation currentness
↓
player-safe People projection
↓
card UI
```

Frozen decisions:

- same-Turn Scheme A is required;
- no People-specific default third Provider call;
- no display-name/fuzzy/first-match authoritative identity binding;
- ambiguous same-name identity remains unresolved rather than guessed;
- identity receipt belongs to existing World / `living_world` owner;
- People latest-known snapshots belong to `information_curation`, not NPC truth;
- no new SQLite table;
- old MW-014/MW-015 curation IDs/parent chain remain valid through backward-compatible record variants;
- People updates are full-snapshot replacement/tombstone by exact stable local identity;
- leaf UI receives only safe card DTOs, never raw World/local IDs/receipt/hash;
- Restore/Regenerate/reopen currentness follows accepted history;
- People never uses the MW-015 Initial Character displaced-future recovery exception;
- GM-only opening and historical People backfill remain out of v0.1.

## 6. MW-016 — PASS / CLOSED

Audit evidence:

`my-world/docs/mw016/MW-016_PEOPLE_ARCHITECTURE_AUDIT.md`

Audit commit:

`4aeff59108bc5f3084b23237f00a34e9252d76dc`

Accepted finding:

```text
stable identity + Timeline are reusable
BUT
People required an exact accepted-person → stable-ID bridge
AND
a real same-Turn semantic → curator barrier
```

Those prerequisites are now implemented by MW-017.

## 7. MW-017 — ENGINEERING PASS / INTEGRATED

Task:

`my-world/docs/tasks/MW-017_PEOPLE_IDENTITY_BRIDGE_AND_BARRIER_TASK.md`

Reviewed candidate:

`dd7b56fb030085b5c74dbe221bf8b5741bef3724`

Reviewed code commit:

`2be0c532d89c0fa7ae1d473d408a42785a32c8d9`

Independent Review:

`my-world/docs/mw017/MW-017_INDEPENDENT_REVIEW_IR1.md`

Review/integration tip:

`27519c0e11ff995df4c584e835dc0be39ee3f4d5`

Integration verification:

`my-world/docs/mw017/MW-017_INTEGRATION_VERIFICATION.md`

Integrated implementation main verification commit:

`73004d67ef14be2fb2bb7c386e6a89a604e93f8f`

Engineering result:

```text
accepted player-authored Turn
→ World semantic model can bind accepted-person spans to request-scoped actor/candidate refs
→ Program resolves those refs to exact stable local IDs
→ same-Turn runtime-created actor is minted before binding
→ actor + durable identity receipt commit atomically
→ current-version terminal releases Information Curator
```

Reviewed protections:

- no authoritative name matching;
- full Player+GM prefix currentness;
- Restore epoch blocks stale callbacks;
- failure/cancel/timeout remains fail-soft and releases Character/Important Experiences curation;
- bridge evidence contains accepted GM spans/quotes only, not raw actor profile/private Knowledge/Agency/Evolution/Source-current material;
- old Games remain valid with no backfill;
- no SQLite table/migration;
- no People UI/content schema yet.

Reviewed evidence includes 120 focused checks / 0 failures, G5/MW-014/MW-015 regressions, Windows export, and one bounded real configured Kimi K3 identity-binding smoke with unchanged Owner production fingerprints.

MW-017 is backend-only and requires no Owner Product UAT.

## 8. CURRENT — MW-018 People Curation + Card Surface

Task Packet:

`my-world/docs/tasks/MW-018_PEOPLE_CURATION_AND_CARD_SURFACE_TASK.md`

Implementation repository task-shaping commit:

`b5af70c7f956c1de062461bc2aca14a67e537787`

Identity:

```text
Work Item: MW-018
Type: G6 product-facing vertical implementation
Primary Implementer: Codex
Reviewer: GPT
Product UAT: Owner
Status: READY FOR CODEX
Branch: mw-018-people-curation-card-surface
Worktree: D:/AI/Projects/.worktrees/my-world/mw-018
Return ceiling: READY FOR INDEPENDENT REVIEW
```

Required product result:

```text
right 信息 navigation
→ 概览 | 角色 | 重要经历 | 人物 | 存档

人物
→ cards collapsed by default
→ compact scan-level identity/headline when collapsed
→ relationship + details only after expand
→ one card = player's latest-known snapshot
→ later player-visible information updates that snapshot
→ hidden/off-screen NPC changes do not leak into the card
```

Implementation must extend the existing Information Curator one-call contract rather than adding a third People model call.

People content must consume only MW-017 accepted identity evidence + current player-known snapshots; never raw stable actor material.

### Product UAT requirement

MW-018 is player-facing. Agent / Engineering Review may only reach:

`READY FOR OWNER UAT`

Owner must accept the real card experience before Product PASS.

## 9. Explicit v0.1 People limitations

```text
GM-only opening
→ no People processing

old Game historical backlog
→ no automatic model backfill
→ People starts from new player-authored accepted opportunities after feature install

Relationship
→ natural-language player-known summary only
→ no affinity/trust/hostility numeric Domain
```

## 10. Next route

```text
MW-018 Codex implementation
→ GPT Independent Review
→ integrate only after Engineering PASS
→ canonical Owner checkout sync + fresh Windows export
→ Owner UAT People card experience

if Owner UAT PASS:
→ MW-018 PRODUCT PASS / CLOSED
→ choose next grounded G6 outcome

if NOT PASS:
→ same MW-018 lineage if outcome remains unchanged
→ GPT root-cause / scope classification
→ Codex correction
→ Independent Review + Owner UAT again
```

MW-013 remains HOLD until multiple grounded Surfaces/mechanic consumers prove repeated UI patterns.

## 11. Agent routing

```text
GPT
→ product semantics / architecture / Task Shaping / dispatch / Independent Review

Codex
→ sole default production implementer

Owner
→ Product UAT / explicit product verdict for player-facing outcomes
```
