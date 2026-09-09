---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 4.9
created: 2026-08-25
updated: 2026-09-09
current_phase: G6
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
supersedes: v4.8
---

# my world｜总体规划路线图 CURRENT

## 0. 路线原则

本文件拥有 G1–G9 阶段顺序、当前 Core-first Package Axis、Stage Gate 与 Deferred/Non-scope。实时 owner / current task / build/UAT 状态以 `MY_WORLD_CURRENT_STATUS.md` 为准。

Owner 已冻结：

> **优先保证游戏尽快完成完整游戏闭环。核心开发先做；扩展功能、体验优化、Creator、Reference 与其它外围增强后置。**

> **Internal Dynamic UI 是 V0 核心能力，必须在 V0 Core Closure Reality Gate 前完成。**

通用顺序：Vertical before platform；Consumer before Creator；真实需求 → 最小能力 → 真实 consumer → Engineering Review → Owner Product evidence。

---

## 1. 总体阶段

```text
G1 Foundation & Project Bootstrap                PASS / CLOSED
G2 AI Conversation Spine                         PASS / CLOSED
G3 Persistent Game / Save / Timeline Foundation PASS / CLOSED
G4 Primary Source Assets & Local Game Creation   PASS / CLOSED
G5 World Semantics & GM Runtime                  PRODUCT PASS / CLOSED
G6 RPG Core Closure + UAT Observability + Internal Dynamic UI ACTIVE / REALITY GATE
G7 Long-session Context & Knowledge Hardening    QUEUED
G8 Product Expansion / Authoring / External Contract QUEUED
G9 Standalone Alpha / Release Validation         QUEUED
```

当前技术/产品脊柱：

```text
Launch / Main Menu
→ Continue / New Game
→ AI GM Narrative
→ Player free-form natural-language action
→ OOC / Recommendations
→ durable World / actor consequence
→ Character / Important Experiences / People / Open Threads
→ Public d20 / System
→ factual Inventory
→ Internal Dynamic UI presentation
→ Save / exit / reopen / Restore
→ continue play
```

G6 当前不再增加计划内核心能力；进入完整 V0 Core Reality Gate。

---

# G6｜Core-first Package Axis

## Package 0｜Correction Train + Focused Owner UAT｜PRODUCT PASS / CLOSED

Closed outcomes:

```text
MW-018 R1 People                  PRODUCT PASS
MW-015 R1 Important Experiences  PRODUCT PASS
MW-019 R1 Recommendations        PRODUCT PASS
MW-020 Context Budget            ENGINEERING PASS_WITH_NOTES / INTEGRATED
MW-021 Narrative Scroll          PRODUCT PASS
```

Package 0 不因后续工作自动重开；只有具体 regression 才进入对应 lineage。

## Package 1｜UAT Observability / Debug Mode v0.1｜PRODUCT PASS / CLOSED

`MW-022` 提供 bounded read-only Debug Mode；`MW-023` 已确认 ordinary gameplay typography/readability。

Debug 当前覆盖 Narrative / World / Identity / Character / Experiences / People / Threads / Mechanics / Inventory / Recommendations / Save-Restore 等真实终态，不拥有第二份事实。

## Package 2｜Core Interaction Control｜ENGINEERING COMPLETE / CORE OUTCOME ACCEPTED / PRODUCT CONFIRMATION DEFERRED

Integrated:

- MW-024 OOC / GM Guidance;
- MW-025 Character-guided Recommendations + accepted-action Character evidence;
- MW-026 Package-2 bounded cleanup.

核心保护：free-form action primary；recommendations != allowed-action list；OOC != protagonist action / World mutation / d20 opportunity。

Owner 已接受核心方向，但最终集中体验确认被明确延期到 Package 7；不得虚标 Product PASS。

## Package 3｜Open Threads / 事务｜ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT CONFIRMATION DEFERRED

`MW-027` 已集成。现有 Information Curator 同一次调用维护 model-owned unresolved-matters snapshot；Program 不建设 Quest keyword/rule engine。

真实模型是否能稳定选择/保持安静/移除已解决事务，留给 Package 7 Product evidence。

## Package 4｜System / Public d20｜ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT CONFIRMATION DEFERRED

`MW-028` 已集成。Program-owned Public d20 truth 通过共享 currentness seam 投影为 `系统` history；NO_CHECK 保留为 mechanics truth/Debug/continuity，但不刷满玩家列表。

不引入第二 mechanics owner 或虚构 HP/Mana/Level 等状态。

## Package 5｜factual Inventory / 行囊｜ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT CONFIRMATION DEFERRED

`MW-029` 已集成。当前行囊是 exact accepted Narrative 支持的 factual possession state：

```text
World semantic same call
→ ADD / UPDATE / REMOVE
→ Program-owned item identity + version-bound events
→ current fold
→ 行囊 Surface + foreground grounding
```

无 authoritative Inventory event 时结构化行囊为空；不从 Character prose 猜起始装备，不硬塞默认物品。

真实模型 possession extraction / GM natural use 留给 Package 7。

## Package 6｜Internal Dynamic UI Host v0.1｜ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT CONFIRMATION DEFERRED TO PACKAGE 7

Reviewed/integrated implementation main:

`my-world/main@69ac2030b90f4165deb2ecb5302e3743422af585`

Work Item:

`MW-030｜Internal Dynamic UI Host v0.1 + Model-curated Visibility Preference`

已成立：

```text
Character / Important Experiences / People / Threads / Inventory / System
→ domain-owned player-safe DTO
→ first-party bounded definition
→ one shared Internal Dynamic UI Host
→ Godot Controls
```

