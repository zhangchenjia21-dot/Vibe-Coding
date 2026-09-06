---
title: my world｜G6 Surface / Information Architecture Draft v0.4
status: DRAFT / FOR OWNER DISCUSSION
version: 0.4
created: 2026-09-06
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
semantic_owner: GPT + Owner
historical_reference: zhangchenjia21-dot/the-world + zhangchenjia21-dot/sillytavern
---

# G6 Surface / Information Architecture Draft v0.4

> **DRAFT — NOT FROZEN — NOT IMPLEMENTATION AUTHORITY**

本文件用于 Owner + GPT 讨论 `my world` 的三栏职责与右侧 RPG Surface 分类。它吸收 `the-world` 与 SillyTavern 已验证的 UI 经验，但不机械复制旧宿主实现。

当前已确认的 Owner 方向：

1. **左侧 Player Host 不承担“我是谁”**；长期定位是角色立绘 + 当前角色状态 / mechanics HUD。
2. **右侧 `角色 / Character Sheet` 是会随游戏发展持续更新的完整当前角色页**，回答“现在的我是谁”。
3. **`重要经历 / Important Experiences` 是独立于 `角色` 的一级 Surface**，按时间记录真正改变主角人生轨迹的重大经历与长期变化，回答“我是怎样走到现在的”。
4. **起始携带物、当前装备、背包内容不属于角色页**；统一归 `行囊 / Inventory` Surface。没有正式 Inventory Domain 时，不为了展示完整度把物品塞回角色页。

## 1. Historical evidence

旧 `the-world-panel` 形成过一条重要产品原则：

> **Workspace is organized for truth maintenance; UI is organized for player decisions.**
>
> 后台事实按 Owner 组织；玩家 UI 按玩家要回答的问题组织。

SillyTavern 后期 Product UI 又进一步验证：

```text
Left   → Player Status / mechanic contributions
Right  → Character Detail / People / Inventory / Knowledge / Journal / Save...
```

这些是产品证据，不是当前 `my world` 的存储实现合同。

## 2. Current my-world product skeleton

桌面长期骨架保持：

```text
Player Status Host | Narrative Host | World Information Host
```

职责：

```text
Player Status Host
→ 角色立绘 + 当前角色状态 / mechanics HUD
→ 不承担完整 Character identity/profile

Narrative Host
→ GM Narrative + Player natural-language action
→ 核心视觉与交互区

World Information Host
→ 主动查询的 RPG 信息
→ Character / Important Experiences / People / Journal / Inventory / System / Map / Save...
```

正式原则：

> **Canonical truth ownership != player information architecture.**
>
> **UI is a projection, not a second truth source.**
>
> **Core Host owns placement; real Domain / Expansion owns state semantics.**

## 3. Player Status Host — Portrait + live status HUD

长期职责：

> **角色的可视锚点 + 当前可玩状态的高频 HUD。**

候选内容：

```text
角色立绘 / portrait

真实 mechanics / Domain contributions，例如：
- HP / health
- MP / mana
- stamina / endurance
- strength / dexterity / attributes
- injury / buff / debuff
- hunger / fatigue / survival state
- special resource / energy
```

以上只是例子，不是 Core schema。

正式边界：

```text
Player Status Host slot exists
!= Core owns HP / MP / attributes / injuries / inventory
```

没有真实 Domain / Expansion mechanic state 时，不创建假数值。

长期不属于左栏：

```text
姓名 / 年龄 / 来历 / 背景 / 性格
能力说明 / 局限 / 目标 / 原则 / Biography
World / Entry identity
recent actions / player-turn count
Player-known world facts
完整装备 / 行囊明细
```

当前 MW-011 rich left profile 是阶段性过渡实现；右侧 Character Surface 成熟后再迁移。

## 4. World Information Host mother taxonomy

当前母版调整为：

```text
概览
角色
重要经历
人物
事务
行囊
系统
地图
存档
```

不是所有 Surface 都现在实现。一个 Surface 只有同时满足以下条件才进入产品：

```text
real player question
+ real domain owner
+ player-safe projection
+ non-trivial product value
```

## 5. Character Surface — evolving current Character Sheet

