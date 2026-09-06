---
title: my world｜当前状态
status: current-project-status
version: 16.17
created: 2026-08-26
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
current_task: MW-017 People Identity Bridge + Same-turn Barrier
current_owner: Codex implementation lane
parent_task: G6 RPG Experience & Internal Declarative UI Host
semantic_owner: GPT
owner_uat_required: false
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
MW-017 People Identity Bridge               READY FOR CODEX
MW-018 People Curation + Card Surface        BLOCKED BY MW-017
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

Current implemented right-side set:

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
→ collapsed = key identity + very brief latest-known positioning
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

## 5. MW-016 — ARCHITECTURE AUDIT PASS / CLOSED

Audit evidence:

`my-world/docs/mw016/MW-016_PEOPLE_ARCHITECTURE_AUDIT.md`

Audit commit:

`4aeff59108bc5f3084b23237f00a34e9252d76dc`

Accepted finding:

```text
existing stable actor identity + Timeline storage are reusable
BUT
there is no safe exact accepted-person → stable-ID bridge
AND
current World semantic worker / Information Curator have no real same-turn ordering barrier
```

Therefore People UI must not be implemented by dumping `stable_npcs`, using display-name matching or passing the full stable roster/raw NPC material to the curator.

## 6. People identity + curation architecture — FROZEN

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

- adopt MW-016 Scheme A: same-turn barrier;
- do not intentionally delay a usable new person until the next Turn;
- no People-specific default third Provider call;
- no display-name/fuzzy/first-match authoritative identity binding;
- ambiguous same-name identity remains unresolved rather than guessed;
- identity receipt belongs to existing World / `living_world` owner;
- People latest-known snapshots belong to `information_curation`, not NPC truth;
- no new SQLite table;
- old MW-014/MW-015 curation IDs/parent chain must remain valid through backward-compatible record variants;
- People card updates are full-snapshot replacement/tombstone by exact stable local identity;
- leaf UI receives only safe card DTOs, never raw World/local IDs/receipt/hash;
- Restore/Regenerate/reopen currentness follows accepted history;
- displaced-future People must never use the MW-015 Initial Character baseline recovery exception.

### v0.1 explicit limitations

```text
GM-only opening
→ not processed for People in v0.1

old Game historical backlog
→ no silent model backfill in v0.1
→ People starts accumulating from new player-authored accepted Turns
```

These are deliberate scope/cost/currentness choices, not claims that earlier information is unimportant.

## 7. CURRENT — MW-017

Outcome:

> Establish the exact stable-NPC identity bridge and same-turn coordination needed by People, without implementing People content or UI yet.

MW-017 is backend-only and does not require Owner product UAT.

It must prove:

- existing and same-turn runtime-created actors bind by exact Program identity;
- transient candidate refs cannot drift after normalization;
- same-name ambiguity never falls back to name matching;
- successful/no-op identity opportunities create durable replayable receipts;
- semantic failure/timeout remains fail-soft and does not block accepted Narrative or Character/Important Experiences;
- Restore/Regenerate cannot publish stale receipts;
- no hidden actor material becomes People-curator disclosure evidence;
- no SQLite table is added;
- G5 actor/knowledge/agency and MW-014/015 regressions remain valid.

Return ceiling:

`READY FOR INDEPENDENT REVIEW`

## 8. Next route

```text
MW-017 Codex implementation
→ GPT Independent Review
→ if Engineering PASS: integrate
→ shape MW-018 against proven bridge
→ MW-018 People Curation + Card Surface
→ GPT Independent Review
→ Owner-build sync/export
→ Owner UAT
```

MW-018 is not authorized to start before MW-017 Engineering PASS.

MW-013 remains HOLD until multiple grounded Surfaces/mechanic consumers prove repeated UI patterns.

## 9. Agent routing

```text
GPT
→ product semantics / architecture / Task Shaping / dispatch / Independent Review

Codex
→ sole default production implementer

Owner
→ Product UAT / explicit product verdict when a player-facing outcome is ready
```
