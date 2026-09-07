---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 4.3
created: 2026-08-25
updated: 2026-09-07
current_phase: G6
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
supersedes: v4.2
---

# my world｜总体规划路线图 CURRENT

## 0. 文档职责

本文件拥有 G1–G9 阶段顺序、当前 Core-first Task Axis、Stage Gate、Deferred / Non-scope 与排序原因。实时 PASS / blocker / current owner 以 `MY_WORLD_CURRENT_STATUS.md` 为准。

Owner 于 2026-09-07 冻结以下路线原则：

> **优先保证游戏尽快完成完整游戏闭环。核心开发先做；扩展功能、体验优化、Creator、Reference 与其它外围增强后置。**

> **Internal Dynamic UI 是 V0 核心能力，必须在 V0 Core Closure Reality Gate 前完成。**

> **UAT observability 直接降低后续每个核心 Package 的 Owner 验收成本，因此 Debug Mode + 回合级后台变化可视化前移到当前 UAT Gate 关闭后的第一优先级。**

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
G6 RPG Core Closure + UAT Observability + Internal Dynamic UI ACTIVE
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

G6 的任务是把这条技术脊柱收敛成**完整、可观察、可长期试玩验证的 V0 RPG 产品闭环**。

---

# G1–G5｜CLOSED

G1–G5 的 Foundation、Conversation、Persistence/Timeline、Source/Game Creation、Living World 均已关闭；当前不因路线重排重新开启。

---

# G6｜RPG Core Closure + UAT Observability + Internal Dynamic UI

## Outcome

Owner 可以完成一局连续真实试玩，并确认：

> **自然语言自由、AI GM、Living World、人物/角色/事务/行囊/mechanics、动态 UI、Save/Restore 共同组成一个完整可靠的 V0 RPG 闭环；同时 Debug Mode 能明确告诉 Owner 每回合后台哪些域真的发生了变化、哪些没有、哪些失败。**

## G6 Core-first Package Order

### Package 0｜MW-018 + MW-019 Combined Owner UAT｜CURRENT

- MW-018 People：Engineering PASS / Integrated / Owner UAT active；
- MW-019 Five Recommended Actions：Engineering PASS / Integrated / Owner UAT active。

Owner 继续正常试玩并累计 finding。真实 defect 留在各自 lineage；MW-019 当前已出现推荐选项文本过长/拥挤与 UI 尺寸偏小的产品 finding，最终 correction 在本轮 UAT 收束后统一 Task Shape。

**Exit：** MW-018 / MW-019 分别获得 Owner Product verdict；若需 revision，完成 bounded correction + focused re-UAT 后关闭 Package 0。

---

### Package 1｜UAT Observability / Debug Mode｜CORE UAT LEVERAGE

**目标：让后续核心开发每做一项，Owner 都能低成本判断“系统后台到底有没有真的变化”。**

这是 P-34 Debug Mode 与 P-32 Consequence Diff 的一个**前置 UAT 切片**，不等于提前实现完整玩家版“本回合变化”。

v0.1 至少提供：

```text
Debug Mode OFF
→ 与普通模式体验一致

Debug Mode ON
→ 每个 accepted Turn 显示 bounded UAT trace
→ domain terminal state + changed / no-change / failed / stale / cancelled
→ 异常时自动给出人能理解的错误原因
```

第一批应覆盖当前已经存在的真实 lane / owner，例如：

- Narrative accepted / failed；
- World semantic changed / no-change / failed / stale；
- actor/NPC materialization / identity bridge terminal；
- Character changed / no-change；
- Important Experiences changed / no-change；
- People changed / no-change；
- Recommendations success / malformed / unavailable / stale；
- Save / Restore / currentness 关键结果。

之后 Open Threads、System/mechanics、Inventory 完成时接入同一只读诊断投影，不为每个新模块再造一套日志 UI。

保护：

- 默认只显示“是否变化 + terminal state + Turn / Provider / Model 等必要诊断”，**不默认展开 GM-private / NPC-private 的隐藏语义内容**，避免 UAT 被剧透；
- player-visible 结构可按需显示 safe diff；
- Debug Mode 只读，不成为第二事实源，不改变模型输入、判定、world mutation 或 currentness；
- 不输出 API Key / credential / 无关本机隐私；
- 不建设 giant EventBus / universal telemetry framework；优先聚合现有 domain terminal evidence。

**Exit：** Owner 能在一次真实回合后快速判断关键后台域是否变化，并在异常时看到具体失败原因，而无需手工查日志/数据库。

---

### Package 2｜Core Interaction Control

- OOC / GM Guidance；
- Character-guided Recommendations；
- Player 最终 accepted action 反过来作为 Character evolution evidence。

保护：Five recommendations != allowed-action list；OOC != character action；OOC != World mutation；free-form action always primary。

---

### Package 3｜Core Information Continuity｜事务 / Open Threads

