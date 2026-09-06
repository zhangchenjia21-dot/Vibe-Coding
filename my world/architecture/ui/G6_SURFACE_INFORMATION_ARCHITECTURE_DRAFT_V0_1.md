---
title: my world｜G6 Surface / Information Architecture Draft v0.3
status: DRAFT / FOR OWNER DISCUSSION
version: 0.3
created: 2026-09-06
updated: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
semantic_owner: GPT + Owner
historical_reference: zhangchenjia21-dot/the-world
---

# G6 Surface / Information Architecture Draft v0.3

> **DRAFT — NOT FROZEN — NOT IMPLEMENTATION AUTHORITY**

本文件用于 Owner + GPT 讨论 `my world` 的三栏职责与右侧 RPG Surface 分类。它吸收 `the-world` 与 SillyTavern 已验证的 UI 经验，但不机械复制旧宿主实现。

当前已确认的 Owner 方向：

1. **左侧 Player Host 不承担“我是谁”**；长期定位是角色立绘 + 当前角色状态 / mechanics HUD。
2. **右侧 `角色 / Character Sheet` 是会随游戏发展持续更新的完整角色页**，不是静态“开局角色卡查看器”。
3. **起始携带物不属于角色页**；物品/装备归 `行囊 / Inventory` Surface。没有正式 Inventory Domain 时，不为了展示完整度把起始物品塞回角色页。

## 1. Historical evidence

旧 `the-world-panel` 形成过一条重要产品原则：

> **Workspace is organized for truth maintenance; UI is organized for player decisions.**
>
> 后台事实按 Owner 组织；玩家 UI 按玩家要回答的问题组织。

其玩家分页母版为：

```text
概览
角色
人物
行囊
事务
系统
存档
```

SillyTavern 后期 Product UI 又进一步验证：

```text
Left   → Player Status / mechanic contributions
Right  → Character Detail / People / Inventory / Knowledge / Journal / Save...
```

这些是产品证据，不是当前 `my world` 的存储实现合同。

## 2. Current my-world product skeleton

桌面长期骨架保持：

```text
Player Host | Narrative Host | World Surface Host
```

职责：

```text
Player Host
→ 角色立绘 + 当前角色状态 / mechanics HUD
→ 不承担完整 Character identity/profile

Narrative Host
→ 现在发生了什么？我接下来想做什么？

World Surface Host
→ 我是谁 / 这个世界里有哪些值得主动查看的信息？
→ Character / People / Journal / Inventory / System / Map / Save...
```

正式原则：

> **Canonical truth ownership != player information architecture.**
>
> **UI is a projection, not a second truth source.**
>
> **Core Host owns placement; real Domain / Expansion owns state semantics.**

## 3. Player Host — Portrait + live status HUD

Player Host 的长期职责是：

> **角色的可视锚点 + 当前可玩状态的高频 HUD。**

候选内容：

```text
角色立绘 / portrait

角色状态 / mechanics contributions
├─ 生命值 / 魔力值
├─ 力量 / 敏捷 / 体力等属性
├─ 伤势 / Buff / Debuff
├─ 饥饿 / 疲劳 / 生存状态
├─ 资源 / 能量 / 特殊机制摘要
└─ 其它真实高频 current-status widget
```

以上只是示例，不是 Core schema。正式边界：

```text
Player Host slot exists
!= Core owns HP / MP / attributes / injuries / inventory
```

具体状态必须来自正式 Domain 或 Expansion mechanic state 的 player-safe projection。没有真实 mechanic state 时，不创建假数值。

不属于左栏长期职责：

```text
姓名 / 年龄 / 来历 / 背景 / 性格
能力说明 / 局限 / 目标 / 原则 / Biography
World / Entry identity
recent actions / player-turn count
Player-known world facts
完整装备 / 行囊明细
```

当前 MW-011 的 rich left profile 是阶段性过渡实现；右侧 Character Surface 成熟后再迁移。

## 4. World Surface mother taxonomy

当前母版：

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

只有满足以下条件的 Surface 才进入产品：

```text
real player question
+ real domain owner
+ player-safe projection
+ non-trivial product value
```

## 5. Character Surface — evolving Character Sheet

Owner 当前明确要求：

> **Character Sheet 必须随着游戏发展持续更新。**

因此它不能只是：

```text
frozen Character Source / player_profile viewer
```

长期目标是：

```text
Authored origin / starting identity
+
Game-local lived Character reality
→ current player-safe Character Sheet
```

建议结构：

```text
角色
├─ 基本资料
│  ├─ 姓名
│  ├─ 年龄 / 性别（若有可靠资料）
│  └─ 其它稳定身份信息
├─ 出身 / 来历
├─ 当前身份 / 社会身份        # 可随 lived history 更新
├─ 背景 / 重要经历            # Source 既有经历 + 本局形成的重要履历
├─ 性格 / 价值观 / 原则        # baseline + 已明确形成的 durable 变化
├─ 能力 / 专长说明            # narrative capability，不等于 numeric mechanic stats
├─ 局限 / 长期特征
└─ 其它 player-safe Character detail
```

### 5.1 Character Sheet 不拥有 live mechanic stats

必须区分：

```text
“体格强健、受过现代格斗训练”
= Character description
= Character Surface

“力量 14 / HP 23/30 / 疲劳 40%”
= Runtime mechanic state
= Player Host HUD + optional System detail
```

### 5.2 Character Sheet 不拥有 Inventory

