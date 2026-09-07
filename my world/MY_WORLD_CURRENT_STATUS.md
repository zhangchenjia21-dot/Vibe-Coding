---
title: my world｜当前状态
status: current-project-status
version: 17.1
created: 2026-08-26
updated: 2026-09-07
phase: G6 RPG Core Closure + Internal Dynamic UI
current_task: Combined Owner UAT — MW-018 People + MW-019 Recommendations
current_owner: Owner
parent_task: G6 Core Closure Package 0
semantic_owner: GPT
owner_uat_required: true
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
current_roadmap: MY_WORLD_总体规划路线图_CURRENT.md@v4.2
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

G6 RPG Core Closure + Internal Dynamic UI   ACTIVE
```

Current G6 implementation state：

```text
MW-011 RPG Host / Player Profile            PRODUCT PASS / CLOSED
MW-012 Zhang Chen Player Character Card     ENGINEERING PASS / INTEGRATED
MW-014 Model-driven Information Curation    ENGINEERING PASS / INTEGRATED
MW-015 Character + Important Experiences    PRODUCT PASS / CLOSED
MW-016 People Architecture Audit            PASS / CLOSED
MW-017 People Identity Bridge               ENGINEERING PASS / INTEGRATED
MW-018 People Curation + Card Surface        ENGINEERING PASS / INTEGRATED / OWNER UAT ACTIVE
MW-019 Five Recommended Actions             ENGINEERING PASS / INTEGRATED / OWNER UAT ACTIVE

Internal Dynamic UI / MW-013 capability     ROUTE AUTHORIZED AS CORE
old MW-013 Task Packet                      STALE / DO NOT EXECUTE AS-IS
Dynamic UI execution                       NOT CURRENT — after Packages 2–4 real consumers
```

---

## 2. Owner-approved Core-first route

2026-09-07 Owner 批准新版 Task Axis，最高优先级：

> **优先保证游戏尽快完成完整游戏闭环。核心开发先做；扩展功能与体验优化后置。**

Owner 同时明确：

> **动态 UI 很重要，Internal Dynamic UI Host v0.1 必须放在 V0 Core Closure Reality Gate 之前。**

CURRENT G6 顺序：

```text
Package 0  MW-018 + MW-019 Combined Owner UAT      ← CURRENT
↓
Package 1  OOC + Character-guided Recommendations
↓
Package 2  事务 / Open Threads
↓
Package 3  System / Public d20 real consumer
↓
Package 4  factual Inventory vertical
↓
Package 5  Internal Dynamic UI Host v0.1
↓
Package 6  V0 Core Closure Reality Gate
```

在 Package 6 Product PASS 前，除真实 blocker 外，不允许 Creator / Reference / model-management / diagnostics / richer information features 插队。

---

## 3. Current G6 Host / IA

```text
Player Status Host
→ portrait + real live mechanics/status HUD only
→ may collapse when empty

Narrative Host
→ GM Narrative + Player natural-language action
→ optional five recommendations near composer
→ primary visual/interaction surface

