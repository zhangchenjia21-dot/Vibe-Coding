---
title: G8-UAT-02｜Game-local Semantic Materialization 与活世界收口规格
status: current-spec
version: 1.0
date: 2026-08-18
project: 酒馆游戏新版主体
scope:
  - creation-semantic-materialization
  - game-local-assets
  - opening-world
  - runtime-world-materialization
  - inventory
  - player-profile
  - information-surface
---

# G8-UAT-02｜Game-local Semantic Materialization + Living World Convergence

## 0. 任务目的

修复 `52d0421...` Stage UAT 暴露的“语义占位世界”问题。

本任务不是 polish，也不是完整 G9 Asset Ecosystem。

必须建立第 16 号裁定的**最小 production vertical**：

```text
Creation semantic input
→ typed semantic materialization
→ Game-local canonical entities / definitions
→ Runtime binding/state
→ player-safe Product projection
→ Narrative / Suggestions
```

并增加最小 Living World capability，使游戏不再是静态占位 seed。

---

## 1. 禁止继续使用的正常路径

Configured AI-assisted Creation 正常成功时不得再生成玩家可见 canonical：

- `附近的人`
- `附近区域`
- `随身物品`
- `新的地点`

这些 generic labels 只能用于 explicit degraded/error diagnostics，不得冒充 materialized world content。

---

## 2. Creation Field Semantic Audit

对全部 Creation Project field 建立语义分类表，至少区分：

1. scalar canonical definition；
2. structured entity/list candidate；
3. semantic/materialization guidance；
4. world hard constraint；
5. player public profile；
6. private/hidden seed candidate；
7. runtime state seed；
8. product-only / advisory metadata。

必须重点审计：

- important_opening_characters
- initial_resources_and_carried_items
- opening_location
- current_situation
- immediate_problem_or_opportunity
- public_origin_background
- current_goals_and_attachments
- important_past_experiences
- initial_personality_style
- language_expression_style

不得继续假设 `string field = one Runtime Entity`。

---

## 3. Typed Creation Materialization Plan

新增 internal typed contract；不冻结 G9 external schema。

语义上至少可表达：

### Opening Character Plan

- 0..N concrete Character definitions
- display name
- public description
- role / opening relevance
- initial scene
- optional private seed
- provenance

### Opening Place Plan

- concrete opening Place / Scene
- 0..N concrete adjacent destinations
- public description
- Connection(s)
- traversal duration
- provenance

### Opening Item / Resource Plan

- 0..N concrete Item definitions
- optional non-item resource records if current Runtime owner supports them
- public description
- initial holder
- provenance

### Player Profile Plan

- identity
- background
- goals
- past
- personality
- language style

Program validates cardinality, refs, visibility, identity and world constraints before commit。

---

## 4. AI-assisted Creation Materialization

调整先前 `Final Create zero mandatory AI call` 的解释：

### Manual / No-key

继续允许 deterministic creation。

但 deterministic path：

- 只能 materialize 明确结构化、无歧义的数据；
- 不得用 generic placeholder 假装理解复杂自由文本；
- 若缺少足够结构化内容，可给出明确 degraded/manual-required 状态，而不是创建语义错误实体。

### AI-assisted + configured Provider

允许 / 要求 bounded typed semantic materialization call：

```text
Creation Project semantic fields
→ Materialization Provider
→ strict typed plan
→ Program validation
→ atomic Game-local seed commit
```

这次调用属于 AI-assisted Creation 的正式组成，不是 Narrative side effect。

不得输出 reasoning / hidden prompt / arbitrary code。

---

## 5. Game-local Canonical Seed

本任务首次 production-proof 第 16 号裁定的最小 Game-local semantics。

至少需要稳定 provenance：

- creation-defined
- creation-materialized
- runtime-generated（为 Living World 使用）

Source/Creation Project 后续不被运行时反写。

Materialized Character / Place / Item 拥有 stable semanticRef，并由 Save / Restore 保持。

不要求本任务完成完整 external World Pack / Character Card clone/bind；那仍属于 G9。

---

## 6. Opening Beat / Proactive World

AI-created game 首次进入 Session 时，正常 configured path 必须有具体 opening affordances，而不是只有静态环境。

至少满足：

- >= 1 concrete named / clearly identified NPC；
- >= 1 concrete meaningful destination；
- >= 1 immediate hook / problem / opportunity 可被 Narrative 自然提出；
- 玩家 Creation 中如果描述多个重要 opening roles，不能合并成一个人格混杂 NPC；
- Narrative 首轮可以主动介绍 current authoritative opening cast / hook。

允许 NPC 名称符合世界风格；不得强制所有世界使用现代人名格式。

---

## 7. Minimal Runtime World Materialization

将第 16 号 World Materialization 的最小纵切提前到 G8 Exit 前。

至少覆盖：

### 7.1 Plausible unknown local destination

例：

玩家在学院：
`我去附近找一家酒馆。`

如果 World constraints 允许，但当前没有 tavern Scene：

```text
Semantic identifies local exploration/materialization need
→ Model proposes bounded Place/Scene/Connection
→ Program validates
→ create stable Game-local place/scene/connection
→ optional move in same controlled transaction
→ Narrative realizes committed destination
```

不得再永久返回“没有酒馆”，也不得 Narrative 先写酒馆存在再补数据库。

### 7.2 New local NPC

当 opening/story/player interaction 合理需要一个新具体角色时：

