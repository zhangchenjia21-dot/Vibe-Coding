---
title: my world｜G6 Open Threads Surface v1.0 Decision
status: FROZEN / CURRENT
version: 1.0
created: 2026-09-09
updated: 2026-09-09
phase: G6 Package 3 Core Information Continuity
owner: Owner + GPT
parent_authority:
  - architecture/ui/G6_MODEL_DRIVEN_INFORMATION_CURATION_AUTHORITY_DECISION.md
  - MY_WORLD_总体规划路线图_CURRENT.md
---

# G6 Open Threads / 事务 Surface v1.0｜CURRENT

## 1. Product question

`事务 / Open Threads` 回答：

> **“最近有哪些还没有真正结束、但值得我继续记住和跟进的事情？”**

它是玩家当前信息 Surface，不是 Quest 系统、任务管理器、剧情流水账或后台 World Truth 浏览器。

可能进入这里的内容包括正在推进的计划、尚未兑现的承诺、等待结果的事情、未解决的冲突或风险、仍然重要的线索与悬而未决的问题。以上只是例子，不形成固定类别表。

允许当前没有任何事务。

## 2. Semantic authority

继承冻结原则：

> **Model owns semantic interpretation and curation; Program owns normalized storage, temporal integrity and presentation.**

因此模型直接判断：

- 一件未完事项是否值得持续记住；
- 本轮是否新增一个事务；
- 已有事务是否发生了有意义的状态变化；
- 某个事务是否已经解决、失效或不再值得持续占用玩家注意力；
- 如何用玩家可读的当前摘要表达它。

Program 不建立平行的 Quest / task semantic engine，不用关键词、regex、事件类型、重要度分数、优先级规则、回合阈值或“见到某类词就建任务”的 heuristic 代替模型判断。

**当前场景是否提到一件事既不是进入事务栏的必要条件，也不是充分条件。** 场外但仍然悬而未决的重要事项可以继续保留；当前回合出现的小问题也可以完全不建事务。

## 3. Current snapshot, not history

`事务` 保存的是**当前仍值得跟进的未完事项快照**，不是每个事务的完整历史。

规则：

- 新事实使某个事务的当前状态发生变化时，模型可更新它；
- 事务已经解决、失效或不再需要持续提示时，模型可从当前快照中移除；
- 被移除不等于删除过去发生过的事；其历史仍存在于 accepted Narrative / Timeline / Save 中；
- 普通回合非常可以 `no-change`；不要求每回合制造一个事务变化。

这与其它 Surface 的职责区分为：

- `角色`：现在的我是谁；
- `重要经历`：哪些过去事件真正塑造了我；
- `人物`：我当前对值得持续记住的人最近最新知道什么；
- `事务`：现在还有哪些值得我继续记住的未完事项。

长期人生方向仍由 Character 拥有。若某个长期方向同时产生了一个当前具体的未完问题，`事务`只描述当前待推进/待解决部分，不复制第二份“角色身份真相”。

## 4. One Information Curator, no extra semantic call

Package 3 优先扩展现有 **Post-turn Information Curator**，不增加独立 Open Threads Provider call。

普通 lived curation 继续是一次后台调用，在现有 Character / Important Experiences / People 之外同时维护 Open Threads：

```text
accepted current Player action
+ accepted current GM Narrative
+ current player-safe Character / Experiences / People evidence
+ current player-safe Open Threads snapshot
+ existing bounded safe context
↓
existing Information Curator
↓
Character / Experiences / People / Open Threads bounded result
```

Open Threads 只消费玩家已接受、玩家可知的材料与其自身当前安全投影。不得把 raw `world_state`、NPC private Knowledge / Agency / Evolution、幕后尚未披露变化或其它 GM-private material 暴露给 Curator / leaf UI 以“补全任务”。

OOC 仍然不是 protagonist lived action，继续服从 Package 2 既有 typed-mode exclusion；本 Decision 不改变 OOC 语义。

## 5. Minimal bounded curation contract

v1.0 采用完整当前快照替换，而不是先造稳定 Quest ID / status enum / operation forest。

Lived curation 新增：

```json
"open_threads": null
```

或：

```json
"open_threads": [
  {
    "title": "简洁标题",
    "summary": "当前为什么仍然悬而未决，以及玩家现在知道的关键状态",
    "details": ["必要时补充的玩家已知信息"]
  }
]
```

语义：

