---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 4.2
created: 2026-08-25
updated: 2026-09-07
current_phase: G6
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
supersedes: v4.1
---

# my world｜总体规划路线图 CURRENT

## 0. 文档职责

本文件拥有 G1–G9 阶段顺序、当前 Core-first Task Axis、Stage Gate、Deferred / Non-scope 与排序原因。实时 PASS / blocker / current owner 以 `MY_WORLD_CURRENT_STATUS.md` 为准。

Owner 于 2026-09-07 冻结两条最高路线原则：

> **优先保证游戏尽快完成完整游戏闭环。核心开发先做；扩展功能、体验优化、Creator、Reference、模型管理、诊断与外围增强后置。**

> **Internal Dynamic UI 是 V0 核心能力，必须在 V0 Core Closure Reality Gate 前完成。**

继续遵守：Vertical before platform；Consumer before Creator；真实需求 → 最小能力 → 真实 consumer → Owner UAT → 再抽象。

---

## 1. 总体阶段

```text
G1 Foundation & Project Bootstrap                PASS / CLOSED
↓
G2 AI Conversation Spine                         PASS / CLOSED
↓
G3 Persistent Game / Save / Timeline Foundation PASS / CLOSED
↓
G4 Primary Source Assets & Local Game Creation   PASS / CLOSED
↓
G5 World Semantics & GM Runtime                  PRODUCT PASS / CLOSED
↓
G6 RPG Core Closure + Internal Dynamic UI        ACTIVE
↓
G7 Long-session Context & Knowledge Hardening
↓
G8 Product Expansion / Authoring / External Contract
↓
G9 Standalone Alpha / Release Validation
```

已成立技术脊柱：

```text
Launch
→ Main Menu
→ Continue / New Game
→ AI GM free-form Narrative
→ Player natural-language action
→ durable world / actor consequences
→ Save / exit / reopen
→ Continue / Restore
→ coherent world + context recovery
```

G6 的任务是把这条技术脊柱收敛成**完整可长期玩的 V0 RPG 产品闭环**。

---

# G1–G5｜CLOSED

## G1 Foundation
Godot 4.7.2 / GDScript / same-process Runtime / Windows-local Host / Provider streaming / local IO / Windows export 成立。

## G2 AI Conversation Spine
自然语言输入、真 streaming Narrative、多回合、Cancel / Regenerate / Retry / provider-failure recovery 成立。

## G3 Persistence / Save / Timeline
One authoritative SQLite flow、atomic mutation、accepted Conversation durability、Timeline、Save、Restore、future-memory isolation、backup/recovery 成立。

## G4 Source / Game Creation
Managed Source Library → World / Character / Expansion exact Composition → Atomic Final Create → independent Game-local Reality → multiple Games → real AI GM play 成立。

## G5 Living World
free-form Narrative → durable semantic consequences；World Truth != actor Knowledge != human-player disclosure；stable NPC Agency；World Evolution；Public d20 mechanics grounding；player-safe projection 均已 Product PASS。

---

# G6｜RPG Core Closure + Internal Dynamic UI

## Outcome

Owner 可以完成一局连续真实试玩，并确认：

> **自然语言自由、AI GM、Living World、人物/角色/事务/行囊/mechanics、动态 UI、Save/Restore 共同组成一个完整可靠的 V0 RPG 闭环。**

G6 不再以“继续增加很多页面”作为完成标准。

## G6 Core-first Package Order

### Package 0｜MW-018 + MW-019 Combined Owner UAT｜CURRENT

- MW-018 People：Engineering PASS / Integrated / Owner UAT pending；
- MW-019 Five Recommended Actions：Engineering PASS / Integrated / Owner UAT pending。

先关闭当前 Gate。真实 UAT defect 留在各自 lineage；不借机建设通用 framework。

**Exit：** MW-018 / MW-019 分别获得 Owner Product verdict。

---

### Package 1｜Core Interaction Control

包含：

- OOC / GM Guidance；
- Character-guided Recommendations；
- Player 最终 accepted action 反过来作为 Character evolution evidence。

保护：

```text
Five recommendations != allowed-action list
OOC != character action
OOC != World mutation
free-form action always primary
```

---

### Package 2｜Core Information Continuity｜事务 / Open Threads

让玩家知道当前未解决的问题、线索、承诺、计划和风险。

优先扩展 Information Curator；不建设 Quest keyword/rule engine。

---

### Package 3｜Core Mechanics Visibility｜System Surface

以现有 Public d20 为第一真实 mechanics consumer：

```text
real mechanic owner
→ bounded player-safe contribution
→ System Surface
```

不硬编码虚构 HP / Mana / Hunger / Money。

---

### Package 4｜Core Inventory Vertical｜事实型行囊

先冻结最小 Inventory owner / mutation contract，再证明：

```text
初始真实物品
→ 一次使用 / 转交 / 失去
→ durable state
→ Inventory Surface
→ Save / Restore / reopen 一致
```

不提前建设装备槽、loot、crafting、economy、durability 或复杂 stack framework。

---

### Package 5｜Internal Dynamic UI Host v0.1｜CORE REQUIRED

**Owner 明确要求：V0 Core Closure 前必做。**

进入时已经拥有多个真实 consumer：Character、Important Experiences、People、Open Threads、System/Public d20、Inventory。

