---
title: my world｜G6 Surface / Information Architecture Draft v0.2
status: DRAFT / FOR OWNER DISCUSSION
version: 0.2
created: 2026-09-06
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
semantic_owner: GPT + Owner
historical_reference: zhangchenjia21-dot/the-world
---

# G6 Surface / Information Architecture Draft v0.2

> **DRAFT — NOT FROZEN — NOT IMPLEMENTATION AUTHORITY**

本文件用于 Owner + GPT 讨论 `my world` 的三栏职责与右侧 RPG Surface 分类。它吸收 `the-world` 真实试玩形成的 UI 经验，但不机械复制旧 DSH/Markdown 实现。

v0.2 根据 Owner 2026-09-06 明确纠正：**左侧 Player Host 不承担“我是谁”的信息架构职责；它长期应是角色立绘 + 当前角色状态 / mechanics HUD 的 presentation host。完整身份、背景、性格、能力、局限等 Character Sheet 信息属于右侧 `角色` Surface。**

## 1. Historical evidence from The World

旧 `the-world-panel` 在真实试玩后明确形成一条产品原则：

> **Workspace is organized for truth maintenance; UI is organized for player decisions.**
>
> 后台事实按 Owner 组织；玩家 UI 按玩家要回答的问题组织。

旧面板最终实际采用过的玩家分页：

```text
概览
角色
人物
行囊
事务
系统
存档
```

其长期 UI projection 经验大致为：

```text
角色 HUD      ← Player / mechanic current state
时间/地点     ← current scene truth
Journal       ← open threads
人物图鉴      ← durable characters
势力界面      ← organizations
地图          ← authored world map + place truth
系统/机制     ← mechanic runtime state
关系界面      ← relationship state
历史时间线    ← story/timeline
存档界面      ← save points / restore
```

这些结论是产品证据，不是当前 `my world` 的字段/存储实现合同。

## 2. Current my-world product skeleton

桌面长期骨架保持：

```text
Player Host | Narrative Host | World Surface Host
```

三栏职责修订为：

```text
Player Host
→ 角色立绘 + 当前角色状态 / mechanics HUD
→ 展示“现在的角色状态”，不承担完整 Character identity/profile

Narrative Host
→ 现在发生了什么？我接下来想做什么？

World Surface Host
→ 我是谁 / 这个世界里有哪些值得我主动查看的信息？
→ Character / People / Journal / Inventory / System / Map / Save...
```

正式原则：

> **Canonical truth ownership != player information architecture.**
>
> **UI is a projection, not a second truth source.**
>
> **Core Host owns placement; real Domain / Expansion owns state semantics.**

## 3. Draft responsibility split

### 3.1 Player Host — Portrait + live status HUD

Owner 当前方向：左栏长期不承担“我是谁”的文本信息架构职责。

Player Host 更准确的产品职责是：

> **角色的可视锚点 + 当前可玩状态的高频 HUD。**

长期候选内容：

```text
角色立绘 / portrait             # Character authored visual，存在才显示

角色状态 / mechanics contributions
├─ 生命值 / 魔力值               # 只有相应 Expansion / Domain 存在后
├─ 力量 / 敏捷 / 体力等属性       # 同上，不由 Core 虚构
├─ 伤势 / Buff / Debuff           # 同上
├─ 饥饿 / 疲劳 / 生存状态         # 同上
├─ 资源 / 能量 / 特殊机制摘要     # 同上
└─ 其它高频 current-status widget # 由已经证明的真实 mechanic-state consumer 拉出
```

不属于左栏长期职责：

```text
姓名 / 年龄 / 来历 / 身份叙述
背景 / 性格 / 能力说明 / 局限
目标 / 原则 / Biography
World / Entry identity
recent actions / player-turn count
Player-known world facts
完整装备 / 行囊明细
```

这些信息应进入右侧对应 Surface，而不是为了填满左栏而常驻显示。

重要边界：

```text
Player Host slot exists
!= Core owns HP / MP / attributes / injuries / inventory
```

Core 只提供稳定 placement / layout / safe contribution seam；具体角色状态必须来自正式 Domain 或 Expansion mechanic state。

如果当前 Game 没有 portrait，也没有任何合法 status contribution，**允许 Player Host 为空、极简或折叠**；不得拿 Character biography / world metadata 填空。

当前 MW-011 把完整 `player_profile` 放左栏，是为修复“面板有面积但完全没信息”的过渡实现，Owner 已接受其阶段性价值，但当前 IA 已明确：