Internal vocabulary 仅覆盖真实重复需求：`section / text / fact_list / field_list / card`。Definition 是 disposable presentation material，不是 gameplay/Timeline truth、查询语言或 external Mod UI protocol。

保护：

- renderer 不拿 raw/omniscient `world_state` 本地过滤；
- no arbitrary callback / NodePath / expression / SQL / OS command / Provider call / authoritative mutation from definition data;
- Narrative/composer、Save、Debug、inline d20 保持 imperative；
- generic Action Intent 继续 Deferred；
- external Source/Expansion/Mod UI declaration 后置 G8；
- 旧 MW-013 / pre-consumer declarative UI 决策已 superseded，不可执行。

Owner-approved visibility right 第一版已实现：

```text
People + Important Experiences
→ legitimate opaque presentation key
→ 隐藏 / 已隐藏(N) / 恢复显示
→ Game-local preference outside Timeline
```

隐藏不删除语义、不反馈给模型、不触发 Provider；survive reopen；Restore 不 rewind；hidden People 可更新且不会自动 unhide。Open Threads 当前无合法 stable item identity，因此不按标题/文本/位置强造 hide identity；System/Inventory 也不 generic-hide。

## Package 7｜V0 CORE CLOSURE REALITY GATE｜CURRENT

Active UAT record:

`docs/uat/G6_V0_CORE_CLOSURE_REALITY_GATE_U1.md@v1.0`

当前先执行 bounded build prep，随后由 Owner 对 exact reviewed build 做集中真实试玩。

建议自然覆盖：

- 20–30 个正常回合；
- 1 个新出现并持续相关的 NPC；
- 1 次 OOC Guidance；
- 1 次 Public d20 CHECK；
- 1 次 factual Inventory ADD + 后续 UPDATE/REMOVE；
- 1 次 Save + exit/reopen；
- 1 次 Restore；
- 1 次明显偏离推荐项的自由输入；
- 多类 Dynamic UI Surface；
- 1 次 People/Important Experience hide + recover；
- 必要时使用 Debug 解释后台 changed/no-change/failed。

本轮集中吸收 Package 2–6 延期 Product evidence。

真实闭环必须表现为：

```text
Launch / New Game / Continue
→ GM opening
→ recommendations + free-form action
→ OOC
→ durable World / NPC consequence
→ Character / People / Open Threads
→ System / d20
→ Inventory mutation
→ Internal Dynamic UI + player visibility control
→ Save / exit / reopen / Restore
→ world + information + mechanics + UI currentness 一致
→ continue play
```

**G6 Exit：只有 Owner 可明确宣布 `V0 Core Game Loop = PRODUCT PASS`。**

若 UAT 只发现 bounded defects，优先一次 correction train 后复验，不重新扩产品范围。

---

# G7｜Long-session Context & Knowledge Hardening

## Package 8｜Long-session Core

- Context Orchestrator;
- Structured Output Reliability（只针对真实 machine-schema lanes）；
- working-set/currentness/latency/long-session reality test。

原则：`相关 != 当前有效 != 当前有权使用`；`Bounded context != starved context`。

## Package 9｜Knowledge Integrity & Correction Foundation

- Provenance;
- Epistemic Status;
- Turn Freshness;
- Conflicting Evidence;
- 玩家纠正 AI 派生信息。

Reality Correction Mode 只有在 World / Inventory / NPC / Knowledge / Mechanics / Timeline authority 与 atomicity/currentness 足够稳定后再审计/实现。

---

# G8｜Product Expansion / Authoring / External Contract

## Package 10｜Information Surface Expansion

- People Shared History;
- Organization / Faction player-known Surface;
- Player-known World Chronicle;
- 完整玩家版 Consequence Diff。

## Package 11｜Player Utility / Personalization / Archive

- Narrative Preference;
- Bookmark;
- Player Notes;
- readable Adventure Chronicle export;
- Game-local Frozen Manifest。

## Package 12｜Provider / Model Operations

- Narrative / Background model separation;
- AI usage / latency / token visibility;
- Compatibility Preflight;
- Model Profiles;
- richer observability dashboard。

## Package 13｜Source Library / Reference / Creator

```text
Source Library 作品化 + Composition
→ Reference Library
→ 对话式 Creator
→ Creator Preview Sandbox
→ human-readable Validation / Publish UX
→ only then consider external Declarative UI contract
```

---

# G9｜Standalone Alpha / Release Validation

## Package 14｜Standalone Alpha

- Windows standalone packaging;
- onboarding / credentials / Source setup;
- upgrade / migration / recovery reality tests;
- long-play / corruption / reinstall validation;
- release UAT / defect closure;
- documentation / diagnostics / support boundary。

**G9 Exit：** independent user can install, create a game, play continuously, save/recover and understand real failures.

---

## Deferred / Non-scope

继续不提前建设：multiplayer/cloud account/server dependency、3D free-movement、full-universe per-NPC tick simulator、universal ECS/giant EventBus、arbitrary external code execution、giant universal Source/UI schema、automatic map generation before real evidence、generic Action Intent、external Declarative UI before internal Host + Reality Gate evidence、Visual Runtime before authored first-party demand。

Current retained engineering debt that does not block Package 7 by itself:

- two exact-baseline G3 Context assertions;
- known bounded teardown/resource warnings;
- layer-boundary/Shell decomposition debt;
- long-session Context Orchestrator and general Structured Output reliability work reserved for G7.
