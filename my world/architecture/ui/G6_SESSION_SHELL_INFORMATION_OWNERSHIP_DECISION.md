---
title: my world｜G6 Session Shell Information Ownership Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-06
phase: G6 RPG Experience & Internal Declarative UI Host
owner: OWNER + GPT
historical_evidence:
  - zhangchenjia21-dot/sillytavern/docs/product-ui/G8玩家产品界面架构.md
  - zhangchenjia21-dot/sillytavern/docs/product-ui/G8运行时可扩展界面宿主架构.md
supersedes_when_conflicting:
  - architecture/ui/声明式UIHost设计.md §1 Player Host 的“我是谁”旧表述
---

# G6 Session Shell Information Ownership｜CURRENT

## 1. Owner decision

Owner 于 2026-09-06 明确纠正三栏职责：

> **左栏不负责回答“我是谁”。左栏负责角色立绘，以及由真实角色状态 / mechanics / Expansion 提供的高频当前状态与属性。完整身份、背景、性格、人物说明等属于右侧信息 Surface。**

因此当前正式 Session Shell 信息职责为：

```text
Global / Top Context Strip   # 有真实数据时
→ Game identity / day-time / location breadcrumb / global navigation

Player Status Host（左）
→ portrait
→ high-frequency live character status / mechanic contributions

Narrative Host（中）
→ GM Narrative + Player natural-language action

World Information Host（右）
→ Character identity/profile + People / Inventory / Journal / System / Map / Save 等主动查询 Surface
```

这里的“Top”是 Shell-level context placement，不要求立即增加一个新复杂 Host；当前没有正式 day/time/location projection 时不得造假数据。

## 2. Historical validation from SillyTavern

SillyTavern 已实现的 Player Product UI 给出了直接可复用的产品证据：

- `Top` 放 Game identity、玩家已知 day/time、Region › Place › Scene 与 Save/Settings 等全局导航；
- `Left` **只渲染 `PlayerProductUiContribution[]`**；Core 明确不写死 HP、MP、Strength、Level、Gold 等 RPG 假设；没有 contribution 时允许空/收起；
- `Center` 是核心 Narrative / Composer；
- `Right` 承载 People/Relationship、Map、Inventory、Knowledge、Commitment/Journal、Save 等玩家主动查询信息；
- Runtime-extensible Host 又明确区分 `Player Status` 与 `Player Character Detail` 两种 placement/capability。

这证明“Player Status HUD”与“完整 Character Detail”应是不同的信息职责，而不是因为都与玩家角色有关就塞在同一左栏。

本项目继承的是该产品语义，不迁移旧 React/HTTP/TypeScript 实现。

## 3. Player Status Host ownership

左栏长期产品定位：

> **角色的视觉锚点 + 高频实时状态 HUD。**

允许的长期内容类型：

```text
Portrait / Character visual

Mechanic / Domain contributions，例如：
- HP / health
- MP / mana
- stamina / endurance
- strength / dexterity / other attributes
- injury / buff / debuff
- hunger / fatigue / survival state
- special resource / energy
- other proven high-frequency current-status presentation
```

但这些只是**例子，不是 Core schema**。

正式边界：

```text
Player Status Host owns placement
!= Core owns HP / MP / Strength / Dexterity / injury / resource semantics
```

具体数值与规则必须来自真实 Domain 或 Expansion mechanic state 的 player-safe projection。Core 不根据 Character prose 推导数值，不提供假默认值。

如果一局没有 portrait，也没有合法 status contribution，左栏允许极简、空白或折叠；不得用 biography、World metadata 或 debug/session 信息填满版面。

## 4. What does NOT belong in the left Host

长期默认不属于左栏：

```text
姓名 / 年龄 / 性别 / 来历
背景 / Personality / Biography
人物能力的 authored prose 说明
局限 / 初始目标 / 原则
World / Entry identity
player-turn count / recent actions
Player-known world facts
完整 Inventory / equipment list
People / Relationship / Faction details
Save / Timeline
```

这些进入对应右侧 Surface 或 Shell-level context placement。

例外只允许由真实产品需求证明，而不是为了填空。

## 5. Character identity vs character mechanics

必须永久区分：

```text
Character identity/profile
→ “我是谁”
→ authored / frozen player-safe Character material
→ right-side Character Surface

Character live mechanic/status
→ “我现在的可玩状态是多少/怎样”
→ current authoritative Domain / Expansion state
→ left Player Status Host summary
→ optional full detail in right-side System / mechanic Surface
```

例如：

```text
“体格强健、受过现代格斗训练”
= authored Character description
= Character Surface

“力量 14 / HP 23/30 / 疲劳 40%”
= mechanic runtime state（只有真实 Expansion/Domain 存在时）
= left HUD high-frequency contribution
```

同理：

```text
“穿越时带着军用水壶”
= authored starting possession reference

“当前背包还有 640ml 水、匕首耐久 76%”
= dynamic Inventory/mechanic state
```

两者不得互相冒充。

## 6. Right-side information ownership

右侧 World Information Host 是玩家主动查看信息的主要容器。

“我是谁”属于右侧 `角色 / Character` Surface。

当前 G6 Surface taxonomy 仍在 Owner + GPT 讨论中；母版候选继续是：

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

只有真实 Domain + player-safe projection + non-trivial player value 成立的 Surface 才出现；不创建空 RPG Tab。

## 7. Current transitional UI

MW-011 为解决左栏信息过薄，已经阶段性把完整 `player_profile` 渲染到左栏，并获得 Owner Product PASS。

这不再代表长期 IA：

```text
current rich left profile = ACCEPTED TRANSITIONAL IMPLEMENTATION
long-term left            = portrait + live status/mechanics HUD
long-term identity/detail = right Character Surface
```

在右侧 Character Surface 尚未实现前，不要求立刻清空左栏，避免产品回退成无信息面板。

完成 Character Surface 后，再通过 Owner UAT 决定实际迁移节奏。

## 8. Consequence for G6 route

下一步继续讨论/冻结：

1. right-side Character Surface 的信息结构；
2. 当前 transitional left profile 中哪些字段在 Character Surface 完成后移除；
3. People Surface 的 player-safe actor boundary；
4. 第一个真实 Expansion mechanic-state consumer 如何同时提供：
   - left high-frequency Player Status contribution；
   - right System/detail Surface。

MW-013 Internal Declarative UI Host 继续 HOLD，等待更多真实 Surface / mechanic consumer 形成重复模式。