```text
current rich left panel = ACCEPTED TRANSITIONAL IMPLEMENTATION
long-term Player Host   = portrait + live status/mechanics HUD
```

是否在第一个 `角色` Surface 就立即搬空左栏，还是等合法 portrait/status consumer 出现后再完成物理迁移，仍待 Owner 讨论。

### 3.2 Narrative Host — primary play surface

继续保持全产品视觉中心：

- GM Narrative；
- streaming；
- Player natural-language Composer；
- Cancel / Regenerate / Retry 等低风险 turn actions。

不把 Character Sheet、World Surface 导航或 mechanics dashboard 塞进中央 Narrative。

### 3.3 World Surface Host — Player information browser

右侧承担**主动查询 / 详情 / 管理型 RPG 信息架构**。

它既可以回答“我是谁”，也回答“世界里有哪些值得主动查看的信息”。

初版母版继承 The World 已验证分类，但只显示有真实 Domain / player-safe projection 的 Surface：

```text
概览
角色
人物
事务
行囊
系统
地图
存档
```

不是所有 Surface 都必须现在存在，也不要求永远保持这 8 个一级入口。

## 4. Candidate Surface matrix

| Surface | 玩家问题 | 当前 Domain / Projection 成熟度 | Draft disposition |
|---|---|---:|---|
| 概览 | 这一刻世界与局势有哪些值得快速了解的信息？ | 高 | 已存在，继续保留 |
| 角色 | 我是谁？我的完整角色资料是什么？ | 高 | **建议第一个新真实 Surface** |
| 人物 | 我已经知道/遇见了哪些人？他们现在怎样？ | 中 | 建议第二候选；先审 player-safe actor projection |
| 事务 | 还有什么承诺、线索、未解决问题？ | 低 | Deferred，当前无正式 Thread/Journal Domain |
| 行囊 | 我此刻真正拥有什么？ | 低 | Deferred，当前无正式 Inventory Runtime Domain |
| 系统 | 本局启用了什么机制？机制当前怎样？ | 中 | 跟随 G6 Expansion mechanic-state consumer；其中高频 current-status 可贡献到左栏 HUD |
| 地图 | 我在哪里？空间/区域如何理解？ | 低 | Deferred；无 spatial authority，视觉 resolver 也 deferred |
| 存档 | 如何保存、恢复、理解历史节点？ | 高 | 已存在；继续保留并逐步完善 |

## 5. Draft: first new Surface = 角色 / Character Sheet

理由：

1. `player_profile` 已有完整、player-safe、Game-local frozen projection；
2. Owner 明确要求“我是谁”类信息归右侧，而非左侧 HUD；
3. 不需要新增 Runtime truth；
4. 可以第一次把 **Character identity/profile** 与 **live mechanic/status HUD** 分开；
5. 这是旧 The World 已真实验证的 IA 方向。

Draft `角色` Surface 可包含：

```text
角色
├─ 基本身份
│  ├─ 姓名
│  ├─ 年龄 / 性别（若 authored）
│  ├─ 当前/起始身份描述
│  └─ Character / T0 profile summary
├─ 背景
├─ 性格
├─ 能力 / 专长说明       # authored capability description，不等于 numeric mechanic stats
├─ 局限
├─ 初始目标 / 长期原则
├─ 起始携带物             # authored starting facts，不冒充 dynamic Inventory
└─ 其它低频 Character reference material（只来自 player-safe projection）
```

必须区分：

```text
Character authored capability description
!= Expansion-owned numeric/stat state

Character authored starting possessions
!= Runtime Inventory current possessions
```

例如张琛的“体格强健 / 徒手格斗基本功”属于 Character Sheet；未来 Expansion 提供的 `力量 14 / HP 23/30` 属于 live mechanic state，优先展示在左栏 HUD，并可在对应 System/detail surface 展开。

## 6. Draft: 人物 Surface

长期方向：不是简单 NPC 名单，而是**关系化人物视图**。

候选玩家可见信息：

```text
显示名
当前公开/已知状态
最后已知位置        # 只有 player-known evidence 支撑时
所属/阵营           # 同上
与主角关系          # 只有正式 relationship/domain 或安全派生时
最近确认/最近互动
可展开的已知人物详情
```

人物页不得直接展示：

- actor-private Knowledge；
- GM agenda / red line / independent next move；
- omniscient world truth；
- Source private/reference prose；
- internal actor IDs / fingerprints。

