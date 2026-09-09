---
title: my world｜总体规划路线图
status: current-canonical-roadmap
version: 4.6
created: 2026-08-25
updated: 2026-09-09
current_phase: G6
current_status_source: MY_WORLD_CURRENT_STATUS.md
implementation_repo: https://github.com/zhangchenjia21-dot/my-world
supersedes: v4.5
---

# my world｜总体规划路线图 CURRENT

## 0. 文档职责

本文件拥有 G1–G9 阶段顺序、当前 Core-first Package Axis、Stage Gate、Deferred / Non-scope 与排序原因。实时 PASS / blocker / current owner 以 `MY_WORLD_CURRENT_STATUS.md` 为准。

Owner 冻结路线原则：

> **优先保证游戏尽快完成完整游戏闭环。核心开发先做；扩展功能、体验优化、Creator、Reference 与其它外围增强后置。**

> **Internal Dynamic UI 是 V0 核心能力，必须在 V0 Core Closure Reality Gate 前完成。**

> **UAT observability 直接降低后续每个核心 Package 的 Owner 验收成本，因此 Debug Mode + 回合级后台变化可视化是 Package 0 关闭后的第一优先级。**

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

G1–G5 不因后续路线调整重新开启。

---

# G6｜RPG Core Closure + UAT Observability + Internal Dynamic UI

## Outcome

Owner 可以完成一局连续真实试玩，并确认：

> **自然语言自由、AI GM、Living World、人物/角色/事务/行囊/mechanics、动态 UI、Save/Restore 共同组成一个完整可靠的 V0 RPG 闭环；同时 Debug Mode 能明确告诉 Owner 每回合后台哪些域真的发生了变化、哪些没有、哪些失败。**

## G6 Core-first Package Order

### Package 0｜Correction Train + Focused Owner UAT｜PRODUCT PASS / CLOSED

Package 0 已于 2026-09-08 关闭。

Final outcomes：

```text
MW-018 R1 People                  PRODUCT PASS
MW-015 R1 Important Experiences  PRODUCT PASS
MW-019 R1 Recommendations        PRODUCT PASS
MW-020 Context Budget            ENGINEERING PASS_WITH_NOTES / INTEGRATED
MW-021 Narrative Scroll          PRODUCT PASS
```

Formal closure evidence：

`docs/uat/G6_PACKAGE0_OWNER_REUAT_U2.md@v1.2`

Reviewed/integrated closure artifact：

`my-world/main@d81f5f215360780cc50038ccd3bce7cb4163b866`

Package 0 不因后续相邻 UI 工作自动重开；只有新的具体 regression 才进入对应 lineage。

---

### Package 1｜UAT Observability / Debug Mode v0.1｜PRODUCT PASS / CLOSED

**目标：让后续核心开发每做一项，Owner 都能低成本判断“系统后台到底有没有真的变化”。**

这是 P-34 Debug Mode 与 P-32 Consequence Diff 的一个**前置 UAT 切片**，不等于提前实现完整玩家版“本回合变化”。

Executable outcome：

`MW-022｜UAT Observability / Debug Mode v0.1`

Frozen architecture：

`architecture/observability/G6_UAT_OBSERVABILITY_DEBUG_MODE_V0_1_DECISION.md`

v0.1：

```text
Debug Mode OFF
→ 普通游戏体验不变

Debug Mode ON
→ bounded read-only UAT panel
→ recent accepted Turn trace
→ Narrative / World / Identity / Character / Experiences / People / Recommendations / Save-Restore
→ changed / no-change / failed / stale / cancelled 等真实终态
→ 异常时显示人能理解的安全原因
```

保护：

- Debug UI 独立于 `概览/角色/重要经历/人物/...` 玩家信息 taxonomy；
- 默认只显示 terminal/change/Turn/Provider/Model/安全计数，不默认展开 GM-private / NPC-private 隐藏语义；
- player-visible 数据只允许 safe projection 范围内的 bounded evidence；
- Debug Mode 只读，不改变模型输入、World mutation、mechanics、推荐严格性或 currentness；
- 不输出 API Key / credential / raw provider payload / model reasoning；
- 不建设 giant EventBus / universal telemetry platform；
- 不持久化 Debug history/preferences；
- Open Threads、System、Inventory 成为真实 consumer 后接入同一 bounded observability seam。

MW-023 Gameplay Typography Readability 亦已 `PRODUCT PASS / CLOSED`，作为后续核心 Surface 的可读性基线保留。

---

### Package 2｜Core Interaction Control｜ENGINEERING COMPLETE / CORE OUTCOME ACCEPTED / PRODUCT CONFIRMATION DEFERRED

已完成并集成：

```text
MW-024  OOC / GM Guidance
MW-025  Character-guided Recommendations + accepted-action Character evidence
MW-026  bounded Package-2 UAT cleanup
```

保护：Five recommendations != allowed-action list；OOC != character action；OOC != World mutation；free-form action always primary。

Owner 已完成探索性 UAT并接受 Package-2 核心方向；MW-026 的三项小修亦已独立审核并集成。Owner 于 2026-09-09 明确决定：