让玩家知道当前未解决的问题、线索、承诺、计划和风险。优先扩展 Information Curator；不建设 Quest keyword/rule engine。

Package 1 Debug Mode 同步显示本回合 `Open Threads changed / no-change / failed`。

---

### Package 4｜Core Mechanics Visibility｜System Surface

以现有 Public d20 为第一真实 mechanics consumer：

```text
real mechanic owner
→ bounded player-safe contribution
→ System Surface
```

不硬编码虚构 HP / Mana / Hunger / Money。Package 1 Debug Mode 同步接入 mechanics terminal/change evidence。

---

### Package 5｜Core Inventory Vertical｜事实型行囊

先冻结最小 Inventory owner / mutation contract，再证明：

```text
初始真实物品
→ 一次使用 / 转交 / 失去
→ durable state
→ Inventory Surface
→ Save / Restore / reopen 一致
```

不提前建设装备槽、loot、crafting、economy、durability 或复杂 stack framework。Package 1 Debug Mode 同步显示 Inventory 是否真实变化。

---

### Package 6｜Internal Dynamic UI Host v0.1｜CORE REQUIRED

进入时已有多个真实 consumer：Character、Important Experiences、People、Open Threads、System/Public d20、Inventory。

从 production consumer 中抽象有限 internal UI vocabulary，例如 section/group、field、card/list、collapsed region、status/mechanic contribution 与已证明的 safe navigation。

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

### Package 7｜V0 CORE CLOSURE REALITY GATE

Owner 使用真实 build 连续试玩，至少覆盖：

- 20–30 个正常回合；
- 1 个新出现 NPC；
- 1 次 OOC Guidance；
- 1 次 Public d20；
- 1 次真实 Inventory mutation；
- 1 次 Save / reopen；
- 1 次 Restore；
- 1 次明显偏离推荐项的自由输入；
- 至少 3 类 Dynamic UI Host 承载的真实 Surface / contribution；
- Debug Mode 对关键回合变化/异常提供足够 UAT 证据。

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

---

# G7｜Long-session Context & Knowledge Hardening

### Package 8｜Long-session Core

- Context Orchestrator；
- Structured Output Reliability（只收敛真实需要的 machine-schema lanes）；
- working-set / currentness / latency / long-session reality test。

原则：`相关 != 当前有效 != 当前有权使用`；`Bounded context != starved context`。

### Package 9｜Knowledge Integrity & Correction Foundation

合并 Provenance、Epistemic Status、Turn Freshness（只显示第几个回合 / accepted-history node）、Conflicting Evidence、玩家纠正 AI 派生信息。

随后做 Reality Correction Mode Architecture Audit；只有 World / Inventory / NPC / Knowledge / Mechanics / Timeline authority、atomicity、currentness 冻结后才允许实现：

```text
角色行动 | OOC | 世界纠错
```

---

# G8｜Product Expansion / Authoring / External Contract

### Package 10｜Information Surface Expansion

- People Shared History；
- Organization / Faction player-known Surface；
- Player-known World Chronicle；
- **完整玩家版** Player-visible Consequence Diff。

注意：Package 1 只前移 UAT/debug 用的“域是否变化 + safe diff”切片；完整玩家体验仍在此成熟。

### Package 11｜Player Utility / Personalization / Archive

- Narrative Preference；
- Bookmark；
- Player Notes；
- readable Adventure Chronicle export；
- Game-local Frozen Manifest。

### Package 12｜Provider / Model Operations

- Narrative / Background model separation；
- AI usage / latency / token visibility；
- Compatibility Preflight；
- Model Profiles；
- richer observability dashboard。

Debug Mode 核心 v0.1 已前移 Package 1；这里仅做其后续成熟，不重新造第二套诊断系统。

### Package 13｜Source Library / Reference / Creator

```text
Source Library 作品化 + Composition
→ Reference Library
→ 对话式 Creator
→ Creator Preview Sandbox
→ 人话化 Validation / Publish UX
→ only then consider external Declarative UI contract
```

---

# G9｜Standalone Alpha / Release Validation

### Package 14｜Standalone Alpha

- Windows standalone packaging；
- onboarding / credentials / Source setup；
- upgrade / migration / recovery reality tests；
- long-play / corruption / reinstall validation；
- release UAT / defect closure；
- documentation / diagnostics / support boundary。

**G9 Exit：** 独立用户能安装、建局、持续游玩、保存恢复，并在真实失败后得到可理解路径。

---

## Deferred / Non-scope

继续不提前建设：multiplayer / cloud account / server dependency、3D free-movement、full-universe per-NPC tick simulator、universal ECS / giant EventBus、arbitrary external code execution、giant universal Source/UI schema、automatic map generation before real evidence、generic Action Intent、external Declarative UI before Internal Dynamic UI production evidence、Visual Runtime before authored first-party demand。

本轮未通过的提案不进入路线，除非未来 Owner 明确重新开启。