从这些 production consumer 中抽象有限 internal UI vocabulary，例如 section/group、field、card/list、collapsed region、status/mechanic contribution 与已证明的 safe navigation。

保护：

- Dynamic UI 只负责 presentation，不拥有 gameplay truth；
- renderer 不接收 omniscient world_state 后本地过滤；
- Restore / Regenerate 后随 player-safe projection currentness 回退；
- 不允许 arbitrary GDScript callback / NodePath / OS command / 任意 authoritative mutation；
- generic Action Intent 继续 Deferred；
- external Source / Expansion UI declaration 继续后置 G8；
- 旧 `MW-013` Task Packet 不可直接执行，必须基于新增真实 consumers 重新 Task Shape。

**Exit：** 多个真实 Surface / mechanic contribution 由同一 Internal Dynamic UI Host 正确呈现，且无第二事实源、泄密或 currentness 回归。

---

### Package 6｜V0 CORE CLOSURE REALITY GATE

Owner 使用真实 build 连续试玩，至少覆盖：

- 20–30 个正常回合；
- 1 个新出现 NPC；
- 1 次 OOC Guidance；
- 1 次 Public d20；
- 1 次真实 Inventory mutation；
- 1 次 Save / reopen；
- 1 次 Restore；
- 1 次明显偏离推荐项的自由输入；
- 至少 3 类 Dynamic UI Host 承载的真实 Surface / contribution。

闭环必须表现为：

```text
Launch / New Game / Continue
→ GM opening
→ recommendations + free-form action
→ OOC
→ durable World / NPC consequence
→ Character / People / Open Threads
→ System / d20
→ Inventory mutation
→ Internal Dynamic UI presentation
→ Save / exit / reopen / Restore
→ world + information + mechanics + UI currentness 一致
→ continue play
```

**G6 Exit：Owner 明确认定 `V0 Core Game Loop = PRODUCT PASS`。**

只有阻塞这一闭环的真实 defect 可以在 Gate 前插队。

---

# G7｜Long-session Context & Knowledge Hardening

## Outcome

V0 闭环已经成立后，保证“玩久了仍然成立”，并让长期玩家信息可追溯、可纠正。

### Package 7｜Long-session Core

- Context Orchestrator；
- Structured Output Reliability（只收敛真实需要的 machine-schema lanes）；
- working-set / currentness / latency / long-session reality test。

原则：

```text
相关 != 当前有效 != 当前有权使用
Bounded context != starved context
```

不先建设通用 RAG platform。

### Package 8｜Knowledge Integrity & Correction Foundation

合并：

- Provenance；
- Epistemic Status；
- Turn Freshness（只显示第几个回合 / accepted-history node）；
- Conflicting Evidence；
- 玩家纠正 AI 派生信息。

随后做 Reality Correction Mode Architecture Audit；只有 World / Inventory / NPC / Knowledge / Mechanics / Timeline authority、atomicity、currentness 冻结后才允许实现：

```text
角色行动 | OOC | 世界纠错
```

---

# G8｜Product Expansion / Authoring / External Contract

## Outcome

在 V0 Core + long-session foundation 成立后，再增加丰富信息、玩家工具、AI 管理和 Creator；外部 UI contract 必须从已证明的 Internal Dynamic UI vocabulary 派生。

### Package 9｜Information Surface Expansion

- People Shared History；
- Organization / Faction player-known Surface；
- Player-known World Chronicle；
- Player-visible Consequence Diff。

优先复用 Dynamic UI Host 和统一 player-safe information model。

### Package 10｜Player Utility / Personalization / Archive

- Narrative Preference；
- Bookmark；
- Player Notes；
- readable Adventure Chronicle export；
- Game-local Frozen Manifest。

### Package 11｜Provider / Model / Observability

- player-safe generation status / diagnostics；
- Narrative / Background model separation；
- AI usage / latency / token visibility；
- Compatibility Preflight；
- Model Profiles；
- Debug Mode。

Core 阶段若出现真实 debugging blocker，只拉出最小 observability seam，不整体前移。

### Package 12｜Source Library / Reference / Creator

顺序：

```text
Source Library 作品化 + Composition
→ Reference Library
→ 对话式 Creator
→ Creator Preview Sandbox
→ 人话化 Validation / Publish UX
→ only then consider external Declarative UI contract
```

不建设 arbitrary-code plugin platform、在线商店、云账号或 speculative universal package manager。

---

# G9｜Standalone Alpha / Release Validation

### Package 13｜Standalone Alpha

- Windows standalone packaging；
- onboarding / credentials / Source setup；
- upgrade / migration / recovery reality tests；
- long-play / corruption / reinstall validation；
- release UAT / defect closure；
- documentation / diagnostics / support boundary。

**G9 Exit：** 独立用户能安装、建局、持续游玩、保存恢复，并在真实失败后得到可理解路径。

---

## Deferred / Non-scope

继续不提前建设：

- multiplayer / cloud account / server dependency；
- 3D free-movement world；
- full-universe per-NPC tick simulator；
- universal ECS / giant EventBus；
- arbitrary external code execution；
- giant universal Source/UI schema；
- automatic map generation before real evidence；
- generic Action Intent before proven need；
- external Declarative UI before Internal Dynamic UI production evidence；
- Visual Runtime before authored first-party demand。

本轮未通过的提案不进入路线，除非未来 Owner 明确重新开启。
