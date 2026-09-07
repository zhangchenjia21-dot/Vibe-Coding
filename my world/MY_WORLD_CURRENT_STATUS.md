---
title: my world｜当前状态
status: current-project-status
version: 17.3
created: 2026-08-26
updated: 2026-09-07
phase: G6 RPG Core Closure + UAT Observability + Internal Dynamic UI
current_task: Package 0 UAT corrections shaping — MW-018 / MW-019 + MW-015 semantic correction
current_owner: GPT
parent_task: G6 Core Closure Package 0
semantic_owner: GPT
owner_uat_required: true
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.3
owner_uat_record: my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md
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

G6 RPG Core Closure + UAT Observability + Internal Dynamic UI ACTIVE
```

Current G6 state：

```text
MW-011 RPG Host / Player Profile            PRODUCT PASS / CLOSED
MW-014 Model-driven Information Curation    ENGINEERING PASS / INTEGRATED
MW-015 Character + Important Experiences    HISTORICAL PRODUCT PASS / POST-PASS FINDING REOPENED
MW-017 People Identity Bridge               ENGINEERING PASS / INTEGRATED
MW-018 People Curation + Card Surface        ENGINEERING PASS / INTEGRATED / PRODUCT FAIL — REVISION REQUIRED
MW-019 Five Recommended Actions             ENGINEERING PASS / INTEGRATED / PRODUCT FAIL — REVISION REQUIRED

UAT Observability / Debug Mode v0.1         ROUTE AUTHORIZED — NEXT AFTER PACKAGE 0 CLOSE
Internal Dynamic UI Host capability         ROUTE AUTHORIZED AS CORE
old MW-013 Task Packet                      STALE / DO NOT EXECUTE AS-IS
```

---

## 2. Owner-approved G6 order

```text
Package 0  MW-018 / MW-019 UAT corrections + focused re-UAT   ← CURRENT
↓
Package 1  UAT Observability / Debug Mode v0.1
↓
Package 2  OOC + Character-guided Recommendations
↓
Package 3  事务 / Open Threads
↓
Package 4  System / Public d20 real consumer
↓
Package 5  factual Inventory vertical
↓
Package 6  Internal Dynamic UI Host v0.1
↓
Package 7  V0 Core Closure Reality Gate
```

Owner priority remains：

> **优先保证游戏尽快完成完整游戏闭环。核心开发先做；扩展功能与体验优化后置。**

Owner additionally required Debug Mode / per-turn backend change visibility immediately after Package 0 because it materially reduces subsequent Owner UAT cost.

---

## 3. Package 0 Owner UAT U1 — COMPLETE

Formal evidence：

`my-world/docs/uat/G6_PACKAGE0_OWNER_UAT_U1.md`

Tested product-code artifact：

`782daf65348f484d636d46260ac2374559cf554d`

Later `main` commits during UAT were governance-only until the UAT record commit; they did not alter the tested product behavior.

### MW-018 verdict

**PRODUCT FAIL / REVISION REQUIRED**

Observed：

- player had already learned about `钟繇` and explicitly submitted a later turn recalling him；
- no People card appeared for `钟繇`；
- two incidental soldiers in the current scene did receive cards。

Root-cause audit：current MW-018 architecture makes only **current identity-receipt-bound actors** eligible for People update. The current receipt seam is centered on exact current semantic/GM-span actor bindings. Therefore an already player-known/off-screen person may never be exposed to the Curator as a legal `actor_ref`, even though frozen product semantics explicitly allow cards for people learned about without a current physical encounter.

Correction must preserve exact identity / no display-name guessing while broadening the safe exact known-person eligibility seam. Player assertion alone must not mint World Truth when identity/existence is unresolved.

### MW-019 verdict

**PRODUCT FAIL / REVISION REQUIRED**

Confirmed findings：

1. recommendation chips contain long full-action prose and are crowded/hard to scan；
2. desired interaction is `short direction → click → detailed editable draft in composer`；
3. a five-item set can behave like one plan split into five sentences rather than five independently selectable next actions；
4. recommendation/composer UI sizing, font and spacing are too small/cramped for comfortable play。

Character-guided recommendations remain later Package 2 work; it does not replace the basic requirement that the five current suggestions be independent alternatives.

### MW-015 post-PASS finding

`重要经历` over-generated during extended play and behaved like a per-turn recap. Before V0 Core Closure, curation semantics must be tightened so ordinary turns very often produce no milestone update.

Product IA follow-up remains **not silently authorized**：

- a separate `简要回顾` could be useful；
- whether `重要经历` remains top-level or moves under Character still requires explicit product approval before changing IA。

---

## 4. Protected correction boundaries

All Package 0 corrections must preserve：

- free-form Player action remains primary；
- recommendations never define legal actions and click never auto-sends；
- `World Truth != actor Knowledge != human-player disclosure`；
- exact stable identity over display-name/fuzzy guessing；
- Player belief alone does not automatically create World Truth；
- model owns open semantic importance / curation；Program owns structure/currentness/persistence；
- Save / Restore / Regenerate currentness remains authoritative；
- no generic framework / EventBus / universal resolver is pulled forward merely to fix these findings。

---

## 5. Current correction shaping order

Because MW-018 and MW-015 both touch Information Curation semantics, do not run conflicting edits in parallel without an explicit integration plan.

Preferred sequence：

```text
A. MW-018 Revision — known/off-screen People eligibility + exact identity seam + People importance prompt
↓ Independent Review
B. MW-015 Revision — sparse milestone semantics regression correction
↓ Independent Review
C. MW-019 Revision — concise labels + detailed drafts + independent alternatives + readability
↓ Independent Review
D. fresh combined/focused Owner re-UAT
↓
Package 0 close
```

Implementation may be reordered only if current code evidence proves a safer lower-conflict sequence.

---

## 6. Package 1 UAT Observability — NEXT AFTER PACKAGE 0

Target v0.1：

```text
Debug Mode OFF
→ normal play unchanged

Debug Mode ON
→ accepted Turn 后 compact UAT trace
→ domain/lane terminal + changed / no-change / failed / stale / cancelled
→ errors explain why in human-readable form
```

Initial consumers：Narrative, World semantic, actor/identity bridge, Character, Important Experiences, People, Recommendations, Save/Restore/currentness. Later Open Threads / mechanics / Inventory join the same read-only diagnostic projection.

Default debug view does not expose hidden GM/NPC-private values; player-visible projections may show safe diffs. Debug Mode cannot mutate gameplay truth or alter model/mechanics/currentness.

---

## 7. Dynamic UI / V0 closure

Dynamic UI remains Core-required after Open Threads / System / Inventory provide the remaining real consumers. Old MW-013 packet cannot be executed as-is.

Package 7 Reality Run exits only by explicit Owner：

`V0 Core Game Loop = PRODUCT PASS`

---

## 8. Agent routing

```text
GPT
→ correction architecture / Task Shaping / dispatch / Independent Review / UAT interpretation

Codex
→ sole default production implementer

Owner
→ focused Product re-UAT / explicit verdicts
```