```text
Model Character proposal
→ Program validation
→ Game-local Character create
→ Runtime initial state bind
→ stable identity
→ Narrative introduction
```

至少支持 Opening Beat 或首次进入新 materialized Scene 的 bounded cast creation。

### 7.3 Boundedness

每次只 materialize 当前动作 / opening beat 所需最小局部内容。

禁止 speculative world explosion、全城预生成、每 Turn 无条件生成大量实体。

---

## 8. Narrative / Authority

G8-UAT-01 的所有 authority 规则继续保留：

- Program Final Outcome authoritative；
- No Phantom Interactable；
- locationAuthority；
- interactableAuthority；
- hidden disclosure boundary；
- Player Agency。

新的世界内容必须：

```text
materialize + commit first
→ Narrative second
```

或在同一个 atomic formal transaction 中完成 materialization + action + narrative。

---

## 9. Inventory Semantic Fidelity

Creation 中多项物品 / 资源不得被压为单一 generic Item。

要求：

- 明确多个物件时生成多个 stable Item；
- 钱币/抽象资源若当前没有专用 Resource Owner，可暂以明确 Item/Resource representation 处理，但不得全部合并成“随身物品”；
- `检查身上的物品` 必须读取 authoritative inventory；
- Narrative 列出的具体物品必须与 Product 【物品】栏一致；
- 新物品获得/丢失后 Product Surface 随 Runtime state 更新。

---

## 10. Information Surface 语义修复

正式冻结：

```text
Information Surface
!= Generic Formal Event Journal
```

默认显示：

- player-known knowledge / information facts；
- 后续正式 Objective/Quest 信息（若 owner 存在）；
- 其它明确归类为 Information 的 player-safe records。

不得把以下 generic bookkeeping 当“信息”：

- ACTION_COMPLETED
- PLAYER_MOVED
- generic WORLD_ACTION_COMPLETED

这些可保留在 Timeline / Journal / debug evidence，但不进入 Information Core Surface。

---

## 11. Player Profile Product Projection

新增/扩展 player-safe Product Profile，使当前 Creation 已定义的公开角色设定可见。

至少：

- name/calling
- identity
- public background
- current goals / attachments
- important past experiences
- personality style
- language expression style

左侧摘要可保持简洁，但 Player Detail 必须完整读取这些 player-safe canonical fields。

禁止把 hidden/private 信息混入。

---

## 12. Dynamic Five Suggestions

G8-UAT-01 建议系统继续复用，但 working set 必须改为新 materialized concrete entities。

不得推荐：

- `附近的人`
- `附近区域`
- `随身物品`

除非它们在特殊世界里就是明确 canonical 名称。

建议应随新 NPC / Place / Item materialization 刷新。

---

## 13. Real UAT Corpus

除 G8-UAT-01 原始 9 条外，新增项目所有者本轮真实语料：

- 观察以下四周
- 检查一下身上有什么随身物品
- 我向附近的人打招呼，问问这里的情况
- 我前往附近区域
- 我仔细查看随身物品
- 我想了想自己接下来真正想做什么
- 我前往学院
- 回想导师名字 / 导师身份相关输入

测试不能把这些输入改写成更容易通过的句子。

---

## 14. Required Proof

### Creation semantic fidelity

给定 opening roles 三类：
- 导师
- 同行者
- 对手

不得生成一个同时承担三种语义的 generic NPC。

### Item cardinality

给定：
- 专业工具
- 日常用品
- 一袋钱币

Product inventory 至少表达对应独立 concrete records，不得只有一个“随身物品”。

### Concrete destination

Configured AI-assisted opening 不得正常产生 canonical `附近区域` placeholder。

### Opening beat

首次 Session Narrative 能主动介绍 committed concrete NPC / hook；不需要玩家先问三遍“有人吗”。

### JIT place

当前无酒馆但世界允许：
`去附近找一家酒馆`
→ stable tavern scene/connection materialized
→ player can enter
→ later revisit same identity。

### Product consistency

Narrative inventory == Product inventory；
Narrative current location == Map surface；
Narrative concrete NPC == People surface；
Information surface 不含 generic ACTION_COMPLETED / PLAYER_MOVED。

### Player profile

Player Detail 显示 Creation 中完整 player-safe profile。

### Save/Restore

新 materialized NPC / Place / Item 与其 stable identity 在 Save / Restore 后保持。

---

## 15. Real Provider Gate

使用正式 DeepSeek model 验证：

1. multi-role opening brief → distinct concrete NPCs / roles；
2. multi-item resource brief → concrete inventory；
3. opening beat 主动、有具体人物与 hook；
4. plausible unknown tavern → JIT materialize and move；
5. return visit reuses same tavern identity；
6. new NPC remains same identity；
7. no hidden disclosure；
8. Narrative / Product / Runtime 三方一致。

---

## 16. Non-scope

本任务仍不实现：

- external World Pack / Character Card / Expansion machine schema；
- G9 asset-spec vNext；
- Creator rebuild；
- general city/world pre-generation；
- arbitrary model DB write；
- full quest/task engine；
- WEB-06 / WEB-07。

---

## 17. Exit

只有以下同时 PASS：

```text
Semantic fidelity
+ Concrete playable opening
+ Minimal living-world materialization
+ Inventory/Product consistency
+ Player Profile completeness
+ Information IA correctness
+ G5/G6/G7/G8 regression
+ Real Provider Gate
+ Project Owner Stage UAT
```

才能关闭 G8。
