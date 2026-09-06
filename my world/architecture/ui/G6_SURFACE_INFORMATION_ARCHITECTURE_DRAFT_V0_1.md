---
title: my world｜G6 Surface / Information Architecture Draft v0.1
status: DRAFT / FOR OWNER DISCUSSION
version: 0.1
created: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
semantic_owner: GPT + Owner
historical_reference: zhangchenjia21-dot/the-world
---

# G6 Surface / Information Architecture Draft v0.1

> **DRAFT — NOT FROZEN — NOT IMPLEMENTATION AUTHORITY**

本文件用于 Owner + GPT 讨论 `my world` 的三栏职责与右侧 RPG Surface 分类。它吸收 `the-world` 真实试玩形成的 UI 经验，但不机械复制旧 DSH/Markdown 实现。

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
角色 HUD      ← Player truth
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

三栏分别回答：

```text
Player Host
→ 我是谁？我现在怎么样？

Narrative Host
→ 现在发生了什么？我接下来想做什么？

World Surface Host
→ 这个世界里有哪些值得我主动查看的信息？
```

正式原则：

> **Canonical truth ownership != player information architecture.**
>
> **UI is a projection, not a second truth source.**

## 3. Draft responsibility split

### 3.1 Player Host — 高频 HUD，不是完整百科

长期目标：Player Host 应成为“扫一眼即可读完”的高频 HUD。

候选长期内容：

```text
主角姓名 / 一行身份
当前角色概况 / 关键高频状态
当前地点 / 时间            # 只有正式 Domain 存在后
高频资源 / 装备摘要        # 只有正式 Runtime state 存在后
少量 session / turn 信息
[查看角色详情]
```

当前 MW-011 为解决信息过薄，暂时把完整 `player_profile` 放在左栏；Owner 已接受此状态，但指出未来需要重新分栏。

因此：

```text
current rich left panel = ACCEPTED TRANSITIONAL STATE
long-term Player Host   = compact high-frequency HUD
```

### 3.2 Narrative Host — 不变

继续保持全产品视觉中心：

- GM Narrative；
- streaming；
- Player natural-language Composer；
- Cancel / Regenerate / Retry 等低风险 turn actions。

不把右侧 Surface 导航、Character Sheet 或系统状态塞进中央 Narrative。

### 3.3 World Surface Host — 玩家主动查询的 RPG 信息

初版母版继承 The World 已验证分类，但只显示有真实 Domain / player-safe projection 的 Surface。

建议长期候选：

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
| 概览 | 这一刻世界与我处在什么局面？ | 高 | 已存在，继续保留 |
| 角色 | 我的完整角色资料是什么？ | 高 | **建议第一个新真实 Surface** |
| 人物 | 我已经知道/遇见了哪些人？他们现在怎样？ | 中 | 建议第二候选；先审 player-safe actor projection |
| 事务 | 还有什么承诺、线索、未解决问题？ | 低 | Deferred，当前无正式 Thread/Journal Domain |
| 行囊 | 我此刻真正拥有什么？ | 低 | Deferred，当前无正式 Inventory Runtime Domain |
| 系统 | 本局启用了什么机制？机制当前怎样？ | 中 | 跟随 G6 Expansion mechanic-state consumer |
| 地图 | 我在哪里？空间/区域如何理解？ | 低 | Deferred；无 spatial authority，视觉 resolver 也 deferred |
| 存档 | 如何保存、恢复、理解历史节点？ | 高 | 已存在；继续保留并逐步完善 |

## 5. Draft: first new Surface = 角色

理由：

1. `player_profile` 已有完整、player-safe、Game-local frozen projection；
2. Owner 刚刚确认左栏信息完整，但也指出“很多内容未来应分到右侧”；
3. 不需要新增 Runtime truth；
4. 可以让左栏回归 HUD，而右侧承担 Character Sheet；
5. 这是旧 The World 已经真实验证过的 IA。

Draft 角色 Surface 可包含：

```text
角色
├─ 基本身份
├─ 背景
├─ 性格
├─ 能力
├─ 局限
├─ 初始目标 / 长期原则
├─ 起始携带物      # authored starting facts，不冒充 dynamic Inventory
└─ 当前已知事实    # 来自现有 player-known projection，是否放此处待讨论
```

需要特别区分：

```text
Character authored starting possessions
!= Runtime Inventory current possessions
```

没有 Inventory Domain 前，不把“军用水壶/指南针”等起始携带物包装成会动态增减的背包系统。

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

### 事务

旧 The World 有 `THREADS.md` 作为明确 open-thread owner；当前 `my world` 没有正式 Journal/Thread Domain。

所以不创建空“任务/事务”页，也不从 Narrative 关键词硬猜 Quest。

### 地图

旧 The World 已证明长局空间认知会成为真实痛点，但最薄地图也需要 authored map/current location 等真实消费者。

当前 G6 Visual Runtime re-entry 已裁定 implementation deferred，因此 Map 继续等待真实需求。

### 系统

旧 The World 的“系统”只有在本局真的有长期 mechanic state 时才出现。

当前 `my world` 也应保持同样原则：

```text
no durable mechanic state
→ no fake System page
```

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

## 9. Surface appearance rule

一个 Surface 进入产品至少要同时满足：

```text
real player question
+ real domain owner
+ player-safe projection
+ non-trivial product value
```

禁止：

```text
为了“像 RPG”而创建空 Tab
为了 UI 先造 fake HP / location / inventory / faction state
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
Role / Character Surface
+ compact Player HUD redistribution
↓
People Surface
+ player-safe known-actor projection as needed
↓
choose next grounded Surface from real evidence
(事务 / 行囊 / 势力 / Timeline / Map — not precommitted)
↓
Expansion mechanic-state consumer / System Surface
↓
repeated real component patterns exist
↓
MW-013 Internal Declarative UI Host v0.1
↓
bounded Action Intent
↓
responsive / Theme / navigation / final G6 polish
```

Visual asset runtime can re-enter anywhere after a real authored first-party visual consumer appears; it is not permanently cancelled.

## 11. Owner discussion questions

本 Draft 下一轮需要 Owner 重点裁定：

1. 左栏长期是否接受定位成“高频 Player HUD”，完整角色卡迁到右侧 `角色`？
2. 右侧一级分类是否以 `概览 / 角色 / 人物 / 行囊 / 事务 / 系统 / 地图 / 存档` 作为母版？哪些名称想改？
3. `人物` 是否先承担关系/所属信息，而不是现在就拆 `关系`、`势力` 两个一级 Tab？
4. 张琛“起始携带物”在没有 Inventory Domain 前，是继续放角色详情，还是完全不显示？
5. `当前已知事实` 更适合角色页、概览页，还是未来单独 Journal/Knowledge 结构？
6. Save 与 Timeline 是否接受一级 `存档` + 二级 Timeline 的方向？

Owner 讨论完成前，本文件保持 DRAFT，不生成对应 implementation Task。