Owner 明确要求：

> **Character Sheet 必须随着游戏发展持续更新，但默认展示当前状态，不承担完整变化日志。**

它回答：

> **“现在的我是谁？”**

长期数据来源：

```text
Authored origin / starting identity
+
Game-local lived Character reality
+
其它正式 Domain 的必要 player-safe projection
→ current Character Sheet
```

建议结构：

```text
角色
├─ 基本资料
│  ├─ 姓名
│  ├─ 年龄 / 性别（若有可靠资料）
│  └─ 其它稳定身份信息
├─ 出身 / 来历
├─ 当前身份 / 社会角色
├─ 性格 / 价值观 / 原则
├─ 能力 / 专长说明            # narrative capability，不等于 numeric mechanic stats
├─ 局限 / 长期特征
├─ 长期目标 / 自我方向         # 与事务区分
└─ 其它 player-safe current Character detail
```

不在角色页维护：

```text
Inventory / Equipment
→ 行囊

HP / MP / numeric stats / Buff / short-term state
→ Player Status HUD + System

Relationship / People detail
→ 人物 / future Relationship

Open commitment / task / clue / plan
→ 事务

Knowledge / intelligence
→ 概览或 future Knowledge/Journal

历史变化日志
→ 重要经历
```

## 6. Character evolution authority — discussion baseline

角色变化按性质区分：

### A. 客观长期人物事实

可以由世界自然建立并 durable，例如：

- 获得/失去社会身份或职务；
- 掌握长期能力；
- 形成稳定社会角色；
- 其它长期客观人物事实。

### B. 非自愿长期影响

可以由世界建立，但若已有正式 Domain 则 Existing Domain wins，例如 Injury / Condition。

### C. 主角重大内在自我定义

例如：

- 决定效忠某人；
- 发下长期誓言；
- 放弃回到现代；
- 决定自立；
- 根本改变核心价值/人生方向。

这类变化必须有 Player-originated / Player-authorized evidence，不能仅因 GM 一段心理描写就永久改写主角。

### D. 短期状态

不进入 Character Sheet，例如当天情绪、临时疲劳、短期 Buff、短期恐惧。

## 7. Important Experiences Surface — protagonist milestone history

Owner 新增独立一级 Surface：

> **`重要经历` 用于保存/展示主角过往真正重要的经历与变化。**

它回答：

> **“我是怎样走到现在的？”**

产品形态倾向为按时间排序的 protagonist-centered milestone history，例如：

```text
重要经历

184年
- 初至汉末，成为无本地身份的现代来客

185年
- 获得第一份合法社会身份
- 已能阅读常见隶书，原“书面文字识读困难”长期局限解除

187年
- 首次正式领兵，形成实际基层军务经验

190年
- 玩家明确选择加入刘备阵营，长期人生方向发生变化
```

### 7.1 Important Experiences != Character Sheet

```text
Character Sheet
→ current state
→ “现在的我是谁”

Important Experiences
→ selected change history / milestones
→ “我是怎么变成现在的我”
```

因此 Character Sheet 默认不展示旧状态链和 mutation history。

### 7.2 Important Experiences != full Timeline

`重要经历` 不是所有世界历史，也不是每个 Turn 的聊天日志。

```text
World / Game Timeline
→ 整个世界的历史与恢复结构

Important Experiences
→ 只投影与主角人生高度相关、足以改变身份/能力/长期方向/人生轨迹的重大 milestone
```

### 7.3 Important Experiences != 事务

```text
重要经历
→ 已经发生并值得长期回看的重大过去

事务
→ 尚未解决的当前承诺 / 线索 / 问题 / 计划
```

事务关闭后不自动进入重要经历；只有真正达到 protagonist milestone significance 才进入。

### 7.4 Authority rule

UI 不拥有经历事实，不单独维护第二份 biography truth。

长期应是：

```text
authoritative lived history / Character semantic change / relevant formal Domain event
→ bounded protagonist-milestone materialization or projection
→ player-safe Important Experiences Surface
```

如果未来反复需要独立 milestone identity、排序、摘要、回溯与跨 Domain 聚合，再由真实 consumer 拉出最小 formal milestone capability；现在不预造 universal Event/History platform。