- `null` = 本轮保持当前 Open Threads 快照；
- 非空数组 = 用该数组完整替换当前快照；
- `[]` = 当前没有需要持续跟进的事务，可清空快照。

机器边界：

- 最多 12 条 current threads；
- `title` 最多 160 Unicode chars；
- `summary` 最多 800 chars；
- `details` 最多 4 项，每项最多 500 chars；
- 不增加 `type / priority / progress / quest_state / deadline` 等预设语义字段。

这些容量约束只负责 bounded storage / rendering，不决定一件事是否重要。

Program 可以拒绝 malformed / oversize / stale payload；Program 不判断“标题像不像任务”“这条是否足够重要”或“是否真的已解决”。

## 6. Persistence and Timeline currentness

继续使用现有 `information_curation` owner，不新建 SQLite table / 第二持久化 owner。

实现应采用向后可读的 lived schema 演进：

- 既有 Character / Experiences / People 历史继续可读；
- 新版本记录可以携带 Open Threads；
- `current accepted prefix + parent chain` 继续决定 currentness；
- Save / reopen / Restore / Regenerate 后只投影当前 Timeline 的快照；
- displaced future / stale callback 不得污染当前事务；
- no-change 仍可使用既有 durable curation receipt 防止 reopen 重调。

旧 Game 不做历史回扫或 Source-current backfill。升级后从新的 accepted lived turns 开始正常维护；不得为了“补齐”旧事务额外扫描整局或增加 Provider 调用。

初始 Character curation 仍然不能凭 frozen starting profile 创建 Open Threads。Source 世界设定、角色背景或幕后计划不自动成为玩家当前事务。

## 7. Player-facing surface

World Information Host 的 v1.0 导航扩展为：

```text
概览 | 角色 | 重要经历 | 人物 | 事务 | 存档
```

`事务` 使用简单、舒适可读的当前列表/卡片呈现：

- title；
- summary；
- 有内容时显示 bounded details；
- 空列表提供明确 empty state。

v1.0 只是信息 Surface：

- 无 checkbox / 手动完成按钮；
- 无玩家编辑事务；
- 无搜索、筛选、排序、分页；
- 无任务优先级控件；
- 无通用 Dynamic UI 抽象；
- 无“点击事务自动生成行动”或 generic Action Intent。

后续 Package 6 Internal Dynamic UI 可以统一展示词汇，但不得在本任务前置平台化。

## 8. Debug Mode integration

Package 1 的只读 observability seam 增加 `threads` lane。

每次 lived Curator 终态，Debug Mode 应能看到：

```text
Open Threads / threads
→ changed | no-change | failed | stale | cancelled
→ bounded total count when safe/available
```

Debug 只比较 player-safe before/after projection，不读取/显示隐藏 World/NPC 语义，不改变 Curator 结果，也不把 Debug 数据喂回模型。

## 9. Fail-soft behavior

Open Threads 是后台玩家信息维护，不是 Narrative Finalize Gate。

- Curator failure 不撤销 accepted Narrative；
- Open Threads malformed / Provider failure 不阻止下一次正常行动；
- 不为了保住 Threads 而放宽 stale/currentness 写入规则；
- 不为这一 Surface 新增自动 fallback semantic parser。

如果新字段缺失需要兼容旧响应，允许按结构性 no-op 处理以保护既有 Character / Experiences / People lane；不得用 Program heuristic 猜测缺失事务内容。

## 10. Explicit non-scope

Package 3 不实现：

- Quest engine / quest giver / quest reward；
- 任务关键词识别器、重要度打分或自动分类；
- hidden World objective viewer；
- Organization/Faction surface；
- Inventory；
- System Surface；
- full player-facing Consequence Diff；
- generic Action Intent；
- Internal Dynamic UI Host；
- Context Orchestrator / long-session memory platform；
- 历史事务时间线 / Shared History；
- 玩家手动 hide/edit（已批准的通用隐藏权仍后置 Package 6）。

## 11. Independent Review principle

Review 必须重点防止两个方向的架构回退：

1. **Program 重新接管开放语义**：出现关键词、Quest 类型表、importance score、规则树、回合阈值或基于文案的自动完成逻辑；
2. **Open Threads 变成第二事实源**：为了事务栏读取 raw World/NPC 私密状态、独立维护任务真相，或让 UI 本地猜测 currentness。

正确结果应保持：

> **模型自由决定“什么还值得继续记住”；程序只保证这个决定被有界、可逆、当前地保存和呈现。**