Owner 已明确：

```text
起始携带物 / 当前装备 / 背包内容
→ 行囊 / Inventory Surface
→ 不放角色页
```

并永久区分：

```text
Authored starting possessions
!= current Runtime Inventory
```

Character Source 中的“穿越时携带水壶/匕首”等仍然可以作为 GM / starting-world reference，但玩家产品 UI 不应因为缺少 Inventory Domain 就把它们塞进 Character Sheet。

在正式 Inventory Domain / player-safe projection 建立之前，可以暂时没有独立行囊页；**宁可暂时不展示，也不建立第二份假 Inventory truth。**

### 5.3 Current implementation gap

现有 `player_profile` 主要是 frozen authored presentation，因此只能完整回答“开局时这个角色是什么样的人”。

如果 Character Sheet 要真正随本局发展持续更新，后续 implementation 前必须审计并确定：

```text
哪些 Character facts 已经有 game-local authority？
哪些动态身份/履历/人格变化可以安全投影？
是否需要最小 Player Character current-profile / identity projection？
如何保证 Save / Restore / Timeline 一致？
```

不能让 UI 自己从 Narrative 文本猜“你现在是什么身份”。

## 6. Candidate Surface matrix

| Surface | 玩家问题 | 当前成熟度 | Draft disposition |
|---|---|---:|---|
| 概览 | 当前世界/局势有哪些值得快速了解的信息？ | 高 | 已存在；继续收敛 |
| 角色 | 我是谁？现在已经成为怎样的人？ | 中高 | **第一候选；需补动态 Character authority/projection 审计** |
| 人物 | 我知道/遇见了哪些人？他们现在怎样？ | 中 | 第二候选；需 player-safe actor projection |
| 事务 | 还有什么承诺、线索、未解决问题？ | 低 | Deferred；无正式 Thread/Journal Domain |
| 行囊 | 我此刻真正拥有什么？ | 低 | Deferred；无正式 Inventory Runtime Domain |
| 系统 | 本局启用了什么机制？当前怎样？ | 中 | 跟随 Expansion mechanic-state consumer |
| 地图 | 我在哪里？空间/区域如何理解？ | 低 | Deferred；无 spatial authority / visual demand |
| 存档 | 如何保存、恢复、理解历史节点？ | 高 | 已存在；继续保留 |

## 7. People Surface

长期方向：关系化人物视图，而不是简单 NPC 名单。

候选玩家可见信息：

```text
显示名
当前公开/已知状态
最后已知位置
所属/阵营
与主角关系
最近确认/最近互动
可展开的已知人物详情
```

都必须有 player-known / relationship authority 支撑。

不得直接展示 actor-private Knowledge、GM agenda、red line、independent next move、omniscient truth、raw Source private/reference prose、internal IDs/fingerprints。

## 8. Deferred surfaces

### 行囊

正式目标是**当前真实 Inventory**，不是 Source 物品清单。

```text
Source starting possessions
→ Final Create / game-local ownership（未来需要正式承载）
→ Runtime Inventory current state
→ player-safe Inventory projection
→ 行囊 Surface
```

在这条链没有成立前不做假背包。

### 事务

当前无正式 Thread/Journal Domain，不从 Narrative 关键词硬猜 Quest。

### 地图

当前无 spatial authority，Visual Runtime 也继续 deferred。

### 系统 / mechanic state

```text
no durable mechanic state
→ no fake System page
→ no fake left-HUD stats
```

未来同一 mechanic state 可同时投影：

```text
high-frequency current state → Player Host HUD
full mechanic detail         → System Surface
```

## 9. Save / Timeline draft grouping

当前倾向：

```text
存档
├─ Save Points
├─ Restore
└─ Timeline / history secondary view
```

暂不把所有 Runtime Timeline Node 提升为玩家可恢复按钮。

## 10. Proposed G6 sequence — discussion draft

```text
MW-011 Player Host / ViewModel
PRODUCT PASS / CLOSED
↓
G6 Surface / IA convergence
CURRENT DISCUSSION
↓
Character Surface semantic/domain audit
→ freeze evolving Character Sheet ownership
→ determine minimal game-local dynamic Character projection
↓
Character Surface implementation
→ move identity/profile information right
→ keep left only portrait/live status contributions
↓
People Surface
↓
Expansion mechanic-state consumer
→ System Surface + left Player Status contribution
↓
choose next grounded Surface from evidence
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

Visual asset runtime can re-enter when a real authored portrait / scene / map consumer exists。

## 11. Next Owner discussion questions

已确认：

```text
Player Host = portrait + live mechanics/status HUD
Character   = evolving Character Sheet
Inventory   = separate 行囊 Surface
Starting possessions do not belong in Character Surface
```

下一轮需要重点冻结：

1. Character Sheet 中哪些信息属于“稳定背景”，哪些属于“当前会变化的角色现实”？
2. `当前身份 / 社会身份`、重大履历、人格/原则变化分别由什么 game-local authority 承载？
3. `初始目标` 是否继续属于 Character Sheet，还是具体 open goals/commitments 未来进入 `事务`？
4. Character Surface 完成后，当前左栏 transitional profile 是否立即迁走；若此时没有 portrait/mechanic contribution，左栏是否允许折叠？
5. `当前已知事实` 留在概览，还是未来进入 Knowledge/Journal 类 Surface？

Owner 讨论完成前，本文件保持 DRAFT，不生成 Character implementation Task。