在 Relationship/Faction Domain 尚不成熟时，`人物` 可以先吸收“关系/所属”的安全展示，不急于平铺独立 `关系` / `势力` 一级 Tab。

## 7. Why some old The World surfaces stay deferred

### 行囊

旧 The World 可以从 `PLAYER + mechanics inventory-like sections` 做跨 Owner 聚合；当前 `my world` 尚没有等价 authoritative Inventory Domain。

所以暂不实现动态行囊。

角色卡里的起始携带物可以作为 Character authored reference 留在 `角色` Surface，但不得声称等于当前 Inventory。

### 事务

旧 The World 有 `THREADS.md` 作为明确 open-thread owner；当前 `my world` 没有正式 Journal/Thread Domain。

所以不创建空“任务/事务”页，也不从 Narrative 关键词硬猜 Quest。

### 地图

旧 The World 已证明长局空间认知会成为真实痛点，但最薄地图也需要 authored map/current location 等真实消费者。

当前 G6 Visual Runtime re-entry 已裁定 implementation deferred，因此 Map 继续等待真实需求。

### 系统 / mechanic state

旧 The World 的“系统”只有在本局真的有长期 mechanic state 时才出现。

当前 `my world` 同样保持：

```text
no durable mechanic state
→ no fake System page
→ no fake left-HUD stats
```

未来 Expansion mechanic-state consumer 可同时拥有两种 presentation：

```text
high-frequency current state → Player Host HUD contribution
full mechanic detail         → System Surface
```

二者都只是同一 authoritative mechanic state 的不同 projection，不是两套状态。

## 8. Save / Timeline draft grouping

当前倾向：

```text
存档
├─ Save Points
├─ Restore
└─ Timeline / history secondary view   # 后续
```

暂不把 Timeline 自动提升为一级 Surface。

原因：Runtime Timeline Node 是 persistence authority，不代表每个节点都是玩家可点击恢复点。

## 9. Surface / HUD appearance rule

一个 Surface 或 HUD contribution 进入产品至少要满足：

```text
real player question
+ real domain owner
+ player-safe projection
+ non-trivial product value
```

禁止：

```text
为了“像 RPG”而创建空 Tab
为了填左栏先造 fake HP / MP / stats / injury / inventory
从 authored Character prose 推导 numeric mechanic state
从 omniscient world_state 到 leaf UI 再过滤
从 raw Source semantic prose 直接拼玩家页面
```

## 10. Proposed G6 sequence — discussion draft

```text
MW-011 Player Host / ViewModel
PRODUCT PASS / CLOSED
↓
G6 Surface / IA convergence
CURRENT DISCUSSION
↓
Character Surface
→ establish “identity/profile belongs right”
→ decide timing of removing transitional profile/world/session text from left
↓
People Surface
+ player-safe known-actor projection as needed
↓
Expansion mechanic-state consumer
→ System Surface full detail
→ Player Host receives only real high-frequency status contribution
↓
choose next grounded Surface from real evidence
(事务 / 行囊 / 势力 / Timeline / Map — not precommitted)
↓
repeated real component patterns exist
↓
MW-013 Internal Declarative UI Host v0.1
↓
bounded Action Intent
↓
responsive / Theme / navigation / final G6 polish
```

Visual asset runtime can re-enter when a real authored portrait / scene / map consumer exists. Character portrait is the most natural future Player Host visual consumer, but no resolver is built merely to fill the slot.

## 11. Owner discussion questions — updated

v0.2 已明确接受以下 Owner direction as discussion anchor：

```text
Player Host = portrait + live role/mechanics status HUD
“我是谁”      = right-side Character Surface
fake stats     = forbidden
```

下一轮重点讨论：

1. `角色` Surface 的一级结构是否采用：基本身份 / 背景 / 性格 / 能力说明 / 局限 / 目标原则 / 起始携带物？
2. Character Sheet 是否还应显示 `当前已知事实`，还是该信息只留在 `概览` / 未来 Journal/Knowledge？
3. 在合法 portrait/status contribution 尚不存在时，左栏是**折叠/缩窄**，还是暂时保留 MW-011 transitional content 直到第一项 status consumer 到来？
4. `人物` 是否先承担关系/所属信息，而不是现在就拆 `关系`、`势力` 两个一级 Tab？
5. Save 与 Timeline 是否接受一级 `存档` + 二级 Timeline 的方向？

Owner 讨论完成前，本文件保持 DRAFT，不生成对应 implementation Task。