> **“UAT就以后再UAT吧，这次先跳过了”**

因此当前正式状态为：

> **Package 2 = ENGINEERING COMPLETE / CORE OUTCOME ACCEPTED / PRODUCT CONFIRMATION DEFERRED**

含义：

- 不把未做的最终 spot confirmation 伪装成 Product PASS；
- 也不让这次延期阻塞 Core-first 路线；
- 不再为 Package 2 单独准备当前确认 build；
- 剩余体验确认可并入后续集中 UAT / Package 7 Reality Gate，除非 Owner 更早要求；
- 立即进入后续核心 Package。

Formal UAT record：

`docs/uat/G6_PACKAGE2_OWNER_UAT_U1.md@v1.6`

---

### Package 3｜Core Information Continuity｜事务 / Open Threads｜ENGINEERING PASS_WITH_NOTES / INTEGRATED / PRODUCT CONFIRMATION DEFERRED

**目标：让玩家知道当前有哪些还没有真正结束、但值得继续记住和跟进的事情。**

Frozen architecture：

`architecture/ui/G6_OPEN_THREADS_SURFACE_V1_0_DECISION.md@v1.0`

Reviewed implementation：

`MW-027｜Open Threads / 事务`

- Independent Review：**ENGINEERING PASS_WITH_NOTES**；
- reviewed/integrated implementation main：`my-world/main@5a336d0a993fd7e91b05b98f9b0cc14d2bb47b21`；
- real Provider semantic sample：0，因此真实模型是否能稳定挑出“值得继续记住的未完事项”仍属于 Product evidence；
- Owner 当前已决定集中 UAT，故 standalone Product confirmation deferred，不阻塞路线。

已成立 vertical：

```text
existing Post-turn Information Curator
→ model decides add / update / keep / remove
→ existing information_curation durable owner
→ player-safe current Open Threads projection
→ 事务 Surface
→ Debug threads changed / no-change / failed
→ Save / reopen / Restore / Regenerate currentness
```

保护：

- Model owns semantic interpretation；Program 不建设 Quest keyword/rule engine；
- `事务` 是当前未完事项快照，不是任务历史；
- 普通回合可以 no-change；已解决/失效/不再重要的事项可以从当前快照移除；
- 不新增独立 Open Threads Provider call；
- 不读取 raw World / NPC private truth 来“补全任务”；
- 不新建 SQLite table / 第二事实源；
- v1.0 不做 checkbox、手动完成/编辑、搜索筛选、优先级、Quest 奖励或 generic Action Intent。

---

### Package 4｜Core Mechanics Visibility｜System / Public d20｜CURRENT

**目标：把已经真实发生、已经公开给玩家的 Program-owned Public d20 判定，从 Narrative 的即时骰点卡扩展成一个可持续回看的 `系统` 信息 Surface。**

Frozen architecture：

`architecture/ui/G6_SYSTEM_PUBLIC_MECHANICS_SURFACE_V1_0_DECISION.md@v1.0`

当前 executable outcome：

`MW-028｜System / Public d20 Surface`

核心 vertical：

```text
existing Public d20 durable owner
→ shared current accepted mechanics selection
→ bounded player-safe structural projection
→ 系统 Surface
→ Debug mechanics terminal evidence
→ Save / reopen / Restore currentness
```

v1.0 保护：

- `系统` 只投影真实 mechanics truth，不创建第二事实源；
- 玩家 Surface 只列最近 accepted/current 的真实 `CHECK`，最多 12 条；
- 普通 `NO_CHECK` 不长期堆入玩家列表，但仍保留为 durable mechanics truth、GM continuity 与 Debug terminal evidence；
- 不通过 Information Curator，不增加 Provider call；
- 不新建 SQLite owner；
- 不重做 d20 balance/DC/modifier/stance/RNG/no-reroll 规则；
- 不硬编码虚构 HP / Mana / Hunger / Money / Level / Buff 等状态；
- 保留 Narrative inline dice card；
- World Information Host 导航变为 `概览 | 角色 | 重要经历 | 人物 | 事务 | 系统 | 存档`；
- 不为历史骰点重新打开 Player Status Host；
- Debug Mode 接入 `mechanics` terminal/change evidence；
- >=20px typography、Save/Restore/currentness 与 privacy boundary 继续成立。

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

不提前建设装备槽、loot、crafting、economy、durability 或复杂 stack framework。Debug Mode 显示 Inventory 是否真实变化。

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
- Debug Mode 对关键回合变化/异常提供足够 UAT 证据；
- Package 2 延期的 OOC/Public-d20/Recommendation Product confirmation 可在这里一并覆盖；
- Package 3 延期的 Open Threads 模型语义质量可在连续试玩中一并覆盖。

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

Package 1 只前移 UAT/debug 用“域是否变化 + safe bounded evidence”切片；完整玩家体验仍在 G8 成熟。

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

Debug Mode 核心 v0.1 已前移 Package 1；这里仅成熟它，不重新造第二套诊断系统。

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

未通过的提案不进入路线，除非未来 Owner 明确重新开启。