Save / Restore / Timeline 必须保持：Restore 到经历发生前，则该经历与由它导致的当前 Character 变化一起消失。

## 8. Inventory / 行囊

正式目标是**当前真实 Inventory**，不是 Source 物品清单。

```text
Source starting possessions
→ Final Create / game-local ownership（未来需正式承载）
→ Runtime Inventory current state
→ player-safe Inventory projection
→ 行囊 Surface
```

在这条链成立前不做假背包。

## 9. Candidate Surface matrix

| Surface | 玩家问题 | 当前成熟度 | Draft disposition |
|---|---|---:|---|
| 概览 | 当前世界/局势有哪些值得快速了解的信息？ | 高 | 已存在；继续收敛 |
| 角色 | 现在的我是谁？ | 中高 | 第一候选；需 dynamic Character authority/projection |
| 重要经历 | 我是怎样走到现在的？ | 中 | 与 Character semantic audit 联动；需 protagonist milestone authority/projection |
| 人物 | 我知道/遇见了哪些人？他们现在怎样？ | 中 | 后续候选；需 player-safe actor projection |
| 事务 | 还有什么承诺、线索、未解决问题？ | 低 | Deferred；无正式 Thread/Journal Domain |
| 行囊 | 我此刻真正拥有什么？ | 低 | Deferred；无正式 Inventory Runtime Domain |
| 系统 | 本局启用了什么机制？当前怎样？ | 中 | 跟随 Expansion mechanic-state consumer |
| 地图 | 我在哪里？空间/区域如何理解？ | 低 | Deferred；无 spatial authority / visual demand |
| 存档 | 如何保存、恢复、理解恢复点？ | 高 | 已存在；继续保留 |

## 10. Save / Timeline grouping

当前倾向：

```text
存档
├─ Save Points
├─ Restore
└─ recovery/timeline related secondary view（仅按真实需求）
```

`重要经历` 现在承担 protagonist-facing milestone history，因此不再把“人物人生史”塞进 `存档`。

Runtime Timeline Node 仍不自动成为玩家可恢复按钮。

## 11. Proposed G6 sequence — discussion draft

```text
MW-011 Player Host / ViewModel
PRODUCT PASS / CLOSED
↓
G6 Surface / IA convergence
CURRENT DISCUSSION
↓
Character + Important Experiences semantic/domain audit
→ freeze current Character ownership
→ freeze protagonist milestone ownership
→ determine minimal game-local dynamic projections
↓
Character Surface + Important Experiences implementation seam
→ move identity/profile information right
→ keep left only portrait/live status contributions
↓
People Surface
↓
Expansion mechanic-state consumer
→ System Surface + left Player Status contribution
↓
choose next grounded Surface from evidence
(事务 / 行囊 / 势力 / Map / Save polish — not precommitted)
↓
repeated real component patterns exist
↓
MW-013 Internal Declarative UI Host v0.1
↓
bounded Action Intent
↓
responsive / Theme / navigation / final G6 polish
```

Visual asset runtime can re-enter when a real authored portrait / scene / map consumer exists.

## 12. Next Owner discussion questions

已确认：

```text
Player Host            = portrait + live mechanics/status HUD
Character              = evolving current Character Sheet
Important Experiences  = separate protagonist milestone history
Inventory              = separate 行囊 Surface
Starting possessions   = not Character Surface
```

下一轮重点冻结：

1. `重要经历` 的进入门槛：什么程度的事件/变化才值得成为 milestone？
2. Character 与 Important Experiences 是否允许同一次 durable change 同时投影：Character 显示当前结果，Important Experiences 记录变化节点？
3. `长期目标 / 自我方向` 与未来 `事务` 的边界是否按“人生方向 vs 当前待办”冻结？
4. Character Surface 完成后，当前左栏 transitional profile 是否立即迁走；如果当时没有 portrait/mechanic contribution，左栏是否允许折叠？
5. `当前已知事实` 留在概览，还是未来进入独立 Knowledge/Journal 类 Surface？

Owner 讨论完成前，本文件保持 DRAFT，不生成 implementation Task。