World Information Host
→ grounded player information Surfaces
→ later rendered increasingly through Internal Dynamic UI Host
```

Current integrated right-side set：

```text
概览 / 角色 / 重要经历 / 人物 / 存档
```

Mother taxonomy：

```text
概览 / 角色 / 重要经历 / 人物 / 事务 / 行囊 / 系统 / 地图 / 存档
```

Only grounded Surfaces appear. Do not create fake RPG state or expose omniscient Runtime truth merely to fill UI.

---

## 4. Protected information-curation authority

Canonical supporting decisions：

- `architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md`
- `architecture/ui/G6_CHARACTER_AND_IMPORTANT_EXPERIENCES_V1_0_DECISION.md`
- `architecture/ui/G6_INITIAL_CHARACTER_CURATION_BASELINE_V1_0_DECISION.md`
- `architecture/ui/G6_PEOPLE_SURFACE_V1_0_DECISION.md`
- `architecture/ui/G6_PEOPLE_IDENTITY_AND_CURATION_V1_0_DECISION.md`

Protected rule：

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

Program must not add keyword/name/encounter-count/importance/relationship heuristics to reproduce open semantics.

---

## 5. MW-018 People — OWNER UAT ACTIVE

Integrated behavior：

```text
人物
→ model-maintained latest-known cards
→ default collapsed
→ expand for relationship / latest-known details
→ hidden NPC changes do not auto-update
→ accepted replacement / Restore / reopen follow current history
```

Retained UAT risk：real new-actor identity correlation can occasionally be omitted by the model; exact bridge must continue refusing display-name guessing.

Owner UAT should test one known person and one newly introduced person.

---

## 6. MW-019 Recommendations — OWNER UAT ACTIVE

Protected product rule：

> **Five recommended actions != five allowed actions.**

Integrated behavior：

```text
accepted GM Narrative
→ player-safe Action Recommender
→ exactly five actions on successful valid output
→ click fills PlayerInput only
→ Player edits/ignores freely
→ normal send / d20 route unchanged
```

Retained UAT risk：one real Kimi response returned valid-looking fenced JSON and was intentionally rejected by strict parser. If normal play frequently loses recommendations, fix the real Structured Output seam within MW-019 lineage; do not add fence-stripping / hidden fallback heuristics.

---

## 7. CURRENT — Combined Owner UAT ACTIVE

Owner canonical playable checkout：

`D:/AI/Projects/my-world`

### Launch Ready evidence accepted

Owner-build preparation reported：

```text
reviewed product-code HEAD: 782daf65348f484d636d46260ac2374559cf554d
local HEAD == origin/main at preparation time
unknown .gitignore change + ten untracked files preserved unchanged
run-game.ps1 -ValidateExportOnly exit 0
Windows export rebuilt and verified against that checkout
```

After this preparation, implementation `main` advanced by one governance-only commit：

```text
c19ed2c965ed87b355cd5a44a0e40c9491139675
parent: 782daf65348f484d636d46260ac2374559cf554d
changed file: AGENTS.md only
```

This later commit changes execution/governance instructions only; it does not change game code, assets, runtime behavior or the reviewed MW-018/MW-019 product bytes. Therefore the already validated/exported `782daf6...` build remains the accepted Owner UAT artifact. **Do not require a rebuild solely for this governance-only commit.**

Owner can now run：

`D:/AI/Projects/my-world/run-game.cmd`

and perform combined MW-018 + MW-019 UAT.

### People
- card appears/updates after real player-authored turns；
- collapsed state useful；
- expanded relationship/details useful；
- no private/omniscient/debug leak；
- test one known person and one newly introduced person；
- later learned information updates the same stable card；
- Save/reopen/Restore stays current。

### Recommendations
- five useful suggestions appear when generation succeeds；
- feel optional, not restrictive；
- no hidden information；
- click prefills only and never sends；
- text remains editable；
- manual free-form input stays effortless；
- next action / Regenerate / Restore does not leave stale suggestions；
- observe whether `暂时没有推荐行动` occurs frequently enough to be a real product defect。

Owner product question：

> **“这些推荐让我更容易开始行动，同时我仍然觉得自己什么都能做吗？”**

Separate verdict lineage remains：

- People issue → MW-018 revision；
- Recommendation issue → MW-019 revision。

No Product PASS exists until Owner explicitly accepts each outcome.

---

## 8. Next core packages after current Gate

No new executable MW Work ID has been minted yet.

After Package 0 closes：

1. GPT Task Shapes Package 1 into the next bounded executable outcome；
2. Codex remains sole default production implementer；
3. GPT Independent Review；
4. Owner UAT for player-facing outcome；
5. continue Package 2 → 3 → 4；
6. **then重新 Task Shape Internal Dynamic UI Host** using all real consumer evidence；
7. finish Package 5；
8. run Package 6 V0 Core Closure Reality Gate。

Old MW-013 packet cannot be dispatched as-is.

---

## 9. Package 5 Dynamic UI route state

Dynamic UI is now a Core requirement, but not the current task.

Expected consumer evidence before implementation：

- Character；
- Important Experiences；
- People；
- Open Threads；
- System / Public d20；
- Inventory。

v0.1 remains internal only：

- presentation host, not gameplay owner；
- safe typed projection/contribution only；
- no omniscient world_state filtering at leaf UI；
- no arbitrary callbacks / NodePath / OS command / direct authoritative mutation；
- generic Action Intent deferred；
- external Source/Expansion Declarative UI deferred to G8。

---

## 10. V0 Core Closure target

Package 6 Reality Run must prove in one continuous real Game：

```text
Launch / New Game / Continue
→ GM opening
→ recommendations + free-form action
→ OOC
→ durable World / NPC consequence
→ Character / People / Open Threads
→ System / d20
→ Inventory mutation
→ Dynamic UI presentation
→ Save / reopen / Restore
→ all player-visible state and Dynamic UI follow current history
→ continue play
```

Suggested minimum evidence：20–30 turns, new NPC, one OOC, one d20, one item change, Save/reopen, Restore, one action clearly outside recommendations, and at least 3 Dynamic UI consumer types.

**Exit only by Owner explicit `V0 Core Game Loop = PRODUCT PASS`.**

---

## 11. Post-closure route

After V0 Product PASS：

```text
G7
→ Context Orchestrator / Structured Output Reliability
→ Knowledge Provenance / Epistemic / Freshness / Conflicts / Correction

G8
→ Shared History / Organization / World Chronicle / Consequence Diff
→ Narrative Preference / Bookmark / Notes / Chronicle / Manifest
→ model split / Compatibility / Profiles / usage / Debug Mode
→ Source Library / Reference / Creator
→ external UI contract only from proven Internal Dynamic UI vocabulary

G9
→ Standalone Alpha / Release Validation
```

---

## 12. Agent routing

```text
GPT
→ product semantics / architecture / Task Shaping / dispatch / Independent Review / UAT interpretation

Codex
→ sole default production implementer + local UAT-build preparation

Owner
→ Product UAT / explicit verdicts
```
